---
layout: single
title: "Distill: The privacy-first transcription tool that runs entirely on your laptop"
category: [posts]
tags: distill transcription ai privacy local-first whisper diarization cli
date: 2026-04-06 10:00:00
excerpt: "Every week, hours of valuable conversations vanish into black holes of cloud services. Distill breaks the tradeoff between convenience and privacy by running entirely locally — transcription, speaker diarization, and protocol generation, all on your machine."
hidden: true
sitemap: false
---

Every week, hours of valuable conversations vanish into black holes of cloud services. We record meetings on Zoom, phone calls on our phones, academic discussions, project planning sessions — and then we ship them off to Otter.ai, Rev.com, or Google Meet transcription. Sure, it's convenient. But your sensitive conversations? Gone to the cloud.

{: style="text-align: justify;"}

For researchers, academics, and anyone dealing with confidential discussions, this is a non-starter. So I built something different: **Distill**. A command-line tool that transcribes, identifies speakers, and generates structured meeting protocols — entirely on your laptop, with no cloud dependencies for the heavy lifting.

{: style="text-align: justify;"}

This post is about how Distill works, why local-first matters, and how you can set it up to automate your own meeting transcription workflow. If you want to skip the motivation and jump straight to installation, scroll down.

{: style="text-align: justify;"}

---

## What is Distill?

Distill is a CLI tool that takes audio or video files and produces:

1. **Diarized transcripts** — who said what, with timestamps
2. **Structured protocols** — summaries, action items, decisions, open questions
3. **Persistent speaker recognition** — once you label "Alice", Distill recognizes her voice in future recordings

{: style="text-align: justify;"}

It's **heavily inspired by** [pasrom/meeting-transcriber](https://github.com/pasrom/meeting-transcriber), but with key differences:

- **CLI-first design** — built for automation, not just manual processing
- **App-agnostic** — works with recordings from *any* source (phone, watch, Zoom, OBS, whatever)
- **Syncthing integration** — drop files in a synced folder, Distill processes them automatically
- **Flexible LLM backend** — you choose the model (Ollama, llama.cpp, LM Studio, vLLM)
- **Small model friendly** — designed to work with models like Gemma 3 4B on consumer hardware

{: style="text-align: justify;"}

The magic is in the workflow: Record from any app, use Syncthing's robust syncing to drop audio in a folder on your desktop, a machine listens for new files and processes them automatically, outputting protocols to an archive folder where they can be picked up by other tools for further processing.

{: style="text-align: justify;"}

In my own setup, a Hermes agent grabs the protocols and merges them with a knowledge base it maintains.

---

## The Privacy Story

As a professor, I deal with confidential research discussions, student meetings, and sensitive conversations that can't go to cloud services. Most "local" AI tools still require massive models or cloud APIs — Distill had to be clever to work around this.

{: style="text-align: justify;"}

The approach is **agentic design with small models**:

- **Whisper** for transcription (tiny through large-v3, all local)
- **Pyannote** for speaker diarization (runs locally with GPU acceleration)
- **Tiny LLMs** (<5B parameters) for protocol generation via local endpoints like Ollama

{: style="text-align: justify;"}

No audio ever leaves your machine. Speaker databases are stored locally at `~/.distill/speakers.json`. You control the LLM endpoint. The code is open source — audit it yourself.

---

## How It Works (Technical Deep-Dive)

### The Pipeline

```
Audio/video file
       ↓
   ffmpeg          → convert to 16kHz mono WAV
       ↓
   faster-whisper  → transcription (Linux/CUDA) 
   OR
   mlx-whisper     → transcription (macOS/Apple Silicon)
       ↓
   pyannote.audio  → speaker diarization (who spoke when)
       ↓
   speaker DB      → match voices against known speakers (cosine similarity)
       ↓
   local LLM       → protocol generation (summary, action items, decisions)
       ↓
   .txt + .md      → clean, structured output
```

{: style="text-align: justify;"}

### Smart Engineering Tricks

**GPU orchestration:** The `run-distill.sh` script intelligently manages GPU resources. It stops the LLM server, runs Whisper and diarization (which need the GPU), then restarts the LLM for protocol generation. No manual juggling required.

{: style="text-align: justify;"}

**Caching:** Diarization results are cached to `~/.distill/cache/` to avoid reprocessing files you've already analyzed.

{: style="text-align: justify;"}

**Deduplication:** Server mode detects identical uploads by hashing, preventing duplicate processing.

