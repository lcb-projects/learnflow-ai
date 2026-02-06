# LearnFlow AI - Feature Specification

---

## Core Vision

**One-shot learning system**: Drop URLs → Get personalized, multi-modal learning materials → Track and reinforce over time.

No more: copy URL → transcribe → prompt LLM → wrestle output → convert to audio → wait → play.

Just: "Here's what I want to learn. Handle it."

---

## Feature Categories

### 1. INGEST - Content Acquisition

| Feature | Description | Priority |
|---------|-------------|----------|
| **URL Drop** | Paste YouTube URLs, article links, or any web content | P0 |
| **Multi-URL Batch** | Process multiple sources as a unified learning unit | P0 |
| **Transcript Extraction** | Get transcripts with word-level timestamps | P0 |
| **Source Metadata** | Preserve: title, author, duration, source URL, date | P0 |
| **Format Detection** | Auto-detect: video, article, PDF, podcast | P1 |
| **Upload Support** | Drag-drop files (PDF, docs, audio) | P1 |

### 2. DISTILL - Lossless Signal Extraction

| Feature | Description | Priority |
|---------|-------------|----------|
| **Lossless Compression** | Remove fluff/hype, preserve ALL signal | P0 |
| **Preserve Structure** | Keep: examples, steps, definitions, edge cases, numbers | P0 |
| **Timestamp Citations** | Every extracted point links back to source moment | P0 |
| **Outline Mapping** | Hierarchical breakdown: sections → claims → evidence | P0 |
| **Example Preservation** | Special handling to keep examples intact | P0 |
| **Pitfall Extraction** | Identify and highlight "don't do this" moments | P1 |

### 3. VALIDATE - Trust Modes

| Feature | Description | Priority |
|---------|-------------|----------|
| **Trust Mode** | Pure distillation, no external validation (fast) | P0 |
| **Validate Mode** | Research agent verifies claims, flags errors | P1 |
| **Correction Overlay** | Append updated/corrected info to original | P1 |
| **Source Reputation** | Track which sources are typically trustworthy | P2 |
| **Fact Confidence Score** | Rate claims: verified / likely / unverified / disputed | P2 |

### 4. TEACH - Adaptive Output Generation

| Feature | Description | Priority |
|---------|-------------|----------|
| **Study Guide** | Structured document with key points + citations | P0 |
| **Quizzes** | Auto-generated questions from content | P0 |
| **Flashcards** | Anki-style cards for active recall | P0 |
| **Audio Narrative** | TTS version optimized for listening (ElevenLabs) | P0 |
| **Visual Companion** | Graphs, tables, diagrams for technical content | P1 |
| **Mind Maps** | Visual relationship mapping | P1 |
| **Content-Aware Format** | Philosophical = narrative; Technical = repetition + visuals | P1 |

### 5. VIDEO - Clip Extraction & Editing

| Feature | Description | Priority |
|---------|-------------|----------|
| **Best-Of Clips** | Extract only the high-signal moments | P0 |
| **Clip Scoring** | Rank by: information density, novelty, actionability | P0 |
| **Concatenated Video** | Stitch best clips into single watchable video | P0 |
| **Repeat Mode** | Each clip repeated 3-4x for memorization | P0 |
| **Speed Control** | Default 2x with repeats (proven learning method) | P0 |
| **Burned Subtitles** | Captions burned into video for context | P1 |
| **Interleaved Repeats** | A, B, A, C, A pattern to reduce habituation | P2 |

### 6. STORE - Knowledge Management

| Feature | Description | Priority |
|---------|-------------|----------|
| **RAG Database** | All learnings queryable via semantic search | P0 |
| **Categorization** | Auto-tag and organize by topic/domain | P0 |
| **Session History** | Track what you learned and when | P0 |
| **File System Sync** | Mirror to regular filesystem for backup | P1 |
| **Re-Saturate Query** | "Help me remember that thing about X from yesterday" | P1 |
| **Cross-Reference** | Link related learnings across sessions | P2 |

### 7. REINFORCE - Spaced Repetition & Scheduling

| Feature | Description | Priority |
|---------|-------------|----------|
| **Daily Drip** | Scheduled micro-reviews pushed to you | P1 |
| **SMS/Notification** | "Click to hear 2-min recap" texts at optimal times | P1 |
| **Performance Tracking** | Quiz results inform review frequency | P1 |
| **Weak Spot Focus** | Auto-generate reviews for missed concepts | P2 |
| **Review Cut Video** | New video containing only clips you struggled with | P2 |

### 8. MODES - Tunable Parameters

| Feature | Description | Priority |
|---------|-------------|----------|
| **Preset Profiles** | Quick-select: "Technical Deep Dive", "Quick Overview", etc. | P0 |
| **Output Format Toggle** | Choose: audio only, visual only, mixed, etc. | P0 |
| **Depth Control** | Surface skim vs. comprehensive extraction | P1 |
| **Trust/Validate Toggle** | Per-source or per-session | P1 |
| **Repetition Level** | How many times to repeat clips | P1 |
| **Speed Preference** | Default playback speed | P1 |

---

## Integration Points

| Service | Purpose | Priority |
|---------|---------|----------|
| **NotebookLM** | Study guides, briefs, tests | P0 |
| **ElevenLabs (11Reader)** | High-quality TTS for audio | P0 |
| **yt-dlp** | YouTube download | P0 |
| **Whisper** | Transcription with timestamps | P0 |
| **ffmpeg** | Video editing | P0 |
| **ChromaDB/Pinecone** | RAG vector storage | P0 |
| **LangGraph** | Agent orchestration | P0 |

---

## User Flows

### Flow 1: Quick Learn (Most Common)
```
1. Drop URL(s)
2. [Optional] Select preset or tweak params
3. Click "Learn"
4. Receive: study doc + audio narrative + best-of video
5. Materials auto-saved to knowledge base
```

### Flow 2: Deep Validate
```
1. Drop URL(s)
2. Enable "Validate Mode"
3. System extracts + researches + flags
4. Receive: annotated doc with corrections
5. Review disputed claims
```

### Flow 3: Re-Saturate
```
1. Query: "What was that thing about X?"
2. System finds relevant past learning
3. Offers: "Want audio recap? Quiz? Full review?"
4. Delivers chosen format
```

### Flow 4: Scheduled Review
```
1. Receive notification at configured time
2. Click to play 2-min audio recap
3. Optional: quick quiz
4. System updates mastery scores
```

---

## Success Metrics

- **Time to learn**: URL → usable materials in < 5 minutes
- **Retention**: Quiz performance improves with spaced repetition
- **Re-access**: Can find any past learning in < 30 seconds
- **Zero friction**: No manual steps between input and output
