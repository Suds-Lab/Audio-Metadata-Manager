# Audio Metadata Manager

A standalone, single-file HTML app for browsing and editing audio file metadata — no server, no install, no dependencies beyond a modern browser and Python.

---

## Features

- **Folder scanning** — open any music directory and it auto-discovers artists, albums, and tracks recursively
- **Inline editing** — edit title, artist, album, and track number for every file directly in the UI
- **Album art** — detects existing art (`Folder.jpg`, `AlbumArtSmall.jpg`, etc.) and lets you upload replacements
- **Auto-fill** — populate metadata from folder structure in one click (artist from parent folder, album from subfolder, track number from filename prefix)
- **Bulk apply** — push artist/album values to all tracks in an album at once
- **Export script** — generates a Python script using `mutagen` that writes the metadata back to your actual files
- **Supports MP3, M4A, and WMA**

---

## Usage

### 1. Open the app
Download `audio-metadata-manager.html` and open it in **Chrome or Edge**.  
> Firefox is not supported — it lacks the File System Access API.

### 2. Open your music folder
Click **"Open Music Folder"** and select your root music directory. The app scans it and builds a tree of artists and albums.

### 3. Edit metadata
- Click any album in the sidebar to open the editor
- Edit fields inline, upload album art, add or remove tracks
- Hit **"Auto-fill all"** to bulk-populate everything from your folder structure

### 4. Export and apply
Click **"Export Script"** to download `update_metadata.py`.  
Drop the script into your **music root folder** (the same folder you opened in the app), then run:

```bash
pip install mutagen
python3 update_metadata.py
```

No path configuration needed — the script uses its own location as the root. It prints a line for each file processed and lists any errors at the end.

> **Album art:** any cover images you upload in the GUI are base64-encoded and embedded directly in the script. When you run it, they are decoded and written to disk as `cover.jpg` inside each album folder, then embedded into the audio files.

---

## Folder structure conventions

The auto-fill logic assumes this layout:

```
Music Root/
├── Artist Name/
│   ├── Album Name/
│   │   ├── 01 Track Title.mp3
│   │   └── Folder.jpg
│   └── Another Album/
│       └── 01 Track.mp3
└── Standalone Album/        ← no artist (left blank)
    └── 01 Track.mp3
```

- **Artist** → top-level folder name (blank if files sit directly in the root folder)
- **Album** → subfolder name
- **Title** → filename minus extension
- **Track number** → numeric prefix from filename

---

## Requirements

| Task | Requirement |
|------|-------------|
| Running the GUI | Chrome or Edge (any recent version) |
| Writing metadata to files | Python 3.7+ · `pip install mutagen` |
| Running the export script | Drop `update_metadata.py` in your music root folder |

---

## License

MIT