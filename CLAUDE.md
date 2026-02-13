# LearnFlow AI

Agentic learning system that turns URLs into personalized, multi-modal learning materials.

## Status

**Phase**: Research & Design Complete -- no source code yet. All `src/`, `config/`, `scripts/` directories are scaffolded but empty.

## Tech Stack (Planned)

| Component | Technology |
|-----------|------------|
| Orchestration | LangGraph (state machine with 6 agents) |
| LLM | Claude / GPT-4 |
| Transcription | Whisper (timestamped) |
| Video Download | yt-dlp |
| Video Editing | ffmpeg |
| TTS | ElevenLabs |
| RAG | ChromaDB |
| Storage | Local + S3 |

## Commands (When Implemented)

```bash
# Python environment
uv venv
uv pip install -e .
```

## Project Structure

```
docs/
  FEATURES.md              # Full feature spec (6 categories, prioritized)
  GOALS.md                 # MVP roadmap (5 phases), success criteria
  architecture/
    ARCHITECTURE.md        # LangGraph agent architecture, data models
  research/
    RESEARCH-DIGEST.md     # Curated findings from related projects
src/
  agents/                  # LangGraph agents (empty)
  pipelines/               # Processing pipelines (empty)
  integrations/            # External service connectors (empty)
config/                    # Configuration files (empty)
scripts/                   # CLI tools (empty)
```

## Architecture (6 Agents)

1. **Router Agent** -- Content type detection, pipeline selection
2. **Ingest Agent** -- Download, transcribe, extract metadata
3. **Distill Agent** -- Lossless signal extraction, fluff removal
4. **Teach Agent** -- Study guides, flashcards, quizzes, audio
5. **Video Editor** -- Clip scoring, extraction, concatenation, repeat mode
6. **Validate Agent** -- Claim verification, correction overlay
7. **Store Agent** -- RAG storage, session history, semantic search

## MVP Roadmap

1. **Core Pipeline** (Weeks 1-2): YouTube URL to study guide + best-of video
2. **Learning Materials** (Weeks 3-4): Flashcards, quizzes, audio, 2x speed clips
3. **Knowledge Management** (Weeks 5-6): RAG, session history, re-saturate queries
4. **Validate Mode** (Weeks 7-8): Claim verification, corrections, confidence scoring
5. **Scheduling** (Weeks 9+): Spaced repetition, notifications, performance tracking

## Conventions

- Use `uv pip` / `uv venv` for Python, never raw `pip`
- LangGraph for agent orchestration
- Trust vs Validate modes for content processing
- Lossless signal preservation -- remove fluff, keep all insights
