Figure 3 shows how Tonious stores long-term memories in the Tree-of-Life (TOL) layer and later uses those memories during Recall mode. Text chats and video reflections are written into symbolic memory files and JSONL streams. When the user asks for a recap, only the relevant recent messages are pulled, formatted, and summarized by the LLM in a grounded style.

```mermaid
flowchart TD

    %% =============================
    %% Conversation & Video Capture
    %% =============================
    subgraph A[Conversation and Video Capture]
        U[User Messages (General or Video)]
        V[Video Trinity Moments (Scene, Voice, Env)]
    end

    %% =============================
    %% Logging Layer
    %% =============================
    subgraph B[Logging Layer]
        CM[chat_messages Table]
        TL[Text Logs (JSONL)]
        VM[Video Memory Files (Markdown)]
        TS[TOL Streams (Symbolic Triplets)]
    end

    %% =============================
    %% Recall Process
    %% =============================
    subgraph C[Recall Request]
        RQ[User asks for a summary]
        RC[Fetch Recent Context (Last N messages)]
        RP[Build Recall Prompt (Conversation Log)]
    end

    %% =============================
    %% LLM Recall Engine
    %% =============================
    subgraph D[LLM Recall Engine]
        RL[LLM in Recall Mode (Grounded Only)]
        RS[Generated Summary (Bullets or Paragraph)]
    end

    %% Flows from interaction to memory
    U --> CM
    U --> TL
    V --> VM
    V --> TS

    %% Recall pipeline
    RQ --> RC
    RC --> RP
    RP --> RL
    RL --> RS

    %% Summary is written back to memory
    RS --> TL
    RS --> TS
