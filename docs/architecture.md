# Tonious Cognitive Architecture — Technical Overview

This document describes the internal architecture of the Tonious Cognitive Architecture.  
It focuses on the flow of data, the behavior of the major subsystems, and the model-agnostic design choices that allow the entire stack to run on Phi-2, 7B models, 14B models, or larger.

---

## 1. High-Level Overview

Tonious ingests multimodal inputs—video files (converted into Trinity “moments”), text chat messages, and structured conversation history—and routes them through a single LLM reasoning core.  
All significant outputs are written into two parallel storage systems:

- a **relational store** for chat/video metadata  
- a **symbolic Tree-of-Life (TOL) memory graph** for durable, model-agnostic long-term recall  

Video is converted into textual “moments,” text chat is wrapped into structured prompts, and the LLM’s outputs are mirrored into TOL triplets and markdown memory files.

---

## 2. Major Components

### 2.1 Trinity Pipeline (video → moments → summary)

**Responsibilities**
- Probe, ingest, and normalize video metadata (duration, fps, codec, ambient metrics).
- Extract ASR, visual descriptions, and environment audio into a unified “Trinity” representation.
- Produce short summaries and moment-level descriptions used by downstream prompts and memory.

**Inputs**
- Raw video file  
- Video metadata (ffprobe)  
- Pipeline configuration  

**Outputs**
- `videos` DB row (duration, env stats, visual info)
- Scene stream: `08_YESOD/streams/video_<id>_scenes.jsonl`
- TOL memory files:  
  `S05_HARMONY_TIFERET/memories/video_<id>.md`  
  plus JSONL beat streams (`*_tol.jsonl`)

---

### 2.2 Prompting / Control Layer (“ask-tonious” router)

**Responsibilities**
- Build LLM prompts from system instructions, mode, tone, chat history, and video context.
- Enforce context-length limits and crisis detection.
- Route to the correct model tier and depth profile.

**Inputs**
- User message  
- Mode (general / video / recall)  
- Tone (grounded / symbolic)  
- Model key (phi2, tonious-7b, tonious-14b…)  
- Recent chat history  
- Optional video context  

**Outputs**
- Final prompt string  
- Normalized LLM reply  
- Logged message row + TOL entries  

---

### 2.3 Mode System (General / Video / Recall)

**Responsibilities**
- Select correct prompt template and context strategy.
- Decide whether to include chat history or video segments.

**Outputs**
- Mode-specific prompt:

**General:** system + recent chat + current message  
**Video:** stored video summary + Trinity segments + directive  
**Recall:** recent small window of conversation summarized  

---

### 2.4 Tone System (Grounded vs Symbolic)

**Responsibilities**
- Apply deterministic style transforms without modifying underlying meaning.
- Enforce strict grounded tone in video/recall modes.

**Outputs**
- Grounded: concise, literal  
- Symbolic: structured mythic-style prefixing (with safeguards)

---

### 2.5 Tree-of-Life Memory Layer (Symbolic Graph)

**Responsibilities**
- Persist all significant events (text + video beats) as symbolic TOL nodes.
- Provide a model-neutral memory representation enabling recall across all model sizes.

**Inputs**
- Chat messages  
- Video Trinity segments  
- LLM summaries  

**Outputs**
- JSONL logs: `08_YESOD/logs/text_*`  
- Video memory markdowns  
- TOL triplet streams (`*_tol.jsonl`)  
- Global indexes (`05_TIFERET/daat_index.json`)  

---

### 2.6 Model Adapter (Uniform LLM Backend)

**Responsibilities**
- Hide model-specific quirks behind a unified text-completion function.
- Manage device selection, dtype, and depth profiles.

**Inputs**
- `model_key`, prompt, generation params  

**Outputs**
- Completion text  
- Timing/latency metrics  

---

### 2.7 UI Layer

**Responsibilities**
- Present chat, mode/tone selectors, and video cards.
- Translate user actions into backend API calls.

**Outputs**
- REST API calls to `/api/ask_tonious`, `/videos`, and TOL endpoints  
- UI state for active model/tone/mode selections  

