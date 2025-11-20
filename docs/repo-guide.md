# Repository Guide  
A short, practical roadmap for navigating and extending **Tonious — The Model-Agnostic Cognitive Architecture**.

This guide helps new collaborators, researchers, and engineers understand how the repository is organized, what each folder contains, and how to extend or modify the system responsibly.

---

## 📁 Repository Structure

```
tonious-cognitive-architecture/
│
├── docs/                  # Formal documentation for each internal subsystem
│   ├── overview.md
│   ├── architecture.md
│   ├── prompting-layer.md
│   ├── modes.md
│   ├── memory-graph.md
│   ├── interface-and-streams.md
│   ├── limitations.md
│   ├── use-cases.md
│   ├── trinity-pipeline.md
│   ├── scaling-behaviour.md
│   ├── future-work.md
│
├── diagrams/              # Mermaid flowcharts & conceptual diagrams
│   ├── architecture.md
│   ├── llm-core-and-tone.md
│   ├── memory-recall.md
│   ├── mode-routing.md
│   ├── trinity-pipeline.md
│
├── examples/              # UI screenshots and demonstration outputs
│   ├── interface/         # UI layout & rendering
│   ├── recall/            # Recall-mode examples (chat recap)
│   ├── trinity-stream/    # Video streaming → memory examples
│   ├── video-to-moments/  # Timestamped video segmentation examples
│
├── LICENSE                # CC-BY-NC 4.0 License (strict: no commercial use)
├── README.md              # Entry-point landing page for GitHub viewers
└── ABOUTME.md             # Brief creator bio & contact links
```

---

## 🧠 How to Understand Tonious (Conceptual Order)

If you're new, explore the system in this order:

1. **docs/overview.md**  
   High-level explanation of how Tonious works as a cognitive architecture.

2. **docs/architecture.md**  
   System design philosophy + the Trinity Framework (General / Video / Recall).

3. **docs/prompting-layer.md**  
   How prompting, templates, and tone control the LLM core.

4. **docs/modes.md**  
   The three reasoning pipelines and why they are separated.

5. **docs/memory-graph.md**  
   How the Tree-of-Life memory graph stores structured symbolic memories.

6. **docs/interface-and-streams.md**  
   How UI panels correspond to memory streams.

7. **diagrams/**  
   Visual flowcharts that resolve everything at a glance.

8. **examples/**  
   Real outputs from Phi-2 and Tonious-7B running the architecture.

---

## 🧩 How to Extend the Repository

### Adding New Documentation  
Add new `.md` files into `docs/` and link them inside `README.md` if needed.

### Adding Diagrams  
Create a new file in `diagrams/` using GitHub-compatible Mermaid.  
Name format:  
```
feature-name.md
```

### Adding Examples  
Place screenshots or structured examples into the appropriate `examples/` subfolder.  
Follow naming format:
```
CategoryName_<set>_<index>.png
```

---

## 🤝 How to Contribute

Tonious is intentionally **model-agnostic and modular**, so contributors can:

- Add new reasoning modes  
- Improve memory formatting  
- Add adapters for new models (Phi-3, LLaMA 3.1, Qwen, Mistral, etc.)
- Propose new symbolic structures  
- Improve diagrams or documentation  
- Suggest optimizations for video-to-text or memory indexing  

Pull requests should include:

1. A clear purpose  
2. Implementation details  
3. Before/after examples if modifying behavior  
4. Respect for the **CC-BY-NC 4.0 license**  
   (No commercial use; attribution required.)

---

## 🔒 License Expectations

### ✔ Allowed  
- Academic and research use  
- Personal projects  
- Non-commercial modifications  
- Forks (with attribution)

### ❌ Not Allowed  
- Commercial apps  
- Selling derivatives  
- Hosting Tonious behind paywalls  
- Using Tonious ideas in a monetized product without permission

---

## 🧭 Final Notes

Tonious is a living architecture.  
Its strength comes from:

- modular design  
- multi-mode reasoning  
- memory-graph persistence  
- video perception → symbolic memory  
- model-agnostic operation  

If you are exploring cognition, LLM reasoning, or perception-to-memory systems, this repo is a foundation you can build on.

For questions or collaborations, see **ABOUTME.md**.


