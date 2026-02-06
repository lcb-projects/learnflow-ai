# LearnFlow AI - System Architecture

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│   (URL input, preset selection, output viewing, RAG queries)    │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                      ORCHESTRATION LAYER                        │
│                    (LangGraph State Machine)                    │
│                                                                 │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│  │ Router  │→ │ Ingest  │→ │ Distill │→ │ Teach   │           │
│  │ Agent   │  │ Agent   │  │ Agent   │  │ Agent   │           │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘           │
│       │            │            │            │                  │
│       │            │            │            ▼                  │
│       │            │            │      ┌─────────┐             │
│       │            │            └─────→│ Video   │             │
│       │            │                   │ Editor  │             │
│       │            │                   └─────────┘             │
│       │            │                        │                   │
│       │            ▼                        ▼                   │
│       │      ┌─────────┐             ┌─────────┐               │
│       └─────→│Validate │             │ Store   │               │
│              │ Agent   │             │ Agent   │               │
│              └─────────┘             └─────────┘               │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                       STORAGE LAYER                             │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   RAG DB     │  │  File Store  │  │   Session    │         │
│  │  (ChromaDB)  │  │  (Local/S3)  │  │   Memory     │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                    EXTERNAL SERVICES                            │
│                                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │ YouTube  │  │ Whisper  │  │ 11Labs   │  │ Notebook │       │
│  │ (yt-dlp) │  │   API    │  │   TTS    │  │    LM    │       │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘       │
└─────────────────────────────────────────────────────────────────┘
```

---

## Agent Specifications

### 1. Router Agent
**Purpose**: Determine content type and route to appropriate pipeline

**Inputs**:
- URL(s) or uploaded files
- User preferences/presets
- Mode selection (trust/validate)

**Outputs**:
- Content type classification
- Routing decision
- Pipeline configuration

**Logic**:
```python
def route(input):
    if is_youtube(input):
        return "video_pipeline"
    elif is_article(input):
        return "text_pipeline"
    elif is_pdf(input):
        return "document_pipeline"
    elif is_audio(input):
        return "audio_pipeline"
```

---

### 2. Ingest Agent
**Purpose**: Acquire and normalize content from any source

**Capabilities**:
- YouTube: yt-dlp download + transcript extraction
- Articles: Web scraping + content extraction
- PDFs: Text extraction + OCR if needed
- Audio: Download + prep for transcription

**Outputs**:
- Raw content (text/audio/video)
- Transcript with timestamps
- Source metadata (title, author, duration, URL)

**Data Model**:
```typescript
interface IngestedContent {
  id: string;
  sourceUrl: string;
  sourceType: "youtube" | "article" | "pdf" | "audio";
  title: string;
  author?: string;
  duration?: number;
  rawTranscript: string;
  timestampedSegments: Segment[];
  metadata: Record<string, unknown>;
  ingestedAt: Date;
}

interface Segment {
  start: number;  // seconds
  end: number;
  text: string;
  words?: Word[];  // word-level timestamps
}
```

---

### 3. Distill Agent
**Purpose**: Lossless signal extraction from raw content

**Capabilities**:
- Fluff detection and removal
- Structure preservation (examples, steps, definitions)
- Outline generation
- Citation mapping

**Signals to Preserve**:
- Definitions ("X is...", "This means...")
- Examples ("For example...", concrete scenarios)
- Steps/sequences ("First...", "Then...", numbered lists)
- Edge cases ("But if...", "Watch out for...")
- Numbers/data (statistics, measurements, dates)
- Key claims (novel insights, main arguments)

**Signals to Remove**:
- Filler ("um", "you know", "like")
- Hype ("This is amazing", "You won't believe")
- Repetitive rambling
- Off-topic tangents
- Self-promotion

**Output Model**:
```typescript
interface DistilledContent {
  id: string;
  sourceId: string;
  outline: OutlineNode[];
  extractedClaims: Claim[];
  examples: Example[];
  definitions: Definition[];
  steps: Step[];
  pitfalls: Pitfall[];
  compressionRatio: number;
}

