# Scaling Behaviour

This document explains how the Tonious Cognitive Architecture scales across longer videos, larger models, heavier memory usage, and higher user demand.  
It describes *real constraints*, *expected bottlenecks*, and *paths to stable scaling* without relying on speculation or implementation details.

---

# 1. What “Scaling” Means in Tonious

Tonious scales across four axes:

1. **Context Size**  
   – How much text the model can fit per prompt (segments, summaries, chat history).

2. **Model Size**  
   – 2.7B (Phi-2), 7B, 14B, 30B, 70B…  
   – Smaller = slower reasoning but faster latency.  
   – Larger = deeper reasoning but more memory + compute overhead.

3. **Memory Growth (Tree-of-Life)**  
   – All chat + video moments are stored as symbolic entries that grow over time.

4. **Video Length**  
   – Longer videos → more segments → higher compute costs → larger prompts.

Scaling is therefore a combination of **compute**, **memory**, and **prompt engineering limits**.

---

# 2. Scaling Constraints

## 2.1 Model Context Limits
Each model has a fixed “maximum tokens per prompt.”  
Approximate values:

- **Phi-2:** ~2,600 tokens  
- **7B models:** ~3,500–4,000 tokens  
- **14B:** ~6,000+ tokens  
- **30B+:** 8k+ depending on the quantization  

Because Tonious uses structured prompts, mode templates, and Trinity moment lists, a long video can exceed context limits quickly.

**Outcome:**  
- If too long → Tonious returns a “prompt overflow” message.

---

## 2.2 Video Length Scaling
Tonious is currently tuned for **≤ 5 minutes per upload**.

Longer videos increase:

- ASR cost (Whisper-like models scale linearly with duration)
- Visual sampling cost
- Environment audio analysis
- Trinity segment count
- Token count inside the reflection prompt

Longer videos require:
- Hierarchical summarization  
- Segment thinning  
- Chunked reflection  
- A multi-pass pipeline (e.g., “chapter → scene → moment”)

---

## 2.3 Conversation Growth
Chat history grows indefinitely, but only a **small sliding window** is used for new prompts.  

This prevents prompt-bloat but limits “long-term memory” unless:

- Summarization nodes are added  
- Semantic retrieval (future enhancement) is introduced  
- Vector indexing (future enhancement) is added  
- TOL indexing is improved  

Without those, recall remains strictly short-range.

---

## 2.4 Tree-of-Life Memory Growth
Every message and every video beat creates:

- A Markdown entry  
- A JSONL log  
- A symbolic TOL triplet

On heavy usage, this leads to:

- Large file counts  
- Slow I/O  
- Slow retrieval without indexing  
- Redundant memory nodes  

This is fine for single-user hobby use, but will bottleneck on many users.

Scaling requires:
- Database or graph-store backend  
- Node compaction (merge duplicates)  
- Pruning / summarization  
- Fast retrieval instead of file-scanning  

---

# 3. Scaling by Model Size

## 3.1 Small Models (Phi-2)
Strengths:
- Fast on CPU  
- Low memory footprint  
- Stable with structured prompts  

Weaknesses:
- Limited context length  
- Weaker reasoning  
- Struggles with long Trinity streams  
- Harder to control symbolic tone  
- No recall beyond very recent messages  

Scaling outcome:
- Good for short chats and short videos  
- Not ideal for heavy Trinity use  

---

## 3.2 Medium Models (7B)
Strengths:
- More coherent  
- Handles 3-minute videos  
- Capable of reliable recall (short window)  
- Understands Trinity segments well  

Weaknesses:
- Slower  
- Still context-limited  
- More susceptible to style drift  

Scaling outcome:
- **Best practical tier** before relying on GPUs  
- Works well with the Tonious prompting system  

---

## 3.3 Larger Models (14B–70B)
Strengths:
- Extensive context  
- Stronger reasoning  
- Accurate video reflections  
- Stable symbolic tone control  

Weaknesses:
- Requires GPU  
- Expensive  
- Long loading times  
- More prompt-provided structure required  

Scaling outcome:
- Enables long videos (10–30 min+)  
- Enables archive-long recall  
- Enables multi-video reasoning  

These models unlock “true perception alignment.”

---

# 4. Scaling the Trinity Pipeline

The Trinity pipeline produces:
- Visual description  
- Speech transcript  
- Audio environment labels  

The bottlenecks scale as follows:

| Operation | Cost Scaling | Notes |
|----------|--------------|-------|
| ASR (speech-to-text) | Linear with duration | Whisper-like models slow on CPU |
| Visual sampling | Linear with # frames | Frame rate reduction helps |
| Audio scene classification | Linear with audio duration | Lightweight but cumulative |
| Trinity segment generation | Linear with segment count | More segments = longer prompts |
| TOL memory writing | Linear with total segments | JSONL + markdown I/O |

Scaling improvements would require:
- GPU acceleration  
- Frame sampling heuristics  
- Multi-stage summarization  
- On-demand segment retrieval  

---

# 5. Scaling Across Multiple Users

Tonious is currently designed for a **single user**.

To scale for many users:

### Required:
- Database-backed memory instead of flat files  
- Asynchronous job queues  
- Rate limiting  
- Per-user TOL graphs  
- Session-level isolation  
- Video batch-processing workers  

### Without these, bottlenecks appear:
- Disk I/O slowdowns  
- Concurrent Trinity jobs piling up  
- Models constantly reloading  
- Token limit explosion due to overlapping contexts  

---

# 6. Scaling Outlook

Tonious scales surprisingly well **in concept**, because:

- The architecture is modular  
- The Trinity pipeline produces predictable text  
- The prompt templates are stable  
- The memory graph is model-agnostic  

But Tonious is also constrained by:

- Token limits  
- Model size  
- Latency  
- Hardware  
- Video duration  
- Memory-growth bloat  

To scale beyond single-user or short-video limits, the system requires:

1. **Indexing and retrieval**  
2. **Hierarchical summaries**  
3. **GPU-backed inference**  
4. **Database-backed memory**  
5. **Batch pipelines for video**  
6. **Higher context models (14B–70B)**  

---

# 7. Long-Term Scaling Path

### Stage 1 — Single User (Now)
- CPU or small GPU  
- Short videos (≤ 5 min)  
- Manual recall  
- Memory grows in files  

### Stage 2 — Power User
- GPU  
- 10–30 minute videos  
- Partial automatic memory indexing  
- 7B–14B models  

### Stage 3 — Early Collaboration
- Repository shared  
- Multi-user support  
- Database-backed TOL  
- Semantic retrieval  

### Stage 4 — Research-Grade System
- 30B–70B multimodal models  
- Wearable cameras  
- Direct perception training  
- Real-time video ingestion  
- Massive indexed TOL graph  

Tonious’s architecture already points in this direction.

---

# 8. Summary

Tonious scales as far as your hardware and token limits allow.  
The core bottlenecks are:

- Long videos  
- Growing memory  
- Prompt size  
- Model context  
- CPU latency  
- I/O overhead  

But the design is fundamentally **model-agnostic**, **future-proof**, and ideal for:

- Larger models  
- Faster hardware  
- Better memory indexing  
- Collaborative development  
- Wearable-based perception systems  

This document should help researchers understand how to extend, accelerate, or industrialize the system.

