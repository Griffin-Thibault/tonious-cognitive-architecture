# Repo Guide

This document explains how to navigate, read, and use the Tonious Cognitive Architecture repository.  
It assumes zero prior knowledge of the codebase. The repo is intentionally **no-code**, documentation-first, and focused on the theoretical architecture that powers Tonious.

---

## 1. What This Repository Contains

This repo contains:

- The **theory**, **structure**, and **design** of the Tonious Cognitive Architecture
- Documentation describing each functional subsystem
- Visual explanations (diagrams, screenshots, UI layouts)
- A full walkthrough of the Trinity → Moments → TOL memory pipeline
- Architectural guidelines for model-agnostic LLM interaction
- Future-work notes for perception-based models and wearable devices

It **does not** contain:
- App source code  
- Model weights  
- Backend implementation  
- UI code  
- Deployment scripts  

This is by design. The repo is a **blueprint**, not a software drop.

---

## 2. Repository Structure

All documentation lives inside the `/docs` directory:

```
docs/
  overview.md
  architecture.md
  modes.md
  prompting-layer.md
  memory-graph.md
  model-agnostic-design.md
  limitations.md
  future-work.md
  interface-and-streams.md
  trinity-pipeline.md
  repository-guide.md   ← (this file)
assets/
  ui/                  (screenshots)
  diagrams/            (architectural diagrams)
LICENSE
README.md
ABOUTME.md
```

Each of these documents is self-contained and can be read independently.

---

## 3. Where to Start (New Readers)

If you are new:

1. **Read `README.md` first**  
   It explains the concept at a high level.

2. **Read `docs/overview.md`**  
   This gives you the “what, why, how” in the simplest terms.

3. **Then choose your depth:**
   - **Architecture.md** → High-level systems design  
   - **Prompting-layer.md** → How Tonious shapes LLM behavior  
   - **Modes.md** → General / Video / Recall  
   - **Trinity-pipeline.md** → How video becomes moments  
   - **Memory-graph.md** → The Tree-of-Life representation layer  

4. **Review screenshots and diagrams**  
   These make the flow easy to understand without code.

---

## 4. Who This Repository Is For

This repo is written for several types of readers:

### **Researchers**
Interested in perception-aligned LLMs, symbolic memory graphs, or non-fine-tuned behavioral control of small models.

### **Developers**
Curious about multimodal agents, structured prompting, or memory-driven LLM architectures.

### **Collaborators**
People looking to extend the system, build new modules, or help develop indexing, retrieval, or wearable integration.

### **The Curious**
Anyone who wants to understand how Tonious works internally without touching the implementation details.

---

## 5. How to Use This Repository

### **If you want to learn the system**
Simply read the docs in order. Each file is understandable without coding knowledge.

### **If you want to recreate the system**
Use each document as a blueprint. Every subsystem is described in enough detail that an engineer can rebuild it from scratch.

### **If you want to extend or contribute**
Start with:
- `future-work.md`
- `memory-graph.md`
- `model-agnostic-design.md`
These are the strongest foundation for collaboration.

---

## 6. Tips for Reading the Docs

- The system is **modular** → Most files stand alone.  
- The Memory Graph (TOL layer) is the “spine” of the architecture.  
- The Prompting Layer is the **control brain**, shaping how the model sees the world.  
- The Trinity Pipeline is the **sensory organ**, turning video into structured perception.  
- The Modes System determines Tonious’s “mindset” (general, video, recall).  

Understanding these five elements gives you the whole picture.

---

## 7. How Updates Will Work

The repository is living and will evolve.  
Planned updates include:

- Additional diagrams and UI flows
- Extended examples of Trinity moments
- More detailed symbolic and grounded tone rules
- Multimodal indexing concepts
- Wearable vision pipelines (Meta glasses integration)
- Expanded legal clarifications

All major additions will be announced in the README.

---

## 8. Licensing & Attribution

This repository is licensed under **CC BY-NC 4.0**.

This means:

- **Anyone may read, share, or build on the ideas**
- **No one may use the ideas commercially without permission**
- **All public or redistributed work must credit you (the creator)**

The LICENSE file contains the full legal text.

---

## 9. Contact


- **Reddit**: *GriffinThibault*


---

## 10. Summary

This repo is a complete theoretical blueprint of the Tonious Cognitive Architecture.  
It documents:

- The ideas  
- The structure  
- The subsystems  
- The workflows  
- The memory logic  
- The video perception pipeline  

Without revealing proprietary implementation code.

If Tonious is the machine, this repository is the **manual**.


