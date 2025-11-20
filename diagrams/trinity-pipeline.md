Figure 2 — Trinity Video Pipeline.
Raw video is transformed into a unified “Trinity Moment” representation through ASR, visual analysis, and environment audio.
Moments are fused into a single semantic stream, summarized, and stored in the Tree-of-Life memory layer for later recall, reasoning, and narrative transformation.

                         ┌───────────────────────────────┐
                         │        Raw Video Input         │
                         │      (MP4 ≤ 5 minutes)         │
                         └─────────────────┬──────────────┘
                                           │
                                           ▼
                    ┌──────────────────────────────────────────┐
                    │           Ingest Processor               │
                    │------------------------------------------│
                    │ • ffprobe metadata (fps, codec, duration)│
                    │ • Create DB entry / video_id             │
                    └─────────────────┬────────────────────────┘
                                      │
                                      ▼
        ┌────────────────────────────────────────────────────────────────┐
        │                    Trinity Extraction Pipeline                 │
        │----------------------------------------------------------------│
        │  1. **ASR (Speech)**                                           │
        │     Whisper / whisperX → time-stamped transcript segments      │
        │                                                                │
        │  2. **Visual Analysis**                                        │
        │     Frame sampling → vision model → scene labels + objects     │
        │                                                                │
        │  3. **Environment Audio**                                      │
        │     Ambient classifier → hum / room tone / movement / silence  │
        └───────────────┬───────────────────────────┬────────────────────┘
                        │                           │
                        └───────────────┬───────────┘
                                        ▼
                      ┌──────────────────────────────────────┐
                      │      Trinity Fusion Engine           │
                      │--------------------------------------│
                      │ Combines:                            │
                      │   • speech text                      │
                      │   • visual scene                     │
                      │   • environment tags                 │
                      │ Into unified “Trinity Moments”       │
                      └───────────────┬──────────────────────┘
                                      │
                                      ▼
              ┌─────────────────────────────────────────────────────────┐
              │     Yesod Stream Writer (video_<id>_scenes.jsonl)      │
              │---------------------------------------------------------│
              │ • One JSONL record per moment                           │
              │ • Includes start/end time, scene, voice, environment    │
              └───────────────┬─────────────────────────────────────────┘
                              │
                              ▼
                      ┌──────────────────────────────────────┐
                      │     ToniousBrain Summarizer          │
                      │--------------------------------------│
                      │ • Short summary                      │
                      │ • Long summary                       │
                      │ • Global description                 │
                      │ • Key “beats” extracted              │
                      └───────────────┬──────────────────────┘
                                      │
                                      ▼
                   ┌──────────────────────────────────────────────┐
                   │   TOL Memory Layer (S05_HARMONY_TIFERET)     │
                   │-----------------------------------------------│
                   │ Stores:                                       │
                   │  • Markdown video memory file                 │
                   │  • tol_beats.jsonl (symbolic triplets)        │
                   │  • Linked indexes (daat_index.json)           │
                   └──────────────────┬────────────────────────────┘
                                      │
                                      ▼
                      ┌──────────────────────────────────────┐
                      │        UI Video Mode Output          │
                      │--------------------------------------│
                      │ • Structured reflection              │
                      │ • One paragraph or full breakdown    │
                      │ • Human-memory style summaries       │
                      └──────────────────────────────────────┘
