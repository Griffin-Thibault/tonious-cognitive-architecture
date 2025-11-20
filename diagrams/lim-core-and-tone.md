Figure 5 — LLM Core & Tone System Flow.
This figure shows how Tonious processes a fully-constructed prompt using a model-agnostic LLM engine, then applies the Tone System (Grounded or Symbolic). 
The LLM core performs all inference, while the Tone System reformats only the final text—never altering meaning or safety logic. 
This makes the entire pipeline stable, swappable across models (Phi-2, 7B, 14B, etc.), and consistently stylized.

```mermaid
flowchart TD
    P["Final Prompt (mode + tone + context)"]
    LLM["LLM Core Engine<br/>(Model-Agnostic Inference)"]
    RAW["Raw Model Output<br/>(unformatted text)"]
    TONE{"Tone System<br/>(Grounded / Symbolic)"}
    OUT["Final Assistant Reply"]

    P --> LLM --> RAW --> TONE --> OUT
