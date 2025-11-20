Figure 4 — Mode Routing System (General, Video, Recall)

This figure illustrates how Tonious routes user input into the correct reasoning pipeline. The Prompting Layer
receives the message and passes it to the Mode Router, which selects one of the three independent pipelines:
General Mode (standard chat reasoning), Video Mode (Trinity + segment reflection), or Recall Mode (compressed
summary of recent conversation). Each pipeline uses its own prompt template, context rules, and safety style
before returning output.

```mermaid
flowchart TD
    U["User input (text / commands / optional video id)"]
    P["Prompting layer (templates, tone, history window)"]
    R{"Mode router"}

    G["General mode pipeline"]
    V["Video mode pipeline"]
    RC["Recall mode pipeline"]

    O["Final assistant reply"]

    U --> P --> R
    R --> G --> O
    R --> V --> O
    R --> RC --> O
