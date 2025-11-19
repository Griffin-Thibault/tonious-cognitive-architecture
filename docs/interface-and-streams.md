# Interface & Streams

This document explains how the Tonious system exposes its functionality through the UI, and how every interaction flows into the underlying *streams* that the cognitive backend processes.  
Even without seeing the internal code, this provides a complete mental model of how the interface and data pipelines (the “streams”) work end to end.

---

# 1. Overview

Tonious receives **two categories of user input**:

1. **Direct text prompts** (General, Video, Recall modes)  
2. **Video uploads** (which become multimodal “moment streams”)

Both are normalized into structured text representations and injected into the Tree-of-Life (TOL) memory layer.  
The UI doesn’t encode intelligence. It simply **steers** the cognitive core.

---

# 2. Interface Layout

The interface consists of five major zones:

1. **Model Selector (5 presets)**  
   - *Phi-2*, *Tonious-7B*, *Tonious-14B*, and additional user-defined backends  
   - Models are interchangeable; the architecture remains identical.

2. **Mode Controls**  
   - **General**: regular chat  
   - **Video**: reflection on Trinity moments  
   - **Recall**: structured summary of recent messages  

3. **Tone Controls**  
   - **Grounded** (no metaphors, literal, concise)  
   - **Symbolic** (mythic, poetic, structural)  

4. **Video Library**  
   - Thumbnails, duration, status (“uploaded → processing → ready”)  
   - Clicking a video activates it as contextual memory for the chat  

5. **Chat Panel**  
   - User input → backend request → Tonious reply  
   - Shows system state (model name, latency, safety mode, crisis lock)

These five regions map exactly onto the backend API calls.

---

# 3. The Three Primary Streams

Tonious thinks in *streams*.  
Everything you upload or type becomes one of three:

### 3.1 Text Stream (Prompts & Replies)
- Every message becomes a timestamped entry:  
  `{timestamp, role, text, mode, tone, model_key}`
- Stored under:
  ```
  08_YESOD/logs/text_<chat_id>.jsonl
  ```
- Also mirrored into a TOL-compatible format in:
  ```
  05_TIFERET/journals/chat_<chat_id>.md
  ```

This builds a durable narrative memory.

---

### 3.2 Video Stream (Trinity Moments)
When a video is uploaded, Tonious generates:

- **ASR speech segments**  
- **Visual scene descriptions**  
- **Environment/ambient labels**  

Each is fused into a *Trinity segment*:

```json
{
  "start": 0.00,
  "end": 3.20,
  "scene": "a living room with soft amber lighting",
  "voice": "Hello Tonious.",
  "environment": "low steady hum"
}
```

These are stored in:

```
08_YESOD/streams/video_<id>_scenes.jsonl
```

This is the core “moment stream” the LLM reflects on.

---

### 3.3 Memory Stream (TOL Encoding)
Each text message or video moment is translated into a TOL triplet:

- **Soul** → user intent / emotional vector  
- **Spirit** → semantic content  
- **Body** → literal text or sensory detail  

Written into:

```
S05_HARMONY_TIFERET/memories/video_<id>.md
S05_HARMONY_TIFERET/memories/text_<chat_id>.md
```

And indexed in a JSON structure for retrieval.

This is the *symbolic layer* that makes Tonious stable across models.

---

# 4. How the UI Calls the Backend

### 4.1 Text Prompts
```
POST /api/ask_tonious
{
  "message": "...",
  "chat_id": "...",
  "mode": "general|video|recall",
  "tone": "grounded|symbolic",
  "model": "phi2|tonious-7b|...",
  "context": {"video_id": "..."}
}
```

UI restricts the user from calling video mode when:
- Tonious is already “thinking”
- A video is still processing

These safeguards are handled UI-side.

---

# 5. How Streams Feed the LLM

All streams converge into the **Prompting Layer**:

### General Mode
- Small window of chat history  
- No long-term memory injection  
- Tone is applied after generation  

### Video Mode
- Trinity segments appended directly  
- Optional directive (paragraph, list, summary)  
- Tone mostly suppressed (forced literal)  

### Recall Mode
- Very small, literal subset of chat history  
- No symbolic transformations  

Each mode uses a dedicated “prompt template.”

---

# 6. Stream Persistence

Streams are never overwritten.  
They only grow.

Each video stream and text stream can later be:
- re-fed into the LLM  
- used for recall  
- included in training datasets  
- converted into TOL graphs for semantic queries  

This persistence is what creates Tonious’ long-term cognitive shape.

---

# 7. Why Streams Matter

The Streams design allows:

- **Model-agnostic cognition**  
- **Swapping models without losing memories**  
- **Consistent personality & behavior over time**  
- **Stable recall even in small models**  
- **Deterministic reasoning from purely textual state**  

This is one of the most novel architectural properties of Tonious.

---

# 8. Summary

Tonious’ interface is not cosmetic;  
it is a control panel for routing perception into structured cognitive streams.

- The UI decides *what type* of cognition is invoked.  
- The backend converts inputs into structured streams.  
- The Tree-of-Life layer stores symbolic memory for later reuse.

This division is what allows Tonious to scale across models and eventually serve as the foundation for **direct-perception AI agents**.


