# Diagram 3 — Memory and Recall Flow

Figure 3 — Memory and Recall Flow.  
This diagram illustrates how Tonious stores long-term information in the Tree-of-Life (TOL) layer and later retrieves it during Recall mode.  
User text messages and Trinity-derived video moments are written into structured logs, markdown memories, and symbolic TOL triplets.  
When a Recall request is made, Tonious does not search the entire history.  
Instead, it retrieves only the most recent context window, pulls relevant log segments, and builds a compact recall prompt.  
The LLM then produces a grounded summary, relying on both the recent chat and any linked TOL streams.


```mermaid
flowchart TD

%% ======================
%% Conversation Capture
%% ======================
U[User Messages General Mode]
VTM[Video Trinity Moments Scene Voice Env]

%% ======================
%% Logging Layer
%% ======================
subgraph Logging Layer
    CM[Chat Messages Table]
    TL[Text Logs JSONL]
    VM[Video Memory Files Markdown]
    TS[TOL Streams Symbolic Triplets]
end

%% ======================
%% Recall Request
%% ======================
subgraph Recall Request
    RQ[User Ask Recall]
    RC[Recent Context Fetch Last Messages]
    RP[Recall Prompt Builder]
end

%% ======================
%% LLM Recall Engine
%% ======================
subgraph LLM Recall Mode
    RL[LLM Recall Processing Grounded]
    RS[Recall Summary Output]
end

%% Write path
U --> CM
VTM --> VM
CM --> TL
TL --> TS
VM --> TS

%% Recall path
RQ --> RC --> RP --> RL --> RS

%% Context for recall comes from logs
RC --> TL
RC --> TS
