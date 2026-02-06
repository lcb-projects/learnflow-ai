# LearnFlow AI

Agentic learning system that turns any content into personalized, multi-modal learning materials.

Drop a URL. Get a study guide, flashcards, quizzes, and a "best-of" video with only the high-signal clips.

---

## The Problem

Learning from YouTube videos and online content is slow and fragmented:

1. Find video
2. Watch entire thing (including fluff)
3. Copy URL to transcriber
4. Paste transcript to LLM
5. Wrestle with prompts
6. Copy output to audio generator
7. Wait for TTS
8. Listen
9. Take notes manually
10. Organize notes somewhere
11. Forget 80% in a week

## The Solution

```
Drop URL → Click "Learn" → Done
```

LearnFlow handles everything:
- **Ingest**: Download and transcribe with timestamps
- **Distill**: Remove fluff, preserve every insight
- **Teach**: Generate study guides, quizzes, flashcards, audio
- **Edit**: Extract best clips, repeat at 2x for memorization
- **Store**: RAG database for instant recall
- **Reinforce**: Spaced repetition keeps it in your head

---

## Features

### Content Acquisition
- YouTube URL input (batch supported)
- Article and PDF processing
- Automatic format detection

### Lossless Distillation
- Removes filler and hype
- Preserves: examples, steps, definitions, edge cases
- Timestamp citations back to source

### Multi-Modal Output
- Study guides (Markdown)
- Flashcards (Anki-compatible)
- Quizzes (auto-generated)
- Audio narratives (ElevenLabs TTS)
- Diagrams and mind maps

### Video Editing
- Extracts only high-signal clips
- Scores by: information density, novelty, actionability
- 2x speed + 4x repetition (configurable)
- Concatenates into single "best-of" video

### Knowledge Management
- RAG database (ChromaDB)
- Semantic search across all learnings
- "Re-saturate" queries: "What was that thing about X?"
- Session history and categorization

### Trust vs Validate Modes
- **Trust**: Pure distillation (fast)
- **Validate**: Research agent verifies claims, appends corrections

---

## Project Structure

```
learnflow-ai/
├── docs/
│   ├── FEATURES.md           # Full feature specification
│   ├── GOALS.md              # Roadmap and success criteria
│   ├── architecture/
│   │   └── ARCHITECTURE.md   # System design
│   └── research/
│       └── RESEARCH-DIGEST.md # Curated research findings
├── src/
│   ├── agents/               # LangGraph agents
│   ├── pipelines/            # Processing pipelines
│   └── integrations/         # External service connectors
├── config/                   # Configuration files
└── scripts/                  # CLI tools
```

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Orchestration | LangGraph |
| LLM | Claude / GPT-4 |
| Transcription | Whisper (timestamped) |
| Video Download | yt-dlp |
| Video Editing | ffmpeg |
| TTS | ElevenLabs |
| RAG | ChromaDB |
| Storage | Local + S3 |

---

## Quick Start

```bash
# Coming soon
```

---

## Documentation

- [Features](docs/FEATURES.md) - Complete feature specification
- [Goals & Roadmap](docs/GOALS.md) - MVP definition and success criteria
- [Architecture](docs/architecture/ARCHITECTURE.md) - System design and data models
- [Research](docs/research/RESEARCH-DIGEST.md) - Curated findings from related projects

---

## Status

**Phase**: Research & Design Complete

**Next**: MVP 1 implementation (core pipeline)

---

## Related Projects

Key repos that informed this design:
- [StudentTraineeCenter/edu-agent](https://github.com/StudentTraineeCenter/edu-agent) - Adaptive study platform
- [NirDiamant/GenAI_Agents](https://github.com/NirDiamant/GenAI_Agents) - LangGraph patterns
- [jdepoix/youtube-transcript-api](https://github.com/jdepoix/youtube-transcript-api) - Transcript extraction