{: style="text-align: justify;"}

**Speaker persistence:** Distill stores up to 5 voice embeddings per speaker and uses cosine similarity (threshold: 0.82) to match voices across recordings. Once you label "Alice" once, she's recognized forever.

{: style="text-align: justify;"}

**Agent-friendly CLI:** Designed for automation with tools like Telegram bots for speaker labeling. The bot sends audio excerpts, you reply with names, Distill updates the speaker database.

---

## Real-World Usage: My Workflow

Here's how I use Distill daily:

1. **Record** meetings on my phone, watch, or laptop (whatever's convenient)
2. **Sync** via Syncthing — the default recording folder syncs to my desktop
3. **Process** — Distill automatically picks up new files and transcribes them
4. **Label** — Telegram bot helps identify speakers via audio samples on first occurrence
5. **Output** — Professional meeting protocols in Markdown, ready for my knowledge base

{: style="text-align: justify;"}

The output includes:

- **Summary** — what the meeting was about
- **Participants** — who was there
- **Topics Discussed** — main discussion points
- **Decisions Made** — concrete outcomes
- **Action Items** — who needs to do what
- **Open Questions** — unresolved items

{: style="text-align: justify;"}

All local. All private. All automated.

---

## Installation & Quick Start

### Prerequisites

- Python 3.9+
- A GPU (NVIDIA with CUDA for Linux, or Apple Silicon for macOS)
- Hugging Face token (free, required for diarization models)

{: style="text-align: justify;"}

### Install

```bash
git clone https://github.com/ilyasst/distill
cd distill
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[linux]"  # or ".[macos]"
```

{: style="text-align: justify;"}

### Set Your Hugging Face Token

```bash
export HF_TOKEN=your_token_here
# Or add to ~/.bashrc or ~/.zshrc for persistence
```

{: style="text-align: justify;"}

### Process a Meeting

```bash
distill process meeting.wav \
  --llm-endpoint http://localhost:8080/v1 \
  --llm-model gemma3:4b \
  --output ./results
```

{: style="text-align: justify;"}

This will produce:

- `meeting_diarized.txt` — full transcript with speaker labels and timestamps
- `meeting_protocol.md` — structured protocol with summary, decisions, action items

---

## Why This Matters

### Data Sovereignty

Your conversations stay on your devices. No third-party access, no data mining, no privacy policy fine print. You own your data.

{: style="text-align: justify;"}

### Cost

Free after initial setup. No per-minute charges, no subscription fees, no API limits. Process as many hours of meetings as you want.

{: style="text-align: justify;"}

### Flexibility

Works offline. Customizable. Open source. You can modify it to fit your exact needs.

{: style="text-align: justify;"}

### Innovation

Distill proves you don't need massive models for practical AI tools. Small models, clever engineering, and local-first design can deliver real value without sacrificing privacy.

{: style="text-align: justify;"}

---

## Limitations (Being Honest)

- **Requires setup** — not a one-click install. You need to configure Python, dependencies, and optionally an LLM endpoint.
- **Diarization needs HF token** — free, but you need to get one from Hugging Face.
- **Processing speed depends on hardware** — a 1-hour meeting might take 5-15 minutes to process depending on your GPU.
- **Small Whisper models may struggle** with noisy recordings or heavy accents. Upgrade to `medium` or `large-v3` if accuracy is critical.

{: style="text-align: justify;"}

---

## Call to Action

**Try it:** [GitHub repository](https://github.com/ilyasst/distill)

{: style="text-align: justify;"}

**Contribute:** It's open source. Found a bug? Want a feature? Submit a PR.

{: style="text-align: justify;"}

**Share your workflow:** How are you using Distill? I'd love to hear how you've integrated it into your own setup.

{: style="text-align: justify;"}

---

## Final Thoughts

Distill started as a personal tool to solve my own problem: transcribing confidential meetings without sending them to the cloud. It evolved into something more general-purpose, but the core philosophy never changed — **privacy and functionality aren't mutually exclusive**.

{: style="text-align: justify;"}

It's a working example of practical, local-first AI that respects user autonomy. And it runs on my laptop, which is the kind of constraint that forces good engineering.

{: style="text-align: justify;"}

If you're tired of cloud-based transcription services and want something that respects your privacy while actually working, give Distill a try.

---

*Thanks for reading. If you found this useful, share it with someone who needs local-first tools.*
