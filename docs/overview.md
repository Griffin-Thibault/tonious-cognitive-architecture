# Overview

**Tonious Cognitive Architecture** is a model-agnostic, perception-driven reasoning framework that transforms raw human experience (video, speech, environment, and text) into structured symbolic memory. Unlike traditional prompt wrappers, Tonious fuses *multimodal video understanding*, *structured prompting*, and a *Tree-of-Life memory graph* into a unified system that any language model—small or large—can operate inside.

The system is designed to be:

- **Model-agnostic** (works with Phi-2, 7B, 13B, 70B, etc.)  
- **Modular** (video → Trinity → moments → memory → reasoning)  
- **Symbolic + grounded** (two-mode transformer for tone)  
- **Extensible** (future Meta-glasses / direct perception pipelines)  
- **Research-oriented** (clear interfaces, logging, reproducibility)

This document provides a high-level overview of the system’s vision, components, and workflow.

---

## 1. Core Philosophy

Tonious is not “just another chatbot.”  
It is an **architecture**:

- A way for an LLM to *observe, interpret, and remember* the world.  
- A symbolic layer (Tree-of-Life) that stores experience as structured nodes.  
- A prompting-layer that uses this memory to guide reasoning.  
- A cognitive “soul/spirit/body” hierarchy that keeps models stable and coherent.

It treats video as *first-class input*, converting it into consistent “moments,” which then drive reasoning, recall, and symbolic memory updates.

The long-term vision is training AI directly from human perception streams (e.g., wearable cameras). Tonious is the first iteration of this idea.

---

## 2. System Architecture (Bird’s-Eye View)

```
Video → Trinity Pipeline → Moments → TOL Memory → LLM Reasoning → UI
```

### **Video Input**
Any short video (≤ 5 minutes) can be uploaded.

### **Trinity Pipeline**
Every video is decomposed into:
- **Visual description**
- **Audio environment analysis**
- **Speech transcription**

These are unified into *Trinity segments*: structured “moments” with timestamp, scene, voice, and environment.

### **Moments Stream**
Each moment is turned into:
- a plain-text description
- a symbolic “beat”
- a node in the memory system

### **Tree-of-Life Memory (TOL)**
A symbolic, append-only graph:
- Stores video beats  
- Stores text interactions  
- Creates a persistent memory of user + world  
- Allows future models to use this memory  

### **Prompting Layer**
Decides how to interpret input:
- **General mode:** normal reasoning  
- **Video mode:** synthesize moments into summaries/reflections  
- **Recall mode:** generate grounded summaries of conversation  

### **Tone Transformer**
Two styles:
- **Grounded** (objective, concise)
- **Symbolic** (mythic, expressive)

### **Model Adapter**
Allows *any* model to be plugged in:
- Phi-2
- 7B
- 13B
- Larger LLMs when available

### **UI**
A clean front-end that:
- displays videos  
- shows memories  
- lets users choose tone and mode  
- interacts with Tonious  

---

## 3. Why It Matters

### **Model-Agnostic Perception Reasoning**
Tonious proves that *any* LLM can:
- understand raw video  
- reflect on real-world experience  
- build memory over time  
- maintain coherent identity  

This is normally only achievable with large proprietary models.

### **Symbolic Memory Layer**
Unlike RAG or vector DBs:
- TOL memory is *meaning-structured*, not embedding-structured.  
- It is not tied to any model  
- It is human-readable and model-readable  
- It can be re-used by future models without re-training

### **Pipeline Architecture**
Tonious separates:
- perception  
- memory  
- cognition  
in a way that resembles actual cognitive science.

### **Wearable Future**
The long-term horizon is:
- Meta glasses  
- continuous POV capture  
- real-time reasoning  
- training a model on *direct human perception*  

This architecture is built for that future.

---

## 4. Current Capabilities

Tonious already supports:

### **1. Full Video Understanding**
- scene recognition  
- speech alignment  
- ambient audio tagging  
- moment-level summaries  
- high-level reflections  
- multi-model compatibility  

### **2. Two-Tone Reasoning**
- grounded (factual, minimal)  
- symbolic (mythic, expressive)  

### **3. Memory Recall**
- recent messages  
- video moments  
- symbolic memory nodes  

### **4. General Reasoning**
- normal conversation mode  
- persona-stable replies  
- identity coherence  

---

## 5. Architecture Strengths

- Works on *small models* (Phi-2, 7B)
- Runs locally or cloud
- Full pipeline is transparent  
- Transparent memory  
- Easily extended by others  
- Each subsystem is documented and inspectable  

---

## 6. Future Directions

Tonious is designed as a foundation for:

- **Direct perceptual training** (human POV → model)  
- **Hierarchical cognition** (multiple cooperating models)  
- **Indexed recall system**  
- **Long-video processing (30+ minutes)**  
- **Real-time wearable inference**  
- **AI assistants that perceive the world like humans do**

---

## 7. Why This Repo Exists

This repository exists to:

- document the architecture  
- share the ideas  
- allow researchers to explore the design  
- allow builders to integrate the memory system  
- preserve the theoretical foundation  

The core intellectual value lies in the architecture itself.

---

## 8. Contact

For discussions, collaboration, or academic/commercial inquiries:

- **Creator:** *Shaiyaoh*  
- **Email:** *griffin.thibault@gmail.com*  
- **GitHub:** *Griffin-Thibault*

Feel free to open Issues or Discussions.

---

## 9. License

This project is released under **CC BY-NC 4.0**, meaning:

- you **must credit the creator**  
- you **cannot use it commercially**  
- you **can build on it** non-commercially  

See the `/LICENSE` file for details.


