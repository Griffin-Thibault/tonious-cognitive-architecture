Figure 3 shows how Tonious stores long-term memories in the Tree-of-Life (TOL) layer and later uses those memories during Recall mode. Text chats and video reflections are written into symbolic memory files and JSONL streams. When the user asks for a recap, only the relevant recent messages are pulled, formatted, and summarized by the LLM in a grounded style.

```mermaid
flowchart TD

    %% Conversation and Video Capture
    U[User Messages\n(General or Video)]
    VTM[Video Trinity Moments\n(Scene, Voice, Env)]

    %% Logging Layer
    subgraph Logging_Layer
        CM[chat_messages table\n(user and Tonious turns)]
        TL[Text Logs\n(JSONL)]
        VM[Video Memory Files\n(Markdown summaries)]
        TS[TOL Streams\n(symbolic triplets)]
    end

    %% Recall Request
    subgraph Recall_Request
        RQ[User request:\n"Summarize what we have talked about"]
        RC[Recent Context Fetcher\n(last N messages)]
        RP[Recall Prompt Builder\n(conversation log block)]
    end

    %% LLM Recall Engine
    subgraph LLM_Recall
        RL[LLM in Recall Mode\n(grounded style only)]
        RS[Recall Summary\n(bullets or short paragraph)]
    end

    %% Write path
    U --> CM
    VTM --> VM
    CM --> TL
    VM --> TS
    TL --> TS

    %% Recall path
    RQ --> RC --> RP --> RL --> RS

    %% Context sources for recall
    RC --> TL
    RC --> TS
