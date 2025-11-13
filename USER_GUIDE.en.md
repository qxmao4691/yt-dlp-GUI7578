# yt-dlp GUI — User Guide
[中文](./USER_GUIDE.zh-CN.md) | English

This guide covers the graphical interface for yt-dlp (yt_dlp_gui.py). It lets you parse video/audio formats, select single or multiple (batch) combinations, download videos and audio, use Cookies for login-required content, embed subtitles, and enable EJS (External JS Scripts) + a JS runtime to unlock YouTube 4K/AV1/VP9/HDR and other high-tier formats.

---

## Table of Contents
- Overview
- Requirements
- Installation and Environment
  - Python and dependencies
  - FFmpeg
  - EJS + JS runtime (to unlock 4K/high-tier formats on YouTube)
- Quick Start
- UI Overview
  - Basic Settings
  - Advanced Settings (Cookie / EJS / Runtime)
  - Format Selector Dialog (Multi-select and Batch)
- Download Modes
  - Single-format download
  - Batch download (video × audio combinations)
- Format Expression Examples
- FAQ / Troubleshooting
- Safety and Compliance
- Optional: Package as a Windows executable

---

## Overview
- Paste a video URL and parse all available formats (including separate video/audio).
- Download:
  - Single-format (best, 1080p, audio-only, custom expressions)
  - Batch combinations from multiple selected video and audio formats
- Extras:
  - MP3 audio extraction
  - Subtitle embedding
  - Cookie support (file or from browser)
  - EJS + JS runtime to unlock 4K/AV1/VP9/HDR formats on YouTube
- Live progress and logs.

---

## Requirements
- OS: Windows 10/11, macOS, or Linux
- Python 3.8+
- Network access to targets; if using remote EJS scripts, access to GitHub Releases

---

## Installation and Environment

### 1) Python and yt-dlp
Install Python (add it to PATH), then:
```bash
pip install -U yt-dlp
```
Recommended for YouTube 4K support:
```bash
pip install -U yt-dlp yt-dlp-ejs
```
Run the GUI (after saving as `yt_dlp_gui.py`):
```bash
python yt_dlp_gui.py
```

### 2) FFmpeg (merge separate streams, audio extraction, subtitles)
- Windows:
```bash
winget install ffmpeg
```
- macOS:
```bash
brew install ffmpeg
```
- Linux: use your package manager (e.g., `apt install ffmpeg`).

Without FFmpeg, yt-dlp cannot automatically merge separate video/audio streams into a single file.

### 3) EJS + JS runtime (YouTube 4K/high-tier formats)
YouTube’s 4K/2K/AV1/VP9/HDR often requires deciphering protected parameters via EJS scripts.

Option A (recommended):
```bash
pip install -U yt-dlp-ejs
# JS runtime (choose one, Deno recommended)
# Windows:
winget install Deno.Deno
# macOS:
brew install deno
# Or install Node.js / Bun / QuickJS
```

Option B:
- If you don’t install `yt-dlp-ejs`, the GUI enables remote EJS (ejs:github) when “Enable Advanced Formats (EJS)” is turned on.
- You still need a JS runtime installed.

Configure EJS under Advanced Settings (checkbox + runtime selector/path).

---

## Quick Start
1. Run `yt_dlp_gui.py`.
2. Paste the video URL in “Basic Settings.”
3. Click “Parse Formats.”
4. In the dialog:
   - Single-select a “complete format” or a “recommended preset,” or
   - Multi-select multiple “Video-only” and “Audio-only” formats for batch combinations.
5. Click “Start Download.”

For 4K/high-tier:
- Enable “Advanced Formats (EJS)”
- Install Deno or Node.js (Deno recommended)
- Install `yt-dlp-ejs` (recommended)

---

## UI Overview

### Basic Settings
- Video URL: Paste the target URL (YouTube, Bilibili, etc.)
- Output Directory: Choose where to save
- Format Selection (single-expression):
  - Quick choices (best, 1080p, audio-only, etc.)
  - Custom format expression (see examples)
- Other:
  - Extract Audio (MP3)
  - Embed Subtitles (requires FFmpeg)

### Advanced Settings (Cookie / EJS / Runtime)
- Cookie file (`cookies.txt`) or read cookies from browser (Chrome/Firefox/Edge/Safari/Opera/Brave)
- Enable Advanced Formats (EJS)
- JS Runtime: auto/deno/node/bun/quickjs and optional explicit path

### Format Selector Dialog (multi-select and batch)
Tabs:
- All Formats: Overview of complete/video-only/audio-only (single-select for single download)
- Video Formats (video-only; multi-select with Ctrl/Shift)
- Audio Formats (audio-only; multi-select)
- Recommended Presets (single-select)
- Combination Preview: See selected video IDs, audio IDs, and the cross-product previews

Buttons:
- Generate combinations and confirm: returns a batch set (video × audio). If only video or only audio is selected, returns that list for individual downloads.
- Confirm current single selection: returns the selected single format or preset
- Clear / Cancel

Tip: If you don’t see combinations, ensure you actually multi-selected rows (Ctrl/Shift) and switch to “Combination Preview” or click “Manual Refresh.”

---

## Download Modes

### Single-format download
- Choose a complete format or a recommended preset in the dialog, or write a custom expression in the main UI.
- Click “Start Download.”

### Batch download (video × audio)
- Multi-select N video-only formats and M audio-only formats in the dialog.
- Click “Generate combinations and confirm.”
- Click “Start Download” in main UI.
- The app downloads N×M combinations sequentially (may take time).

Notes:
- Batch runs sequentially; large batches can take long.
- Merging separate streams requires FFmpeg.

---

## Format Expression Examples
```text
# Best video+audio; fallback to best complete format
bestvideo+bestaudio/best

# Max resolution
bestvideo[height<=2160]+bestaudio   # 4K
bestvideo[height<=1440]+bestaudio   # 2K
bestvideo[height<=1080]+bestaudio   # 1080p

# Prefer MP4
bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]

# Specify codecs
bestvideo[vcodec^=av01]+bestaudio   # AV1
bestvideo[vcodec^=vp9]+bestaudio    # VP9
bestvideo[vcodec^=avc]+bestaudio    # H.264/AVC

# Exact format ID pair
137+140   # 137: 1080p video, 140: m4a audio
```

---

## FAQ / Troubleshooting

- No 4K/2K/AV1/VP9 formats?
  - Enable EJS, install `yt-dlp-ejs` + JS runtime (Deno recommended), or permit remote EJS (auto).
  - Ensure GitHub Releases can be accessed for remote scripts.

- Downloaded files are not merged?
  - Install FFmpeg.

- Login-required/Member videos fail?
  - Use Cookies (file or browser).
  - Membership content requires a valid subscription.

- “signature/n/challenge” errors?
  - EJS/runtime not ready. Install `yt-dlp-ejs` and Deno/Node; check logs.

- Combination Preview is empty?
  - Ensure true multi-selection (Ctrl/Shift). Switch to the “Combination Preview” tab or click “Manual Refresh.”

- Cancel does not stop immediately?
  - It’s a soft cancel; it stops after the current part. To hard-stop, close the app.

---

## Safety and Compliance
- Follow target websites’ TOS and local laws.
- Cookie files carry login sessions—keep them private.
- This tool is for lawful use only.

---

## Optional: Package to Windows EXE
```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name="yt-dlp GUI" yt_dlp_gui.py
```
An EXE will be created in `dist`. Users still need FFmpeg (and a JS runtime for EJS/4K).

---
