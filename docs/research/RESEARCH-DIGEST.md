# LearnFlow AI - Research Digest

> Distilled from initial research phase. Signal only, no fluff.

---

## High-Value GitHub Repos

### Tier 1: Best Starting Points

| Repo | What It Does | Why It Matters | Gap to Fill |
|------|--------------|----------------|-------------|
| **[StudentTraineeCenter/edu-agent](https://github.com/StudentTraineeCenter/edu-agent)** | LangGraph study platform: quizzes, flashcards, mind maps, adaptive study plans, proactive tutor | Already built around active recall + performance thresholds (<70% = weak spot) | YouTube ingestion, trust/validate modes, lossless distillation |
| **[A-R007/Multi-Agent-Study-Assistant](https://github.com/A-R007/Multi-Agent-Study-Assistant)** | 6-agent system: roadmaps, quizzes, tutoring, doc Q&A, learning-style adaptation | Clean multi-agent architecture separation | Persistent memory, YouTube pipeline, fact-check agent |
| **[NirDiamant/GenAI_Agents](https://github.com/NirDiamant/GenAI_Agents)** | LangGraph multi-agent patterns including ATLAS (academic planning) and Chiron (learning agent) | Best reference implementation library for agentic teaching loops | Turn patterns into product: persistence, UI, ingestion |

### Tier 2: Ingestion & Processing

| Repo | What It Does | Use Case |
|------|--------------|----------|
| **[Solomonkassa/Youtube-to-Doc](https://github.com/Solomonkassa/Youtube-to-Doc)** | YouTube → comprehensive doc optimized for LLM indexing | Content acquisition layer |
| **[jdepoix/youtube-transcript-api](https://github.com/jdepoix/youtube-transcript-api)** | Python transcript fetcher (auto captions + translation) | Clean "get transcript fast" primitive |
| **[rolme/ytscript](https://github.com/rolme/ytscript)** | CLI for downloading + summarizing YouTube transcripts | Quick automation substrate |
| **[whisper-timestamped](https://github.com/linto-ai/whisper-timestamped)** | Word-level timestamps + VAD for precise clip cutting | Video editing pipeline |

### Tier 3: UX & Workspace Ideas

| Repo | What It Does | Steal This |
|------|--------------|------------|
| **[ThinkEx-OSS/thinkex](https://github.com/ThinkEx-OSS/thinkex)** | AI workspace with YouTube transcript context, persistent knowledge cards | Learning OS UX patterns |
| **[shwetam19/AI-Tutor](https://github.com/shwetam19/AI-Tutor)** | Multimodal inputs, RAG retrieval, agentic web scraping, TTS | Voice interaction patterns |

---

## X/Twitter Signal (Validated)

### Tools Worth Watching

| Source | Tool/Project | Key Feature |
|--------|--------------|-------------|
| @kento_morota | **Echo-Learn** | Agentic RAG → voice-enabled study partners |
| @j_viston | **Physics It Is** | Document + DeepLake + RAG + Agent + gTTS pipeline |
| @ekelemefavour1 | **YouTube Analyst Agent** | Video → timestamps/topics/summaries |
| @omarsar0 | **LongVideoAgent** | Multi-agent for hour-long video understanding |
| @majdisaibi | **Kubra** | Personalized courses from topics |

### Search Queries (For Future Research)

```
("LangGraph" tutor quiz flashcards) filter:links since:2024-01-01
("study plan" "active recall" agent) filter:links since:2024-01-01
("YouTube" transcript agent MCP) filter:links since:2024-01-01
("spaced repetition" LLM agent) filter:links since:2024-01-01
```

---

## Technical Patterns to Adopt

### From edu-agent
- Performance thresholds for adaptive difficulty
- Proactive tutor behaviors (not just Q&A)
- Study artifact generation (quizzes, flashcards, mind maps)

### From GenAI_Agents (ATLAS/Chiron)
- LangGraph state machine patterns
- Multi-agent orchestration
- Self-improvement loops

### From video editing community
- ffmpeg EDL (edit decision list) for clip cutting
- `atempo` for audio speedup (max 2.0 per filter)
- `setpts=PTS/2` for video speedup
- Burned-in subtitles for context

---

## Key Technical Decisions

### Transcription
- **Primary**: whisper-timestamped (word-level timestamps + VAD)
- **Fallback**: youtube-transcript-api (faster, uses existing captions)

### Video Processing
- **Download**: yt-dlp (industry standard)
- **Editing**: ffmpeg (clip cutting, concatenation, speed)

### Agent Framework
- **Primary**: LangGraph (state machines, routing, memory)
- **Patterns**: Steal from GenAI_Agents repo

### Storage
- **RAG**: ChromaDB or Pinecone
- **Files**: Local + cloud backup
- **Memory**: Redis for session state

---

## Anti-Patterns to Avoid

1. **Don't build monolithic** - Keep agents specialized
2. **Don't lose timestamps** - Always preserve source citations
3. **Don't over-process** - Trust mode should be fast and lossless
4. **Don't ignore UX** - The "drip" and "re-saturate" features are the differentiator
