# Modes System (General / Video / Recall)

The Tonious Cognitive Architecture uses three explicit reasoning modes that determine
how the LLM processes context, which inputs are included, and how the final response
is formatted. These modes isolate behavior so that upgrades to one area (e.g., video
reflection) do not affect general chat or memory recall.

---

## 1. General Mode

**Purpose**  
Standard conversational reasoning. Handles everyday dialogue, questions, explanations,
and problem-solving.

**How it works**
- User provides a message.
- Backend retrieves a recent sliding window of chat history for the active `chat_id`.
- Prompt builder composes the final prompt:
  - System identity + safety instructions  
  - Optional recent conversation block  
  - User’s current message
- Model responds using the selected tone (Grounded or Symbolic).
- Output is logged into:
  - `chat_messages` table  
  - TOL text memory logs

**Used for**
- Normal conversation  
- Q&A  
- Analysis and problem-solving  
- Model persona behavior (only when appropriate)

---

## 2. Video Mode

**Purpose**  
Structured reflection over video content using the Trinity pipeline.

**Core inputs**
- Precomputed Trinity segments (`scenes.jsonl`)
- Visual summary  
- Key speech/environments segments  
- User directive (e.g., "one paragraph", "no timestamps")

**How it works**
- UI provides both `message` and `video_id`.
- Backend loads the stored Trinity data.
- Prompt builder constructs a video-mode prompt:
  - System instructions  
  - Optional stored video summary  
  - User directive  
  - Trinity segment text block
- Tone is mostly suppressed in video mode (kept literal).
- Output is:
  - Displayed in the UI  
  - Logged under both `chat_messages` and TOL video memory  
  - Associated with the `video_id`

**Used for**
- Paragraph reflections  
- Timeline breakdowns  
- Scene-level summaries  
- Environmental analysis  
- Multimodal recall using text-only LLMs

---

## 3. Recall Mode

**Purpose**  
Retrieve and summarize the **recent** conversation without hallucinating older content.

**How it works**
- Triggered either manually by the UI (mode="recall")  
  or automatically when the user invokes recall language (“what have we talked about”).  
- Backend loads a small window of the last few turns.
- Prompt builder formats a recall prompt:
  - System identity focused on summarization  
  - “Conversation log:” block with last N messages  
  - Clear instruction not to invent content
- Tone is forced to Grounded.
- Output is:
  - A short recap (paragraph or 3–6 bullets)
  - Logged to the TOL text memory logs

**Used for**
- Thread recap  
- Resetting context for long conversations  
- Testing memory behavior in small models (Phi-2 / 7B)

---

## Mode Interaction Rules

- Switching modes never deletes or alters past logs.
- Only Video Mode accepts `video_id`.
- Recall Mode cannot modify tone.
- General and Video modes support tone toggles (Grounded/Symbolic).
- No mode bypasses safety systems.
- If a mode creates a prompt that exceeds the model’s safe token limit,
  the backend returns an overflow warning instead of generating.

---

## Why Modes Exist

Small and medium LLMs perform dramatically better when
their context is isolated into separate reasoning tracks.  
Modes allow:
- predictable behavior  
- cleaner prompts  
- stable formatting  
- consistent performance across Phi-2, 7B, and 14B  
- model-agnostic design

This modular structure also allows future expansion (e.g., Perception Mode for
AR-glasses input) without affecting existing features.

