# Figure 4 — Mode Routing System (General, Video, Recall)

figure 4 — This figure illustrates how Tonious routes user input into the correct reasoning pipeline.  
The Prompting Layer receives the message and passes it to the Mode Router, which selects one of the  
three independent pipelines: General Mode (standard chat reasoning), Video Mode (Trinity + segment reflection),  
or Recall Mode (compressed summary of recent conversation).  
Each pipeline uses its own prompt template, context rules, and safety style before returning output.

```mermaid
flowchart TD

%% User Input
U[User Input<br>(Text / Commands / Optional Video ID)]

%% Prompt Builder
P[Prompting Layer<br>(Templates + Tone<br>History Window)]

U --> P

%% Mode Router
R{Mode Router<br>General / Video / Recall}

P --> R

%% General Mode
subgraph G[General Mode Pipeline]
GMH[Recent Chat History<br>(Limited Window)]
GMP[General Prompt Template]
GML[LLM Output<br>(Grounded or Symbolic)]
GMH --> GMP --> GML
end

%% Video Mode
subgraph V[Video Mode Pipeline]
VT[Trinity Segments<br>(Scene / Voice / Env)]
VD[Directive Parser<br>(Paragraph / Structured)]
VP[Video Prompt Builder]
VL[LLM Reflection<br>(Literal Style)]
VT --> VD --> VP --> VL
end

%% Recall Mode
subgraph RC[Recall Mode Pipeline]
RCH[Recent Conversation<br>(Last N Messages Only)]
RCP[Recall Prompt Template]
RCL[LLM Summary<br>(Grounded Only)]
RCH --> RCP --> RCL
end

%% Router Connections
R --> G
R --> V
R --> RC

%% Final Output
O[Final Assistant Reply]

GML --> O
VL --> O
RCL --> O
