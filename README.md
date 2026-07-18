# 🎬 AI Trailer Studio (MVP)

A minimal, deploy-ready Streamlit app for creating cinematic movie trailers from your own video clips — upload footage, trim and arrange scenes, add titles and captions, apply effects, generate AI voice narration, mix in background music, and export a finished MP4.

Built for a hackathon MVP: small feature set, no fragile dependencies (no OpenCV, no Whisper, no ImageMagick), and tested end-to-end against the exact library versions pinned in `requirements.txt`.

## Features

- **Upload** multiple videos (MP4, MOV, AVI, MKV, WEBM, MPEG)
- **Trim** each clip and **arrange** them on a timeline (reorder / delete)
- **Title cards** and **text overlays** with adjustable font, size, and color (uses system DejaVu fonts — no ImageMagick needed)
- **Effects**: fade in, fade out, black & white, speed up/slow motion
- **AI voice narration** via Google Text-to-Speech (gTTS)
- **Background music** with volume control and automatic looping to match trailer length
- **Export** at 480p (fast) or 720p, download the final MP4 directly in the browser

## Project structure

```
.
├── main.py            # the Streamlit app
├── requirements.txt   # pinned Python packages
├── packages.txt        # system packages (ffmpeg, fonts) for Streamlit Cloud
└── README.md
```

## Run locally

1. Install [ffmpeg](https://ffmpeg.org/download.html) on your machine (required by moviepy):
   - macOS: `brew install ffmpeg`
   - Ubuntu/Debian: `sudo apt install ffmpeg`
   - Windows: download a build and add it to your PATH

2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the app:
   ```bash
   streamlit run main.py
   ```

4. Open the URL Streamlit prints (usually `http://localhost:8501`).

## Deploy on Streamlit Community Cloud

1. Push `main.py`, `requirements.txt`, and `packages.txt` to a GitHub repo (all three in the repo root).
2. Go to [share.streamlit.io](https://share.streamlit.io), sign in, and click **New app**.
3. Select your repo/branch and set the main file path to `main.py`.
4. Deploy. Streamlit Cloud automatically installs `requirements.txt` (Python packages) and `packages.txt` (system packages — ffmpeg and fonts).

No API keys or secrets are needed — narration uses the free gTTS service, which just needs outbound internet access (available by default on Streamlit Cloud).

## How to use it

1. **Upload & Trim** — upload your clips, set start/end times, add each trimmed piece to the timeline.
2. **Timeline** — reorder or remove clips.
3. **Titles & Text** — optionally add an intro title card and/or a caption overlay across the whole trailer; pick font, size, and color.
4. **Effects** — toggle fade in/out, black & white, and adjust playback speed.
5. **Narration & Music** — optionally generate AI narration from a script, and/or upload background music with a volume slider.
6. **Export** — choose 480p or 720p, click **Render Trailer**, then preview and download the MP4.

## Notes & limitations

- This is intentionally a lean MVP — it favors reliability over feature count (per the hackathon brief, heavier features like auto scene detection, subtitle transcription, and AI-generated posters were left out to avoid deploy failures).
- Rendering happens on the server, so very long or high-resolution source videos will take longer and use more memory. Keep demo clips short (under ~1–2 minutes total) for a smooth live demo.
- Streamlit Cloud's free tier has limited CPU/RAM — if a render fails, try shorter clips, fewer effects, or 480p export.
- Each session's uploaded files and renders live in a temp directory and are not persisted between app restarts.

## Tech stack

- [Streamlit](https://streamlit.io/) — UI
- [MoviePy 2.x](https://zulko.github.io/moviepy/) — video editing/compositing
- [Pillow](https://python-pillow.org/) — title/caption text rendering
- [gTTS](https://gtts.readthedocs.io/) — AI text-to-speech narration
- [ffmpeg](https://ffmpeg.org/) — underlying encode/decode engine
