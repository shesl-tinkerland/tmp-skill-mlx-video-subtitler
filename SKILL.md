---
name: MLX Video Subtitler
description: >
  Guide users through transcribing video/audio to SRT/VTT subtitles on Apple
  Silicon using MLX Whisper — model selection, language options, and output
  formatting.
source_url: https://github.com/RayFernando1337/MLX-Auto-Subtitled-Video-Generator
---

# MLX Video Subtitler

Help the user set up and run local video/audio transcription on an Apple Silicon
Mac using the MLX-Auto-Subtitled-Video-Generator Streamlit app. Cover
installation, model selection, transcription workflow, and subtitle export.

Provenance:
- Source repository: https://github.com/RayFernando1337/MLX-Auto-Subtitled-Video-Generator
- Discovery URL: https://x.com/RayFernando1337/status/1820148732396229038

## What This Tool Does

The upstream project is a Streamlit web UI that:

1. Accepts video uploads (MP4, AVI, MOV, MKV)
2. Extracts audio via FFmpeg
3. Runs MLX Whisper inference on Apple Silicon (M-series GPU)
4. Outputs VTT and SRT subtitle files as a downloadable ZIP

It runs entirely on-device — no cloud API, no data leaves the machine.

## Prerequisites

- Apple Silicon Mac (M1 / M2 / M3 / M4 series)
- Conda or Miniforge for environment management
- FFmpeg installed via Homebrew
- Xcode Command Line Tools

## Installation Steps

```bash
git clone https://github.com/RayFernando1337/MLX-Auto-Subtitled-Video-Generator.git
cd MLX-Auto-Subtitled-Video-Generator
conda create -n mlx-whisper python=3.12
conda activate mlx-whisper
xcode-select --install
pip install -r requirements.txt
brew install ffmpeg
```

## Running

```bash
conda activate mlx-whisper
streamlit run mlx_whisper_transcribe.py
```

The Streamlit UI opens in the browser. Upload a video, pick a model, click
Transcribe, then download the ZIP with VTT/SRT files.

## Model Selection Guide

| Model | Size | Speed | Best for |
|---|---|---|---|
| Tiny (Q4) | Smallest | Fastest | Quick drafts, short clips |
| Small (FP32) | Small | Fast | General use, good accuracy |
| Small English (Q4) | Small | Fast | English-only content |
| Distil Large v3 | Medium | Fast | English, high accuracy |
| Large v3 | Largest | Slower | Best multilingual accuracy |
| Large v3 Turbo | Medium | ~50x realtime on M2 Ultra | Best speed/accuracy balance |

Recommend **Large v3 Turbo** for most users — it transcribes 12 minutes of
video in ~14 seconds on M2 Ultra while staying close to Large v3 accuracy.

## Supported Languages

Auto-detect, English, Spanish, French, German, Italian, Portuguese, Dutch,
Russian, Chinese, Japanese, Korean, Hebrew — and all other Whisper-supported
languages via auto-detect.

## Troubleshooting

- "No Metal device" — verify the Mac has Apple Silicon (`uname -m` should show `arm64`)
- FFmpeg errors — ensure `brew install ffmpeg` completed and `ffmpeg` is on PATH
- Slow transcription — try a smaller model or check that no other GPU-heavy process is running
- Import errors — make sure the conda environment is activated and all requirements installed

## When the User Asks

- **Which model?** — default to Large v3 Turbo; switch to Tiny for speed tests
  or Large v3 for maximum multilingual accuracy.
- **Non-English content?** — use Large v3 or Large v3 Turbo with auto-detect or
  explicit language selection.
- **Batch processing?** — the Streamlit UI handles one file at a time; for batch
  work, suggest scripting `mlx_whisper.transcribe()` directly in Python.
- **Output format?** — the app generates both VTT and SRT; both are in the
  download ZIP.
