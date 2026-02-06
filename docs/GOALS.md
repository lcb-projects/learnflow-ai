# LearnFlow AI - Goals & Roadmap

---

## Primary Goal

**Eliminate friction between "I want to learn this" and "I've learned this".**

Current reality:
```
Copy URL → Transcriber → LLM → Prompt wrestling → Format conversion →
Audio generation → Wait → Play → Take notes → Organize notes → Forget half of it
```

Target reality:
```
Drop URL → Click "Learn" → Done
```

---

## Core Principles

### 1. Lossless Signal Preservation
Every insight, example, step, and nuance from the source material must be preserved. Remove fluff, keep everything else. When in doubt, keep it.

### 2. Multi-Modal Learning
Different content types need different delivery methods:
- Technical → Visual diagrams + repetition
- Philosophical → Narrative audio
- Procedural → Step checklists + video clips

### 3. Active Recall Over Passive Consumption
Generate quizzes, flashcards, and spaced repetition schedules. Don't just serve content—teach it.

### 4. Trust vs Validate
Not all sources are equal. Trusted sources get pure distillation. Unknown sources get fact-checked. User controls the dial.

### 5. Retrieval is Learning
If you can't find what you learned, you didn't learn it. RAG database + semantic search + "re-saturate" queries.

---

## MVP Definition

### MVP 1: Core Pipeline (Week 1-2)
**Input**: Single YouTube URL
**Output**:
- `study-guide.md` - Structured distillation
- `best-of.mp4` - Clips concatenated (1x speed)

**Scope**:
- yt-dlp download
- Whisper transcription with timestamps
- Basic distillation prompt
- Clip scoring (simple heuristics)
- ffmpeg concatenation

### MVP 2: Learning Materials (Week 3-4)
**Additions**:
- Flashcards (Anki-compatible JSON)
- Quiz questions
- Audio narrative (ElevenLabs TTS)
- 2x speed + repeat mode for clips

### MVP 3: Knowledge Management (Week 5-6)
**Additions**:
- RAG storage (ChromaDB)
- Session history
- "Re-saturate" queries
- Category tagging

### MVP 4: Validate Mode (Week 7-8)
**Additions**:
- Claim identification
- External research agent
- Correction overlay
- Confidence scoring

### MVP 5: Scheduling & Drip (Week 9+)
**Additions**:
- Spaced repetition scheduler
- SMS/notification delivery
- Performance tracking
- Review cuts

---

## Success Criteria

### Speed
| Metric | Target |
|--------|--------|
| URL to study guide | < 3 minutes |
| URL to best-of video | < 5 minutes |
| Query to past learning | < 10 seconds |

### Quality
| Metric | Target |
|--------|--------|
| Signal preservation | 100% of key insights retained |
| Fluff removal | > 40% compression on typical YouTube |
| Clip accuracy | Timestamps within 1 second |

### Retention
| Metric | Target |
|--------|--------|
| Quiz improvement | +20% on repeated quiz |
| Content recall | Can explain topic after 1 week |
| Findability | Any past learning queryable |

---

## Non-Goals (For Now)

- Real-time streaming/live video
- Social features
- Mobile app
- Multi-language (English first)
- Paid content integration (Netflix, courses)

---

## Open Questions

1. **UI Priority**: CLI-first or web-first?
2. **Model Selection**: Single model or multi-model routing?
3. **Local vs Cloud**: Full local processing or cloud APIs?
4. **NotebookLM Integration**: Automated or manual handoff?
5. **Repetition Pattern**: Fixed 4x or adaptive based on content?

---

## Dependencies & Risks

### Technical Risks
| Risk | Mitigation |
|------|------------|
| Whisper hallucination | VAD + confidence thresholds |
| Clip scoring quality | Iterative prompt tuning + human feedback |
| Video processing speed | Parallel ffmpeg, pre-computed |
| RAG retrieval quality | Hybrid search, metadata filtering |

### External Dependencies
| Dependency | Fallback |
|------------|----------|
| YouTube access | yt-dlp alternatives, manual upload |
| ElevenLabs API | Local TTS (lower quality) |
| Whisper API | Local Whisper (slower) |

---

## Inspiration Sources

- **NotebookLM**: Study guide generation, audio overviews
- **Anki**: Spaced repetition algorithms
- **Readwise**: Highlight surfacing and review
- **Blinkist**: Audio-first learning
- **Superhuman**: Speed and keyboard-first UX
