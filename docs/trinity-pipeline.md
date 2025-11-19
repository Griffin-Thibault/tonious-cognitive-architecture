# Trinity Pipeline (video → moments → memory)

The Trinity Pipeline converts raw video into structured, time-aligned “moments” built from three parallel streams:

- Vision (scene + objects + setting)
- Speech (ASR transcript)
- Environment (ambient audio patterns)

The result is a fused sequence of readable moments that the LLM can understand, along with long-term symbolic memory stored in the Tree-of-Life.

------------------------------------------------------------
1. High-Level Flow
------------------------------------------------------------

Input:
- MP4 video file
- Optional user settings

Output:
- Trinity segments (scene / speech / environment)
- Fused human-readable moment lines
- Video memory Markdown file
- TOL symbolic JSONL memory

Steps:
1. Ingest video and extract metadata
2. Run audio pipeline (ASR + environment)
3. Run visual pipeline (scene recognition)
4. Align all streams in time
5. Fuse into descriptive moments
6. Write Trinity streams and TOL memory

------------------------------------------------------------
2. Ingest and Metadata
------------------------------------------------------------

On upload:
- File saved locally
- ffprobe extracts duration, resolution, codec
- Duration > limit (e.g., 5 minutes) is rejected
- Insert row into videos table:
    id, filename, duration, status, created_at, etc.

------------------------------------------------------------
3. Audio Pipeline (Speech + Environment)
------------------------------------------------------------

Speech:
- Extract audio track
- Run Whisper or WhisperX
- Generate time-stamped segments:
    { start, end, text }

Environment:
- Analyze spectral features (RMS, centroid, ZCR, etc.)
- Map to labels such as: low_hum / room_tone / birds / hiss
- Output:
    { start, end, labels }

------------------------------------------------------------
4. Visual Pipeline
------------------------------------------------------------

- Sample frames over time
- Run lightweight vision model
- Detect scene description and mood
- Output segments:
    { start, end, scene, style }

Also generates:
- A global visual summary
- Per-segment visual lines

------------------------------------------------------------
5. Trinity Alignment
------------------------------------------------------------

Each Trinity segment combines:
- The visual segment covering that time
- Any overlapping speech segments
- Any overlapping environment labels

Example aligned item:
{
  "start": 16.0,
  "end": 21.0,
  "scene": "a dog standing next to a couch",
  "speech": "Look at her. She looks happy.",
  "env": ["low_hum"]
}

Saved to:
08_YESOD/streams/video_<id>_scenes.jsonl

------------------------------------------------------------
6. Fused Moment Sentences
------------------------------------------------------------

Each Trinity entry becomes a single readable line:

"<start>-<end>: <scene>. Ambient: <env>. Voice: "<speech>"".

If speech or environment is missing, it is omitted cleanly.

These moments appear in the UI’s left column.

------------------------------------------------------------
7. Memory Artifacts
------------------------------------------------------------

1. Video Memory Markdown  
   Path: S05_HARMONY_TIFERET/memories/video_<id>.md  
   Contains:
   - Global summary
   - Key beats
   - Tags

2. TOL Symbolic Streams  
   Path: 05_TIFERET/logs/video_<id>_tol.jsonl  
   Each moment translated into a soul/spirit/body node.

------------------------------------------------------------
8. How the LLM Uses Trinity (Video Mode)
------------------------------------------------------------

When the user asks for a video reflection:
- UI sends instruction + fused Trinity lines
- Backend builds video-mode prompt:
    system → stored summary → segments
- LLM produces:
    - structured summary, or
    - a single paragraph reflection

Symbolic tone is disabled here for accuracy.

------------------------------------------------------------
9. Current Limitations
------------------------------------------------------------

- Duration limit due to compute
- LLM only sees text (not native multimodal input)
- Simple time-based segmentation (not shot-based)
- Requires hierarchical summaries for long videos
- No salience ranking of important moments

------------------------------------------------------------
10. Future Improvements
------------------------------------------------------------

- Longer videos with chapter-level structure
- Better salience-based filtering
- Similarity search over moments using embeddings
- Real shot-boundary detection
- Dynamic “zoom in / zoom out” summarization

------------------------------------------------------------
Core Principle
------------------------------------------------------------

Trinity converts perception into structured text.  
That text becomes memory.  
Any model can then reason over it.

