# Blog Post Plan: Distill - Your Local AI-Powered Meeting Assistant

**Issue:** PRO-144  
**Status:** Plan Created - Awaiting Approval  
**Date:** 2026-04-06

---

## Target Audience

- Privacy-conscious professionals and researchers
- Tech-savvy users who want local-first AI tools
- People tired of cloud-based transcription services
- Open-source enthusiasts

---

## Blog Post Structure

### 1. Hook: The Meeting Recording Problem

- We record countless meetings (Zoom, in-person, phone calls)
- Traditional solutions: Otter.ai, Rev.com, Google Meet transcription
- The catch: Your sensitive conversations go to the cloud
- Privacy concerns for researchers, academics, corporate users

### 2. Enter Distill: The Local-First Solution

- CLI tool that transcribes, diarizes, and summarizes audio/video
- Key differentiator: Everything runs locally except optional LLM call (you control it)
- Perfect for confidential project discussions, academic research, sensitive meetings
- **Inspired by:** https://github.com/pasrom/meeting-transcriber
- **What's different:** Turns it into a flexible CLI, recording app-agnostic process
- **The magic:** Record from ANY app, use Syncthing's robust syncing to drop audio in a folder, machine listens for new files, processes automatically, outputs protocols to archive folder or further LLM processing
- **Real integration:** Hermes agent grabs protocols and merges them with a knowledge base it maintains

### 3. How It Works

```
audio/video file
      ↓
   ffmpeg          → convert to 16kHz mono WAV
      ↓
   faster-whisper  → transcription (Linux/CUDA) or mlx-whisper (macOS)
      ↓
   pyannote.audio  → speaker diarization (who spoke when)
      ↓
   speaker DB      → match voices against known speakers
      ↓
   local LLM       → protocol generation (summary, action items, decisions)
      ↓
   .txt + .md      → clean, structured output
```

### 4. Features That Make It Special

- **Speaker Persistence**: Once you label a speaker ("Alice"), distill recognizes them across all future recordings using voice embeddings
- **Agent-Friendly Design**: Built for automation — works seamlessly with Syncthing for automatic processing
- **Flexible LLM Backend**: Works with Ollama, llama.cpp, LM Studio, vLLM — you choose your model
- **Small Model Friendly**: Designed to work with models like Gemma 3 4B, making it laptop-friendly
- **Multi-Format Support**: Handles .wav, .mp3, .m4a, .mp4, .mkv, .webm, .ogg, and more

### 5. Real-World Use Case: Professor Workflow

- Record meetings on phone/watch/laptop
- Syncthing syncs recordings to desktop
- Distill automatically processes new files
- Telegram bot helps identify speakers via audio samples
- Output: Professional meeting protocols in Markdown
- All local, all private, all automated

### 6. Installation & Quick Start

```bash
git clone https://github.com/ilyasst/distill
cd distill
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[linux]"  # or ".[macos]"

# Process a meeting
distill process meeting.wav \
  --llm-endpoint http://localhost:8080/v1 \
  --llm-model gemma3:4b \
  --output ./results
```

### 7. What You Get

- Diarized transcript (who said what, with timestamps)
- Structured protocol with:
  - Summary
  - Participants
  - Topics Discussed
  - Decisions Made
  - Action Items
  - Open Questions

### 8. Privacy & Data

- No audio ever leaves your machine
- Speaker database stored locally at ~/.distill/speakers.json
- You control the LLM endpoint
- Open source: Audit the code yourself

### 9. Limitations

- Requires some setup (not a one-click install)
- Diarization needs HuggingFace token (free, but required)
- Processing speed depends on hardware
- Small Whisper models may have lower accuracy for noisy recordings

### 10. Call to Action

- Try it: GitHub repo link (https://github.com/ilyasst/distill)
- Contribute: Open source, welcomes PRs
- Share your workflow: How are you using distill?

---

## Visual Elements to Include

- Architecture diagram (ASCII or image)
- Sample input/output comparison
- Screenshot of a generated protocol
- Terminal usage examples with colored output

---

## SEO Keywords

- local transcription
- private AI
- meeting assistant
- speaker diarization
- open source transcription
- offline AI tools
- CLI productivity

---

## Related Blog Posts to Link

- Whisper model comparisons
- Local LLM setup guides
- Privacy-first AI tools

---

## APPROVAL STATUS

- Plan created: 2026-04-06
- Plan approved: YES (2026-04-06)
- Feedback incorporated: Added pasrom/meeting-transcriber inspiration, CLI/app-agnostic emphasis, Syncthing workflow, Hermes KB integration
- Next step: Write full blog post and push as hidden post to _posts/

---

## Research Notes

**Repository explored:** ~/Repositories/ilyasst/distill/

**Key files analyzed:**
- README.md: Comprehensive documentation of the tool
- distill.py: Main Python implementation
- distill-server.py: Server component for automation
- jobs/: Contains processed meeting examples with protocols

**Key technical details discovered:**
- Uses cosine similarity for speaker matching (threshold: 0.82)
- Stores up to 5 embeddings per speaker
- Speaker IDs format: SPK_XXX (e.g., SPK_001)
- Cache directory for diarization results: ~/.distill/cache/
- Speaker database: ~/.distill/speakers.json

**Sample output format:**
- Meeting protocols include: Summary, Participants, Topics, Decisions, Action Items, Open Questions
- Full transcript with speaker labels and timestamps
- Both .txt (diarized transcript) and .md (structured protocol) outputs
