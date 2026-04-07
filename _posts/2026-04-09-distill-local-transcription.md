---
layout: single
title: "Distill: Privacy-first transcription tool that runs entirely on your laptop"
category: [posts]
tags: distill transcription ai privacy local-first whisper diarization cli
date: 2026-04-09 10:00:00
excerpt: "Every week, hours of valuable conversations vanish into black holes of cloud services. Distill breaks the tradeoff between convenience and privacy by running entirely locally — transcription, speaker diarization, and protocol generation, all on your machine."
hidden: true
sitemap: false
---

One of the hardest things for early-stage graduate students is tracking tasks from one meeting to the next. After years of mentoring, I realized I also had to track their action items and review them every meeting. It works, but it is tedious. So when LLM tools started offering automatic meeting protocols, I paid attention.

I tried tools like Otter.ai, Rev.com, Microsoft Teams transcription, and Google Meet transcription. The concern with all of them: every spoken word gets uploaded to someone else's server.

So I started testing open-source local alternatives. Transcription itself is easy to run on modest hardware today, even smartphones. The hard part is turning those raw transcripts into useful protocols (tasks, decisions, actions) with models small enough to fit on a laptop.
{: style="text-align: justify;"}

Enter **Distill**: a command-line tool that transcribes, identifies speakers (diarization), and generates structured meeting protocols entirely on your laptop.
{: style="text-align: justify;"}

---

## What is Distill?

Distill is a CLI tool that takes audio or video files and produces:

1. **Diarized transcripts** — who said what, with timestamps
2. **Structured protocols** — summaries, action items, decisions, open questions
3. **Persistent speaker recognition** — once you label a speaker, they stay recognized across future recordings

{: style="text-align: justify;"}

It's **heavily inspired by** [pasrom/meeting-transcriber](https://github.com/pasrom/meeting-transcriber), but with key differences:

- **CLI-first design**: built for automation, not just manual processing
- **Recording app-agnostic**: works with recordings from *any* source (phone, watch, Zoom, OBS, whatever)
- **Built for Syncthing**: drop files in a synced folder, Distill processes them automatically
- **Flexible LLM backend**: you choose the local model (Ollama, llama.cpp, LM Studio, vLLM)

{: style="text-align: justify;"}

The workflow was the hardest part, because a key constraint was to make it all work with lightweight models: Record from any app, sync via Syncthing to a desktop folder, let a machine pick up new files and process them automatically, then archive protocols for downstream tools.

{: style="text-align: justify;"}

In my setup, an old Macbook Air M1 with a broken display handles transcription and protocol generation, while a Hermes agent ingests the results into a knowledge base.

---

## The Privacy Part

As a professor, I deal with confidential research discussions, student meetings, and sensitive conversations that shouldn't go to cloud services. Most "local" AI tools still require massive models or cloud APIs, and actually, doing what Distill does with one of those models would be super fast and easy, the challenge was to try to find a way to make it work on models that fit on your laptop so my data remains at home.

{: style="text-align: justify;"}

The approach is **agentic design with small models**:

- **Whisper** for transcription (tiny through large-v3, all local)
- **Pyannote** for speaker diarization (runs locally with GPU acceleration)
- **Tiny LLMs** (<5B parameters) for protocol generation via local endpoints like Ollama

{: style="text-align: justify;"}

No audio ever leaves your machines. Speaker databases are stored locally at `~/.distill/speakers.json`. You control the LLM endpoint. The code is open source — audit it yourself.

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

**GPU orchestration:** Distill comes with a `run-distill.sh` script that manages GPU resources between LLM for text and for audio. It stops the LLM server, runs Whisper and diarization (which need the GPU), then restarts the LLM for protocol generation. No manual juggling required.

{: style="text-align: justify;"}

**Caching:** Diarization results are cached to `~/.distill/cache/` to avoid reprocessing files you've already analyzed.

{: style="text-align: justify;"}

**Speaker persistence:** Distill stores up to 5 voice embeddings per recording speaker and uses cosine similarity to match voices across recordings. Once you label "Alice" once, she's recognized forever in future meetings, no need to give her name again. 

{: style="text-align: justify;"}

**Agent-friendly CLI:** Designed for automation with tools like Telegram bots for speaker labeling. The bot sends audio excerpts, you reply with names, Distill updates the speaker database. I use a Hermes agent for this and the agent immediately understood how to do this and even built a skill for it.

---

## Real-World Usage: My Workflow

Here's how I use Distill daily:

1. **Record** meetings on my phone or laptop
2. **Sync** via Syncthing to my old Air M1
3. **Process**: Distill picks up new files, transcribes, diarizes, and generates protocols automatically
4. **Labels**: When needed, a Telegram bot sends audio samples for speaker identification; I reply with names
5. **Output**: Markdown protocols with summaries, participants, topics, decisions, action items, and open questions.

{: style="text-align: justify;"}

All local. All private. All automated.

---

## Why I like this

### Data Sovereignty

Your conversations stay on your devices. No third-party access, no data mining, no privacy policy fine print.

{: style="text-align: justify;"}

### Cost

Free after initial setup. No per-minute charges, no subscription fees, no API limits.

{: style="text-align: justify;"}

### Flexibility

Works offline. Customizable. Open source.

{: style="text-align: justify;"}

### Lightweight

This was actually cool to achieve. I built it with larger models (Qwen3.5:27B & Claude), and they kept insisting I use them for everything. But after figuring out how to structure requests for small models, it became clear: tiny models (the kind that run on phones) can do a lot if you ask them to do only what they know—mostly map and reduce operations. You don't need massive models for practical AI tools. Small models + clever engineering = local-first design that actually works.

{: style="text-align: justify;"}

---

## Limitations (Being Honest)

- **Requires setup**: not a one-click install. You need to configure Python, dependencies, and an LLM endpoint. But an LLM will do all of that for you without any issues.
- **Diarization needs HF token**: free, but you need to get one from Hugging Face, it's a requirement of the free and open source diarization model. Important thing is to then delete the HF token, you don't need to keep it, it's just to download the model.
- **Processing speed depends on hardware**: a 1-hour meeting might take 5-15 minutes depending on your GPU tier.
- **Small Whisper models may struggle** with noisy recordings or heavy accents. Upgrade to `medium` or `large-v3` if accuracy is critical.

{: style="text-align: justify;"}

---

## Call to Action

* Was it fully vibe-coded? Yes.
* Was it built with strong guarantees? No.
* Do I still find bugs and issues that need to be fixed? Yes.
* Does it to the job? Yes.

So...
**Contribute:** It's open source. Found a bug? Want a feature? Fix it with your favorite LLM.

{: style="text-align: justify;"}