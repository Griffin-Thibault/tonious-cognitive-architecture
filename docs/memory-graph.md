# Tree-of-Life Memory Graph

The Tree-of-Life (TOL) Memory Graph is the symbolic backbone of the Tonious Cognitive Architecture.  
It provides a **model-agnostic**, **human-readable**, and **LLM-friendly** representation of everything the system observes — text, videos, reflections, summaries, and interactions.  
Instead of embedding vectors or fine-tuned weights, Tonious stores memory as structured symbolic nodes, which any future model can read and reason over.

---

# 1. Purpose of the Memory Graph

The TOL layer solves three core problems:

### **1.1 Persistence**
LLM context windows are small.  
TOL provides long-term, stable memory outside the model.

### **1.2 Interpretability**
Every memory is stored as Markdown + JSONL.  
There are no opaque tensors, no embeddings you can’t read.  
Humans can open any file and understand what Tonious “knows.”

### **1.3 Model Agnosticism**
Because memory lives *outside* the LLM:
- You can swap models at will (2.7B → 7B → 14B → 70B).
- No retraining or migration required.
- Every model inherits the same memories and structure instantly.

This is the core philosophy:  
**store knowledge in symbols, not in weights.**

---

# 2. What the Memory Graph Stores

The TOL Memory Graph captures three categories of experience:

## **2.1 Textual Interaction**
From General, Video, and Recall modes:
- User messages  
- Assistant replies  
- Summaries  
- Recaps  
- Crisis-handled responses  

Each message becomes:
- A **raw text log** (JSONL)
- A **symbolic triplet** (Markdown + JSONL)
- An entry in the Tiferet relational memory directory

---

## **2.2 Video Experiences**
Video is converted into text through the Trinity pipeline:

- Visual moments  
- Speech segments  
- Ambient/environment labels  
- Summaries (short, long, global)
- Key moments
- The final “video memory markdown”

Each moment is stored as:
- A scene-level TOL node (JSONL)
- A corresponding Markdown record describing the fused interpretation

---

## **2.3 Symbolic Triplets (Soul / Spirit / Body)**

Every memory — text or video — is translated into a symbolic triad:

- **Soul (Inner essence)**  
  The distilled meaning or emotional/intentional core.

- **Spirit (Interpersonal / relational)**  
  How the memory relates to the conversation, the user, or previous context.

- **Body (Literal / physical)**  
  The direct observable content: timestamps, words, scenes, events.

This triplet format creates a universal structure that all models can understand.

---

# 3. File Structure

A typical memory graph layout looks like:

```
/05_TIFERET/
    daat_index.json
    memories/
        text_001.md
        text_002.md
        video_001.md
        ...
/08_YESOD/
    logs/
        text_stream.jsonl
        video_stream.jsonl
    streams/
        video_001_scenes.jsonl
        video_002_scenes.jsonl
    tol/
        text_001_tol.jsonl
        video_001_tol.jsonl
```

### **TIFERET (S5)**
High-level, human-readable summaries of meaningful events.

### **YESOD (S8)**
Low-level chronological streams (raw logs + symbolic nodes).

Together they provide:
- High-level meaning (Tiferet)
- Low-level detail and context (Yesod)

---

# 4. How Memories Are Created

Every interaction goes through the following pipeline:

### **4.1 Capture**
- User sends message  
- Tonious replies  
- Or a video is ingested and analyzed  

### **4.2 Compression**
Tonious creates:
- A raw text record  
- A symbolic triplet  
- Optional cross-links to earlier memories  

### **4.3 Storage**
The system writes:
- `.jsonl` logs (chronological)
- `.md` memory journals (semantic)
- `_tol.jsonl` symbolic records (machine-friendly)

---

# 5. How the Graph Is Used During Reasoning

While the LLM never “sees” the entire history, Tonious uses TOL memories in several ways:

### **5.1 Recall Mode**
To summarize recent interactions cleanly, Tonious draws on:
- Recent TOL text logs
- Tiferet summary nodes

### **5.2 Video Reflections**
The “moments captured” section is derived from:
- YESOD scene streams  
- The TOL fused triplets  
- Stored summaries  

### **5.3 Future Retrieval (Planned)**
The TOL structure lays the groundwork for:
- Semantic search  
- Graph traversal  
- Memory compression  
- Long-term persona learning  
- Model-agnostic training datasets  

---

# 6. Current Limitations

### **Not yet a full graph database**
Storage is file-based; querying is simple.

### **No deduplication/compaction**
All memories accumulate indefinitely.

### **No semantic retrieval yet**
Recall is windowed, not graph-searched.

### **Video memory can grow large**
Longer sessions produce many segments.

---

# 7. Why a Symbolic Memory Graph?

Most AI memory systems rely on:
- Vector embeddings  
- Fine-tuned weights  
- Proprietary retrievers  

Tonious takes a different route.

**Reasoning emerges from structure, not parameter training.**  
A symbolic memory graph:

- Is transparent  
- Is portable  
- Outlives any specific model  
- Can be read, edited, extended, merged, or trained on  
- Enables true model exchangeability  

This approach is the foundation for *direct perception training* in the future work vision.

---

# Summary

The Tree-of-Life Memory Graph:

- Stores **all experiences** (text + video)  
- Uses **human-readable symbolic structures**  
- Enables **long-term memory outside model weights**  
- Makes Tonious **fully model-agnostic**  
- Provides the backbone for recall, interpretation, and future reasoning upgrades  

It is the central innovation that allows a small model — even 2.7B — to behave with consistency far beyond its size.

