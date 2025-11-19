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

