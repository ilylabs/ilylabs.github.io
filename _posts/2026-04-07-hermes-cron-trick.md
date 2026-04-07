---
layout: single
title: "No-LLM Silent Hermes Cron Trick"
category: [posts]
tags: hermes automation cron lllm local inference optimization
date: 2026-04-07 09:00:00
excerpt: "A guerilla technique for running frequent cron-like tasks with Hermes Agent while minimizing LLM token usage on local hardware. Uses standard Linux cron for polling and injects one-time Hermes jobs only when there's actual work to report."
hidden: false
sitemap: true
---

With autonomous agents like Hermes, one of the main uses I have for them is cron-like tasks, such as "Check for new meeting recordings every 20 minutes."
Hermes handles this by spawning a simple agent you can customize to perform the job. See the [Hermes cron documentation](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/). This is what a Hermes cron job is, and that is different from the normal cron job we're used, which I'll call here a linux cron job.
{: style="text-align: justify;"}

At the end of each Hermes cron job run, that ephemeral agent sends a natural-language report to your Hermes agent, which then forwards the result through whatever gateway you're using (such as Telegram) if there is anything to report. If there is nothing to report, the ephemeral agent is expected to start its message with `[SILENT]`, which tells Hermes not to forward anything.
{: style="text-align: justify;"}

If the ephemeral write `SILENT` instead of `[SILENT]` or if the marker is not the very first thing in the message, you will get a useless cron job notification.
{: style="text-align: justify;"}

I am not a fan of this approach for two reasons:

1. When you're using Hermes with a local model running on your own hardware, like `Qwen3.5:27B`, **the `[SILENT]` behavior is pretty hit-or-miss** in my experience.
2. More importantly, if you're on **limited local hardware** and need ***frequent cron runs**, you will still need to allocate at least one model call per job which will quickly become a lot for any cron that is hourly or less. If it is a thinking model, that is going to be a big call which will immediately limits how often you can run these jobs. You can make this more viable by forcing the ephemeral agent to use a lightweight model such as `Gemma3:4b`, but it is still a call. With these simple models, in my experience, the message Hermes receives from the ephemeral agent then becomes messy most of the time and often fails to include `[SILENT]` when it should.
{: style="text-align: justify;"}

---

## The Trick

Here's the simple pattern I've been using to preserve local compute, avoid noisy silent notifications, and still keep a powerful thinking model available for the Hermes-side reporting step.
{: style="text-align: justify;"}

1. **Standard cron polling**: A traditional silent Linux cron job runs a script as frequently as you want. This costs zero LLM tokens.
2. **Silent exits**: If the script finds nothing to do, or a deterministic condition is not met, it exits silently.
3. **One-time Hermes injection**: If the condition is met, for example a new video is available, a new file appears in a folder, or a value crosses a threshold, the script processes it and dynamically builds a one-time JSON Hermes cron job payload. It then injects that payload directly into `~/.hermes/cron/jobs.json`, which triggers a Hermes cron run. Hermes then creates an ephemeral agent that interprets the data and forwards the result through the configured gateway.
{: style="text-align: justify;"}

## Example Injection Code

This is what the Python injection code looks like:

```python
import json, uuid, fcntl
from datetime import datetime, timedelta

def inject_hermes_alert(report_text):
    job_id = uuid.uuid4().hex[:12]
    now_aware = datetime.now().astimezone()
    
    # We instruct Hermes to copy our exact text 
    prompt = f"<<<BEGIN_MESSAGE>>>\n{report_text}\n<<<END_MESSAGE>>>\n\nPrompt for the ephemeral agent (how to report the data) goes here."
    
    new_job = {
        "id": job_id,
        "name": f"alert-{job_id[:6]}",
        "prompt": prompt,
        "model": "fast",
        "schedule": {"kind": "interval", "minutes": 1},
        "repeat": {"times": 1, "completed": 0}, # Run exactly ONE time!
        "enabled": True,
        "state": "scheduled",
        "created_at": now_aware.isoformat(),
        "next_run_at": (now_aware + timedelta(seconds=15)).isoformat(),
        "deliver": "origin",
        "origin": {"platform": "telegram"}
    }

    # Safely inject the job
    with open("jobs.json.lock", "w") as lock:
        fcntl.flock(lock, fcntl.LOCK_EX)
        with open("jobs.json") as f:
            data = json.load(f)
        data.setdefault("jobs", []).append(new_job)
        with open("jobs.json", "w") as f:
            json.dump(data, f, indent=2)
```

## Why This Works

That's the whole trick. With this setup, you get:

1. **Zero Wasted Compute**: Only invoke the LLM when there is an actual update to deliver.
2. **Native Delivery**: Hermes sees the injected one-time job, runs it immediately, and forwards the result through its normal gateway, for example Telegram, as if it had been running the cron all along.
3. **Clean Logs**: No more endless Telegram messages with malformed or misplaced `[SILENT]` markers that get forwarded by the gateway.
{: style="text-align: justify;"}

Know of any LLM guerilla techniques designed to get as much as possible from local LLMs while using as few resources as possible? Feel free to share, I'm always looking for more.
{: style="text-align: justify;"}
