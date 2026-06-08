---
layout: single
title: "Walking on Spheres: A Monte Carlo Method That Solves PDEs by Taking Random Walks"
date: 2026-06-06
author: "Ilyass Tabiai"
categories: [Computational Methods, Monte Carlo]
tags: [Monte Carlo, PDE, Numerical Methods, Walk-on-Spheres, GPU Computing]
excerpt: "What if you could solve partial differential equations by literally taking random walks? The Walk-on-Spheres Monte Carlo method does exactly that — and it's faster, more elegant, and scales to GPUs in ways finite element methods never could."
draft: false
---

## The Problem with Traditional PDE Solvers

When you need to solve a partial differential equation, whether it's **heat diffusion**, electrostatics, fluid flow, or stress analysis, the standard approach is to **discretize the entire domain**. Finite element methods (FEM), finite difference methods (FDM), and their cousins break the problem into millions of grid points or elements, then solve a massive linear system.

This works. But it has two fundamental limitations:

1. **You pay for the whole domain**: even if you only care about the solution at a few points, this means that you have to solve the whole mesh
2. **Parallelization is awkward**: domain decomposition helps, but communication overhead and load balancing are non-trivial

What if there was a better way?

![Monte Carlo method for estimating π — same principles apply to PDEs](/assets/wos-monte-carlo/monte_carlo_pi.png)

## The Trick: You Don't Need the Whole Domain

Here's the key realization: **If you only want to know the value of a harmonic function at a single point, you don't need to solve for every point in the domain.**

Harmonic functions, solutions to Laplace's equation ∇²u = 0, have an interesting property called the **mean value property**: the value at any point equals the average value over any sphere centered at that point.

```
u(x) = (1/|∂B|) ∫_{∂B(x,r)} u(y) dS(y)
```

Basically, the temperature at a point in a steady-state heat distribution equals the average temperature on any sphere around it. This is a mathematically rigorous property, and it leads directly to a Monte Carlo algorithm.

## The Algorithm: Walk on Spheres

The Walk-on-Spheres (WoS) algorithm is elegantly simple:

1. **Start at your query point** x₀ where you want to know u(x₀)
2. **Draw the largest sphere** centered at x₀ that fits entirely inside your domain
3. **Pick a random point** uniformly on that sphere's surface, that's your new position x₁
4. **Repeat**: draw a new sphere at x₁, jump to a random point on its surface
5. **Stop when you hit the boundary**: when the sphere gets close enough to the domain boundary, use the known boundary condition value

The final boundary value you hit is your estimate of u(x₀). Run the walk many times, average the results, and you've solved the PDE at that point.

### Why This Works

Each "hop" to a random point on the sphere surface is mathematically equivalent to applying the mean value property. The random walk **propagates boundary conditions inward** through the domain, and by the law of large numbers, the average converges to the true solution.

![Single Walk-on-Spheres step: draw largest inscribed sphere, jump to random point on surface](/assets/wos-monte-carlo/one_step.png)

No grid. No linear system. No iterative solver. Just random walks.

![Multiple random walks converging to the solution — each walk is independent, all contribute to the final average](/assets/wos-monte-carlo/many_walks.png)

## The Beauty of Monte Carlo

Monte Carlo methods have a reputation for being "slow to converge", the error typically scales as O(1/√N) where N is the number of samples. For many applications, that's unacceptable.

But for Walk-on-Spheres, this is actually a **feature**:

- **Cost scales with query points, not domain size**: Want to know the solution at 10 points or 10,000? The cost is roughly the same per point, regardless of how big your domain is
- **Trivially parallelizable**: Each random walk is independent. Throw 100,000 walks at 100,000 GPU threads and you're done.
- **Adaptive accuracy**: Need more precision at certain points? Just run more walks there
- **No mesh generation**: Complex geometries? No problem. The algorithm only needs to know "am I inside or outside?" and the distance to the boundaries.

## Beyond Laplace: Poisson Equations and Heat Equations

The basic WoS method solves Laplace's equation (∇²u = 0). But most real problems have **source terms**: heat generation, charge distributions, body forces. That's the Poisson equation: ∇²u = f.

The extension is elegant: during each sphere hop, you **accumulate a contribution from the source term** integrated over the sphere. The math gets slightly more involved, but the algorithmic structure stays the same: random walks, accumulating contributions, hitting the boundary.

## GPU Acceleration: Where WoS Really Shines

This is where Walk-on-Spheres separates itself from traditional methods.

A finite element simulation on a 10-million-element mesh? You're looking at hours on a single CPU core, or maybe minutes with careful parallelization on a cluster.

A Walk-on-Spheres simulation with 10 million random walks? **Seconds on a cheap and widely available consumer GPU.**

![FEM vs Walk-on-Spheres: Traditional methods discretize the entire domain, while WoS only computes what you actually need](/assets/wos-monte-carlo/fem_vs_wos_teaser.png)

Each warp (32 threads) on an NVIDIA GPU can run 32 independent random walks in lockstep. Modern GPUs have thousands of cores. The algorithm is **embarrassingly parallel**, no communication between walks, no load balancing, no domain decomposition headaches.

I've been working on a GPU-accelerated WoS implementation using NVIDIA Warp, and the results are striking. Problems that would take hours with FEM run in minutes. And because the method is Monte Carlo, you can even **stream results**, get a rough answer quickly, then refine it as more walks complete.

## When to Use Walk-on-Spheres

WoS isn't a silver bullet. Know when to reach for it:

**Use WoS when:**
- You need the solution at specific points, not everywhere
- Your geometry is complex (mesh generation is painful)
- You have GPU hardware available
- You need adaptive accuracy (more precision in some regions)
- The problem is high-dimensional (WoS scales better than grid methods)

**Stick with FEM/FDM when:**
- You need the solution everywhere in the domain
- You need very high precision everywhere (better than ~3-4 digits)
- You're solving eigenvalue problems
- You don't have GPU access

## The Potential

What's really exciting about Walk-on-Spheres isn't just that it's faster, it's that it **changes the computational paradigm**. Instead of "discretize everything, solve the big system," you ask "what do I actually need to know, and how can I get just that?". It's quite similar to experimental approaches, we rarely can know the temperature everywhere within a mold so a set of strategically positioned thermocouples must be positioned to only measure the temperature where we need to.

This mindset applies beyond PDEs. Monte Carlo methods are having a renaissance in machine learning, finance, and physics because they scale to problems where traditional methods break down.
The GPU revolution makes this even more relevant. We have hardware that can run billions of independent random walks in parallel. Methods like WoS that exploit this capability will only become more important.

## What's Next

I'm currently building an open-source GPU-accelerated WoS library specifically for Melt Extrusion Additive Manufacturing of polymers. The goal is to make this method accessible to anyone who needs to solve PDEs, no PhD in numerical analysis required.

Stay tuned. The code will be on GitHub soon, along with tutorials showing how to solve real problems with random walks.
