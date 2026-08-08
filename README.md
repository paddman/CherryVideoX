# CherryVideoX

**AI-assisted video editor for Desktop + Web** built from one React/TypeScript codebase, with a Tauri 2 desktop shell.

![CherryVideoX editor](docs/cherryvideox-editor.jpg)

CherryVideoX is an original editing workspace for short-form video, social content, product reels, and creator workflows. The repository currently contains a runnable MVP with a polished editor UI, local media import, preview controls, timeline editing, non-destructive inspector controls, project autosave, AI-assistant actions, and browser-side WebM preview export.

> The application is not a CapCut clone and includes no CapCut code or branding. The layout is an original CherryVideoX implementation in the same broad product category.

## Current MVP

- Desktop app: Windows, macOS, and Linux through **Tauri 2**
- Web app: Vite + React + TypeScript
- Local import: video, image, and audio files
- Media browser with search, categories, folders, and timeline insertion
- Multi-track timeline for video, overlays, text, captions, voiceover, music, and effects
- Drag clips horizontally, select, split, duplicate, delete, undo, and redo
- Playhead seeking and frame stepping
- 9:16, 16:9, 1:1, and 4:5 preview ratios
- Transform, opacity, flip, playback speed, volume, brightness, contrast, saturation, and blur controls
- Editable title and subtitle overlays
- Thai AI-caption demo action
- Highlight-cut marker generation
- Background-mask preview
- Cherry AI assistant panel with editor actions
- Local project autosave with `localStorage`
- Browser-side 720×1280 WebM preview export using Canvas + MediaRecorder
- Installable web-app manifest

## Stack

| Layer | Technology |
|---|---|
| Shared UI | React 19 + TypeScript |
| Web build | Vite 8 |
| Desktop shell | Tauri 2 + Rust |
| Icons | Lucide React |
| Project persistence | Local storage for the MVP |
| Preview export | Canvas capture + MediaRecorder |

## Run the Web App

Requirements: Node.js 22.12 or newer.

```bash
npm install
npm run dev
```

Open `http://localhost:1420`.

Production web build:

```bash
npm run build
npm run preview
```

## Run the Desktop App

Install the Tauri prerequisites for your operating system, including Rust and the required native WebView/build packages. Then run:

```bash
npm install
npm run tauri:dev
```

Build desktop installers:

```bash
npm run tauri:build
```

## Repository Structure

```text
CherryVideoX/
├─ public/                  # Brand images, app icons, demo poster, web manifest
├─ docs/                    # README showcase image
├─ src/
│  ├─ App.tsx               # Editor workspace and interactions
│  ├─ demoData.ts           # Demo assets, tracks, and timeline clips
│  ├─ types.ts              # Shared editor models
│  ├─ styles.css            # Full desktop editor visual system
│  └─ lib/
│     ├─ exportProject.ts   # Canvas/MediaRecorder WebM export
│     └─ useHistoryState.ts # Undo/redo state controller
├─ src-tauri/               # Tauri 2 desktop shell
└─ .github/workflows/       # Build validation
```

## Production Roadmap

The current repository establishes the product shell and editing-state model. A production-grade editor still needs the parts that tend to consume entire engineering teams rather than obediently materializing from a mockup:

1. **Render engine**: FFmpeg native sidecar for Desktop and FFmpeg/WebCodecs workers for Web; MP4/H.264, HEVC, AV1, ProRes, proxy media, background rendering, and resumable jobs.
2. **Timeline engine**: ripple/roll/slip/slide trims, nested sequences, compound clips, magnetic snapping, transition handles, keyframe curves, and frame-accurate decoding.
3. **Audio engine**: mixer, waveform extraction, loudness normalization, noise removal, ducking, audio buses, and export muxing.
4. **AI services**: production speech-to-text, diarization, translation, segmentation, motion tracking, highlight ranking, script generation, TTS, and avatar generation.
5. **Project storage**: versioned project JSON, IndexedDB/SQLite media index, autosave journal, recovery, cloud sync, collaboration, and comments.
6. **Security and distribution**: hardened CSP, signed builds, auto-update, crash reporting, telemetry consent, license handling, and release channels.

## Architecture Direction

The shared React application owns project state, media browsing, timeline operations, inspector controls, and the preview UI. Platform-specific capabilities are placed behind adapters:

```text
React/TypeScript Editor
        │
        ├── Web Adapter
        │     ├── File API / IndexedDB
        │     ├── WebCodecs / MediaRecorder
        │     └── Web Workers / WASM
        │
        └── Tauri Adapter
              ├── Native file system
              ├── Rust commands
              ├── FFmpeg sidecar
              └── SQLite / background jobs
```

This keeps one product UI while allowing Desktop to use native rendering and the browser build to use standards-based fallbacks.

## License

MIT