---

## 3. Data Flow

### 3.1 General Mode (Chat)

1. User submits a message with mode=`general` and tone/model options.  
2. UI sends POST `/api/ask_tonious` with message + last ~10 chat bubbles.  
3. Backend logs user message.  
4. Backend retrieves recent chat context.  
5. Prompting layer constructs:

   ```
   System  
   Optional recent chat  
   User message  
   Tonious: 
   ```

6. Token guard checks size; overflow returns a short error.  
7. Model adapter loads/uses the chosen model and performs generation.  
8. Crisis detector sanitizes unsafe content.  
9. Tone engine applies grounded/symbolic transform.  
10. Backend logs assistant reply + TOL entries.  
11. UI displays reply and updates “last model/latency.”

---

### 3.2 Video Mode (Stream → Moments → Reflection)

1. User uploads video → POST `/videos`.  
2. Ingest stores MP4, metadata, and rejects >5 min videos.  
3. ASR, environment analysis, and visual sampling run automatically.  
4. Trinity pipeline fuses all into time-stamped moments.  
5. TOL memory entries + markdown summary are written.  
6. User switches chat to mode=`video` with `video_id`.  
7. Backend fetches:

   - visual summary  
   - key moments  
   - Trinity segments  

8. Prompting layer builds a structured video prompt:

   ```
   Video context
   Instructions (if any)
   Segments:
     <start–end> Scene: … Voice: … Environment: …
   ```

9. LLM produces a literal reflection (tone transform mostly bypassed).  
10. Reply is logged and displayed.

---

### 3.3 Recall Mode

1. Triggered by explicit mode selection or phrases like  
   **“summarize what we talked about.”**  
2. Backend logs the recall question.  
3. Backend pulls only the last few user/assistant messages.  
4. Prompt template:

   ```
   You are Tonious. Summarize the recent conversation...
   Conversation log:
   USER: …
   TONIOUS: …
   ```

5. LLM returns a concise grounded summary.  
6. Logged and shown without tone transform.

---

## 4. Model-Agnostic Design

Tonious treats the LLM as a black-box function:

```
generate(model_key, prompt, max_new_tokens, temperature, top_p) → text
```

**Assumptions**
- Model accepts a single text prompt.  
- Supports basic generation params.  
- Produces deterministic structure with stable prompting.

**Swapping models (7B → 70B) changes only:**
- Registry entry (name, dtype, token limits)  
- Device/memory configuration  
- Optional depth-profile tuning  

**Does NOT change:**
- Prompt templates  
- Tone system  
- Mode behavior  
- Video Trinity structure  
- TOL memory schema  

All memory and Trinity structures are text-based, so any future model can interpret them.

---

## 5. Scaling Considerations

### Longer Videos
- Hard-capped at 5 minutes for latency reasons.  
- Bottlenecks: ASR cost, visual sampling, long prompts.  
- Future: hierarchical summaries, segment thinning, distributed workers.

### Long Conversations
- Router limits to small context windows.  
- Recall degrades for very long threads.  
- Future: semantic retrieval or periodic compression summaries.

### Memory Growth (TOL)
- TOL is append-only JSONL + markdown.  
- Scales linearly but lacks indexing.  
- Future: migrate to a graph/document store with compaction jobs.

### Practical Limits
- Context window per model (Phi-2 ≈ 2.6k tokens).  
- CPU/GPU latency on long prompts.  
- Heavy I/O from constant logging.

---

## 6. Open Questions / Current Limitations

- Video prompts can overflow; no importance-based segment reduction yet.  
- Recall relies on phrase matching + small sliding window.  
- Symbolic style transform can feel constrained on small models.  
- Depth profiles are heuristic and not per-model optimized.  
- TOL lacks a true query language or cross-video reasoning tools.  
- Entire video pipeline converts to text before LLM—no native end-to-end multimodality.

---

This document provides a complete architectural blueprint for understanding or re-implementing the Tonious Cognitive Architecture without access to the original source code.