interface Claim {
  text: string;
  sourceTimestamp: number;
  confidence: "high" | "medium" | "low";
  category: "fact" | "opinion" | "instruction";
}
```

---

### 4. Validate Agent (Optional)
**Purpose**: Verify claims against external sources

**Triggers**: User enables "Validate Mode"

**Capabilities**:
- Identify verifiable claims
- Research via web search
- Cross-reference with authoritative sources
- Flag disputed/incorrect claims
- Append corrections

**Output Model**:
```typescript
interface ValidationResult {
  claimId: string;
  originalClaim: string;
  status: "verified" | "disputed" | "corrected" | "unverifiable";
  correction?: string;
  sources: string[];
  confidence: number;
}
```

---

### 5. Teach Agent
**Purpose**: Generate learning materials from distilled content

**Output Types**:

| Type | Description | Format |
|------|-------------|--------|
| **Study Guide** | Structured document with key points | Markdown |
| **Quizzes** | Multiple choice + short answer | JSON |
| **Flashcards** | Q&A pairs for active recall | JSON (Anki-compatible) |
| **Audio Narrative** | Listening-optimized version | Text → TTS |
| **Mind Map** | Visual relationship diagram | JSON (for rendering) |

**Content-Aware Logic**:
```python
def select_output_format(content):
    if content.is_technical:
        return ["study_guide", "flashcards", "diagrams", "repeated_audio"]
    elif content.is_philosophical:
        return ["narrative_audio", "study_guide", "discussion_questions"]
    elif content.is_procedural:
        return ["step_checklist", "video_clips", "quiz"]
```

---

### 6. Video Editor Agent
**Purpose**: Extract and compile best video clips

**Capabilities**:
- Clip scoring (information density, novelty, actionability)
- Segment extraction via ffmpeg
- Speed adjustment (default 2x)
- Repetition (default 4x per clip)
- Subtitle burning
- Concatenation

**Clip Scoring Model**:
```typescript
interface ClipScore {
  segmentId: string;
  scores: {
    informationDensity: number;  // tokens per second
    novelty: number;             // vs other clips
    actionability: number;       // steps/instructions
    definitionPresence: number;  // key terms defined
    examplePresence: number;     // concrete examples
    fluffRatio: number;          // inverse score
  };
  finalScore: number;
  include: boolean;
}
```

**FFmpeg Operations**:
```bash
# Extract clip
ffmpeg -i input.mp4 -ss START -to END -c copy clip.mp4

# Speed up 2x
ffmpeg -i clip.mp4 -filter:v "setpts=0.5*PTS" -filter:a "atempo=2.0" fast.mp4

# Repeat 4x
ffmpeg -f concat -i list.txt -c copy repeated.mp4

# Burn subtitles
ffmpeg -i input.mp4 -vf subtitles=subs.srt output.mp4
```

---

### 7. Store Agent
**Purpose**: Persist and organize all learning materials

**Destinations**:
1. **RAG Database** (ChromaDB): Semantic search
2. **File System**: Structured folders
3. **Session Memory**: Recent context

**File Structure**:
```
~/.learnflow/
├── library/
│   ├── 2024-01-15_how-transformers-work/
│   │   ├── source.json           # metadata
│   │   ├── transcript.md         # raw transcript
│   │   ├── distilled.md          # processed content
│   │   ├── study-guide.md
│   │   ├── flashcards.json
│   │   ├── quiz.json
│   │   ├── audio-narrative.mp3
│   │   └── best-of.mp4
│   └── ...
├── rag/
│   └── chroma.db
└── config/
    └── preferences.json
```

---

## Data Flow

### Standard Flow (Trust Mode)
```
URL → Ingest → Distill → Teach → Store
                  ↓
            Video Editor
                  ↓
               Store
```

### Validated Flow
```
URL → Ingest → Distill → Validate → Teach → Store
                            ↓
                      (corrections)
```

---

## State Machine (LangGraph)

```python
from langgraph.graph import StateGraph

class LearnFlowState(TypedDict):
    urls: list[str]
    mode: str  # "trust" | "validate"
    preset: str
    ingested: list[IngestedContent]
    distilled: list[DistilledContent]
    validated: list[ValidationResult] | None
    outputs: dict[str, Any]
    clips: list[ClipScore] | None
    final_video: str | None

graph = StateGraph(LearnFlowState)

graph.add_node("ingest", ingest_agent)
graph.add_node("distill", distill_agent)
graph.add_node("validate", validate_agent)
graph.add_node("teach", teach_agent)
graph.add_node("edit_video", video_editor_agent)
graph.add_node("store", store_agent)

graph.add_edge("ingest", "distill")
graph.add_conditional_edges(
    "distill",
    lambda s: "validate" if s["mode"] == "validate" else "teach"
)
graph.add_edge("validate", "teach")
graph.add_edge("teach", "edit_video")
graph.add_edge("edit_video", "store")

graph.set_entry_point("ingest")
graph.set_finish_point("store")
```

---

## Technology Stack

| Layer | Technology | Reason |
|-------|------------|--------|
| **Orchestration** | LangGraph | State machines, routing, memory |
| **LLM** | Claude + GPT-4 | Multi-model for different tasks |
| **Transcription** | Whisper (timestamped) | Word-level accuracy |
| **Video Download** | yt-dlp | Industry standard |
| **Video Editing** | ffmpeg | Reliable, scriptable |
| **TTS** | ElevenLabs | High quality voices |
| **RAG** | ChromaDB | Local-first, simple |
| **Storage** | Local + S3 | Hybrid for backup |
| **UI** | TBD | CLI first, web later |
