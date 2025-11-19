# Limitations

Tonious is an early experimental cognitive architecture. Although the system works end-to-end in practice (chat → video → moments → memory → recall), it has several important constraints across hardware, model behavior, video processing, and memory growth.

---

## 1. Hardware Constraints

Tonious currently runs on laptop-grade hardware. This imposes clear limits.

### Model Size
- Practical upper bound: **7B–14B** models on consumer GPUs or CPU-only setups.
- Larger models (30B/70B) are not supported without specialized hardware.

### Inference Speed
- CPU inference is slow, especially with long prompts (video mode, recall).
- Deep-mode reasoning significantly increases latency.

### Video Processing
- Whisper/whisperX and visual sampling scale **linearly** with video duration.
- Processing multiple videos simultaneously is not feasible on low-end hardware.
- ffmpeg + model inference can saturate the CPU for several minutes per video.

---

## 2. Context-Length & Prompt Limits

Tonious relies on prompting and symbolic memory — not fine-tuning.  
This means context windows are **hard limits**.

- Small models (2.7B–7B) have **short token windows**, restricting:
  - Length of video segments included per prompt
  - Size of “moments captured”
  - Recall mode context
  - Amount of conversation history passed into General mode

If a prompt approaches the token limit, Tonious truncates or returns an overflow message.

---

## 3. Video Mode Bottlenecks

Video mode converts Trinity segments into text (visual + speech + environment).  
This introduces several limitations.

### Segment Explosion
Even a 3–5 minute video can easily produce:
- 30–100 ASR segments  
- 30–100 visual labels  
- Dozens of ambient tags  

Small models cannot fit all of these into a single prompt.

### Overflow Risk
Longer or dense videos cause:
- Prompt overflow  
- Forced truncation  
- Loss of context  
- Reduced accuracy  

### No Hierarchical Summaries (Yet)
The system treats all segments equally.  
There is no:
- Importance scoring  
- Redundancy merging  
- Chapter-level summarization  

Future work will address this.

---

## 4. Recall Mode Limitations

Recall mode is intentionally simple.  
It only retrieves the **last few** dialogue turns (a short sliding window).

Limitations:
- No semantic search  
- No vector database  
- Cannot retrieve older but relevant messages  
- Not a memory engine — only a short contextual recapper

Future improvements may include embedding-based retrieval or TOL graph queries.

---

## 5. Tree-of-Life Memory Growth

The TOL memory layer is append-only.  
It produces large Markdown + JSONL artifacts over time.

### Issues
- No indexing  
- No graph traversal tools  
- No deduplication  
- Disk usage grows linearly  
- File-based I/O becomes heavy at scale

This will eventually require a graph-database backend.

---

## 6. No Native Multimodal LLM Support

Tonious converts every video/audio/visual input into text first.  
Limitations:

- Cannot use native multimodal image/audio tokenizers  
- Loses fine-grained detail  
- Quality depends on preprocessing accuracy (ASR, frames, ambient detection)

True multimodal support would require a new backend adapter.

---

## 7. Model Variability

Because Tonious is model-agnostic:

- Different models behave differently with the same prompts  
- Small models are more sensitive to symbolic transforms  
- Depth profiles are heuristic  
- Base model quirks sometimes leak into style or recall behavior

There are no per-model optimizations yet.

---

## 8. No Real-Time Processing

The entire pipeline is **offline**.

- No real-time vision  
- No real-time ASR  
- No live memory formation  
- No streaming of Trinity moments  

Everything runs sequentially after upload.

---

# Summary

Tonious is functional but deliberately lean:

- Runs on modest hardware  
- Uses small/medium LLMs  
- Transforms video into text  
- Uses symbolic memory instead of fine-tuning  
- Stores everything in an append-only Tree-of-Life memory layer  

These constraints shape the current system — and define the future opportunities for expansion.

