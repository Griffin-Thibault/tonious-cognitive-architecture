Figure 3 shows how Tonious stores long-term memories in the Tree-of-Life (TOL) layer and later uses those memories during Recall mode. Text chats and video reflections are written into symbolic memory files and JSONL streams. When the user asks for a recap, only the relevant recent messages are pulled, formatted, and summarized by the LLM in a grounded style.

```mermaid
flowchart TD

    subgraph A[Conversation & Video Capture]
        U[User Messages<br/>(General / Video)]
        V[Video Trinity Moments<br/>(Scene / Voice / Env)]
    end

    subgraph B[Logging Layer]
        CM[chat_messages Table<br/>(User & Tonious turns)]
        TL[Text Logs<br/>(JSONL Streams)]
        VM[Video Memories<br/>(Markdown Summaries)]
        TS[TOL Streams<br/>(Symbolic Triplets)]
    end

    subgraph C[Recall Request]
        RQ[User: 'Summarize what we have talked about']
        RC[Recent Context Fetch<br/>(Last N Messages)]
        RP[Recall Prompt Builder<br/>(Conversation Log Block)]
    end

    subgraph D[LLM Recall Engine]
        RL[LLM (Recall Mode)<br/>(Grounded Style Only)]
        RS[Recall Summary<br/>(Bullets or Short Paragraph)]
    end

    %% Flows from interaction to memory
    U --> CM
    U --> TL
    V --> VM
    V --> TS

    %% Recall pipeline
    RQ --> CM
    RQ --> RC
    RC --> RP
    RP --> RL
    RL --> RS

    %% Memory feedback
    RS --> TL
    RS --> TS
