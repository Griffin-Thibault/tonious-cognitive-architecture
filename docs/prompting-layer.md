# Prompting Layer

The Prompting Layer is the central “router” that converts user intent, UI settings, chat history, and optional video context into the final prompt string sent to the LLM. It is fully model-agnostic, meaning every supported model receives the same structured text input regardless of size or backend. The prompting layer enforces safety, tone, and mode rules before any model call happens.

---

## 1. Responsibilities

- Combine system identity, safety rules, user message, and context into a single coherent prompt.
- Apply mode-specific formatting:
  - **General Mode** → conversational reasoning
  - **Video Mode** → perception-aligned reflection
  - **Recall Mode** → literal summary of recent messages
- Enforce token limits and produce overflow-safe responses.
- Maintain consistent formatting across all model sizes (Phi-2, 7B, 13B, larger).
- Apply grounded/symbolic tone transforms *after* generation.
- Prevent symbolic tone from activating in Video or Recall mode.
- Guarantee predictable structure so the LLM behaves reliably across sessions.

---

## 2. Inputs

The Prompting Layer receives:

- `message`: the user’s current input.
- `mode`: `"general" | "video" | "recall"`
- `tone`: `"grounded" | "symbolic"` (not used in Recall mode).
- `model_key`: selected model backend.
- `chat_id`: identifies which conversation history to load.
- `recent_history`: last N user/assistant messages (General and Recall modes).
- `video_context`: Trinity segments + summaries (Video mode only).
- `system_instructions`: Tonious identity, safety, crisis protocol.
- `directives`: constraints like “no timestamps” or “single paragraph”.

Everything is passed through a deterministic assembly pipeline.

---

## 3. Output

The layer produces:

- A **final prompt string** ready for LLM inference.
- Mode metadata for logging.
- A **token-length estimate** used to block unsafe oversized prompts.
- Optional overflow message if the prompt is too large.

---

## 4. Prompt Structure (By Mode)

### 4.1 General Mode (Normal Chat)

General mode prompts include:

1. **Core system instructions**  
   - Identity (“You are Tonious”), safety rules, crisis behavior.
2. **Optional recent conversation**  
   - A small window of USER:/TONIOUS: lines.
3. **User message**  
   - Plain natural language as the final line.
4. **Tonious turn indicator**  
   - Ensures the LLM writes only the assistant reply.

**Important behavior:**  
Grounded/Symbolic tone is applied *after* generation. The base reply remains literal.

---

### 4.2 Video Mode (Perception Reflection)

Video mode uses a strict template to avoid hallucinations:

1. **System instructions + video mode rules**
2. **Stored video summaries** (if available)
3. **User directives** (e.g. “one paragraph, no timestamps”)
4. **Segment list**  
   - Trinity outputs formatted as compact lines:
     ```
     0:00 – 0:05 — Scene: ..., Voice: ..., Environment: ...
     ```
5. **Reflection task**  
   - Two possible forms:
     - **Paragraph mode** (“Write one paragraph of 4–6 sentences…”)
     - **Structured mode** (“1. Short visual summary… 2. Moments captured…”)

**Note:**  
Symbolic tone is **disabled** in video mode to avoid metaphor injection.

---

### 4.3 Recall Mode (Summarization)

Recall prompts are the simplest:

1. **System instructions**  
   - “The user is asking for a grounded summary of the recent conversation.”
2. **Conversation log**  
   - Last 2–6 messages.
3. **Constraints**  
   - “Do not add anything not in the log.”
4. **Summary request**

**Tone controls are disabled.**  
Recall mode is always grounded.

---

## 5. Safety & Crisis Handling

Before generating a reply:

- Crisis detector scans user message for self-harm or violent ideation.
- If triggered:
  - The prompt is replaced with a **fixed crisis-safe template**.
  - Symbolic tone is forcibly disabled.
  - The assistant message is returned immediately.

After generation:

- The assistant’s text is scanned again.
- If the model outputs unsafe material, it is replaced with the crisis-safe template.

---

## 6. Token & Overflow Management

To avoid model crashes:

- The prompting layer estimates token count for:
  - System block
  - History block
  - Video segments
  - User directives
- If the estimated size exceeds the model’s context limit:
  - The request is **blocked before inference**.
  - A short overflow-safe response is returned:
    > “The message is too long. Try shortening the request.”

This prevents degenerate outputs where small models ramble or drift.

---

## 7. Post-Processing (After the LLM Replies)

After generation, the prompting layer:

1. **Strips echoed system prompts**  
   (models sometimes repeat header text)
2. **Removes duplicated video segments**
3. **Normalizes whitespace and line breaks**
4. **Applies tone transform** (Grounded or Symbolic)
5. **Logs the assistant output to the TOL memory layer**
6. **Returns final text to the UI**

---

## 8. Model-Agnostic Behaviour

The prompting layer does not depend on model architecture.

All models—Phi-2, 7B, 13B, 14B—are driven through the **same prompt formats**, meaning:

- No model-specific hacks  
- No fine-tuning required  
- Deterministic, stable behavior across upgrades  
- Easy substitution of future models (LLama 3.1, Gemma, Qwen, Mistral, etc.)

This is what makes Tonious “plug-and-play” with any backend.

---

## 9. Limitations

- Small models struggle with long video segment blocks (reasoning load).  
- If history windows are too large, coherence degrades.  
- Symbolic tone sometimes produces mild artifacts if the base reply is weak.  
- Recall mode cannot retrieve older memories without an indexing layer.

(An indexing layer is planned in **future-work.md**.)

---

## 10. Summary

The Prompting Layer is the heart of the Tonious Cognitive Architecture.  
It enforces:

- **Identity**
- **Safety**
- **Mode logic**
- **Tone transforms**
- **Predictable formatting**
- **Model-agnostic operation**

Everything the user sees—General chat, Video reflections, Recall summaries—flows through this single unified text interface.


