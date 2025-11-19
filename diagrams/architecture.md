Figure 1 — High-Level Tonious Cognitive System.
User input flows through the Prompting Layer, Mode Router, and a model-agnostic LLM core.
The Tone System formats responses, and the Memory Layer stores reflections in a persistent Tree-of-Life graph.
Output is delivered through the custom UI.
                          
                          ┌───────────────────────────┐
                          │        User Input         │
                          │  (Text / Video / Images)  │
                          └──────────────┬────────────┘
                                         │
                                         ▼
                         ┌────────────────────────────────┐
                         │        Prompting Layer         │
                         │  (Templates • Tone • Context)  │
                         └──────────────┬─────────────────┘
                                         │
                                         ▼
                             ┌─────────────────────┐
                             │     Mode Router     │
                             │─────────────────────│
                             │  • General          │
                             │  • Video            │
                             │  • Recall           │
                             └───────┬─────┬───────┘
                                     │     │
                                     │     │
                                     ▼     ▼
                           (Selected Mode Pipeline)
                                     │
                                     ▼
                         ┌───────────────────────────┐
                         │     LLM Core Engine       │
                         │  (Model-Agnostic: 7B /    │
                         │    Phi / Qwen / etc.)     │
                         └──────────────┬────────────┘
                                         │
                                         ▼
                          ┌───────────────────────────┐
                          │       Tone System         │
                          │  (Grounded / Symbolic)    │
                          └──────────────┬────────────┘
                                         │
                                         ▼
                         ┌────────────────────────────────┐
                         │    Memory Layer (Tree-of-Life) │
                         │  (Video Moments • Logs • Text  │
                         │           • Recalls)           │
                         └──────────────┬─────────────────┘
                                         │
                                         ▼
                          ┌───────────────────────────┐
                          │     UI Output Layer       │
                          │  (Interface • Examples)   │
                          └───────────────────────────┘

