# Reelsidian — Local-First AI Study Companion

> Turn educational Reels and YouTube Shorts into permanent, searchable, goal-connected study material — privately, on your own machine.

Reelsidian captures audio from educational short videos, transcribes it locally, generates structured study notes and flashcards using a local Ollama model, and connects every report to your personal learning goals through an interactive knowledge graph.

---

## The Problem

Students save hundreds of educational Reels and Shorts, but rarely revisit them.

- Saved videos become a “bookmark cemetery.”
- Video content is not searchable.
- Individual videos do not connect to real learning goals.
- Cloud AI tools require sending audio and transcripts to external servers.

Reelsidian solves this by turning passive short-video consumption into a private, structured second brain.

---

## What Reelsidian Does

1. Capture audio from an Instagram Reel or YouTube Short using a Chrome extension.
2. Transcribe the audio locally using Whisper.
3. Generate a structured study report using an Ollama model.
4. Create flashcards, key concepts, action items, and tags.
5. Store reports and goals locally in SQLite.
6. Connect reports to academic goals in a knowledge graph.
7. Export study material to Markdown or Anki-compatible flashcards.

---

## Key Features

### Chrome Extension
- Captures audio from the active video tab.
- Works with Instagram Reels and YouTube Shorts.
- Shows recording status, timer, and audio visualization.
- Sends captured audio to the local Reelsidian server.

### Local AI Pipeline
- **Speech-to-text:** faster-whisper
- **Study-report generation:** Ollama model
- **Goal linking:** Ollama model
- **Local storage:** SQLite

No ElevenLabs, Gemini, or OpenRouter API key is required for the core local workflow.

### Study Dashboard
- View transcripts, summaries, key concepts, and flashcards.
- Browse all saved study reports.
- Search through transcripts and titles.
- Export reports as Markdown.
- Export flashcards for Anki.

### Knowledge Graph
- Add personal learning goals such as:
  - “Master JavaScript closures”
  - “Prepare for placements”
  - “Learn AI engineering”
- Reelsidian links relevant study reports to your goals.
- View the relationship between what you watch and what you want to learn.

---

## Architecture

```text
Instagram Reel / YouTube Short
            |
            v
Chrome Extension
            |
            | Audio upload
            v
Local Node.js Server
            |
            |----------------------|
            v                      v
     faster-whisper          Ollama model
     local transcription     study synthesis
            |                      |
            |----------------------|
                        v
                    SQLite database
                        |
                        v
                Reelsidian Dashboard
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Browser extension | Chrome Manifest V3, Web Audio API, MediaRecorder |
| Backend | Node.js, Express |
| Transcription | faster-whisper |
| AI model | Ollama |
| Database | SQLite |
| Dashboard | HTML, CSS, JavaScript |
| Graph visualization | Force-directed graph |
| Export | Markdown, Anki TSV |

---

## Project Structure

```text
reelsidian/
├── reelsidian-extension/
│   ├── manifest.json
│   ├── popup.html
│   ├── popup.js
│   ├── background.js
│   ├── content.js
│   ├── offscreen.html
│   └── offscreen.js
│
└── server/
    ├── server.js
    ├── package.json
    ├── .env
    │
    ├── local-ai/
    │   └── transcribe.py
    │
    ├── src/
    │   ├── config/
    │   ├── routes/
    │   ├── services/
    │   └── utils/
    │
    ├── database/
    │   └── reelsidian.db
    │
    └── public/
        ├── index.html
        ├── css/
        └── js/
```

---

## Getting Started

### Prerequisites

Install the following:

- Node.js 18+
- Python 3.10+
- Google Chrome
- Ollama
- Git

### 1. Clone the repository

```bash
git clone [https://github.com/your-username/reelsidian.git](https://github.com/your-username/reelsidian.git)
cd reelsidian
```

### 2. Install backend dependencies

```bash
cd server
npm install
```

### 3. Install local transcription dependencies

```bash
pip install faster-whisper
```

### 4. Install and configure Ollama

Install Ollama from:

https://ollama.com/download

Then pull a local model:

```bash
ollama pull qwen3:4b
```

For testing with an Ollama cloud model, you can use:

```bash
ollama pull gemma4:cloud
```

> Note: `gemma4:cloud` runs through Ollama Cloud. For a fully private and offline version, use a locally downloaded model such as `qwen3:4b`.

### 5. Configure environment variables

Create a `.env` file inside the `server/` folder:

```env
PORT=5000

# Local transcription
WHISPER_MODEL=base

# Ollama configuration
OLLAMA_URL=http://localhost:11434
OLLAMA_MODEL=qwen3:4b

# Database
DATABASE_PATH=./database/reelsidian.db
```

If you are currently testing with Ollama Cloud, use:

```env
OLLAMA_MODEL=gemma4:cloud
```

### 6. Start the server

```bash
npm start
```

Open the dashboard:

```text
http://localhost:5000
```

### 7. Load the Chrome extension

1. Open Chrome.
2. Go to `chrome://extensions/`.
3. Enable **Developer mode**.
4. Click **Load unpacked**.
5. Select the `reelsidian-extension/` folder.
6. Pin the Reelsidian extension to your toolbar.

---

## Usage

### Capture a video

1. Open an educational Instagram Reel or YouTube Short.
2. Click the Reelsidian extension icon.
3. Click **Start Capture**.
4. Play or watch the video.
5. Click **Stop Capture**.

The extension sends the captured audio to your local server.

### Generate study material

Reelsidian will:

1. Transcribe the audio.
2. Analyze the transcript.
3. Generate a study report.
4. Save it to your local database.
5. Display it in the dashboard.

### Add learning goals

In the dashboard, add goals such as:

```text
Learn JavaScript fundamentals
Prepare for software engineering interviews
Build AI projects
```

Reelsidian links relevant study reports to these goals.

### Export your knowledge

You can export:

- Markdown notes
- Anki flashcards
- Transcript-based study material

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/process` | Upload audio and generate a study report |
| `POST` | `/api/analyze-text` | Analyze a transcript or caption directly |
| `GET` | `/api/reports` | Get all saved study reports |
| `GET` | `/api/goals` | Get all learning goals |
| `POST` | `/api/goals` | Add a learning goal |
| `DELETE` | `/api/goals/:id` | Delete a learning goal |
| `GET` | `/api/graph` | Get graph nodes and links |
| `GET` | `/api/events` | Real-time updates using Server-Sent Events |

---

## Privacy

Reelsidian is designed to be local-first.

- Audio is processed on your machine.
- Transcripts are stored locally.
- Study reports are stored locally.
- No ElevenLabs or Gemini API key is required in the local version.
- With a local Ollama model, your learning data does not leave your computer.

If you use an Ollama Cloud model such as `gemma4:cloud`, model inference is handled by Ollama Cloud. Use a locally pulled model for fully offline privacy.

---

## Roadmap

### Version 1 — Local Core
- Local audio transcription
- Ollama-powered study synthesis
- SQLite persistence
- Study dashboard
- Markdown and Anki export

### Version 2 — Knowledge System
- Goal-based knowledge graph
- Report search
- Report editing
- Goal editing
- Duplicate detection

### Version 3 — Smarter Learning
- Semantic transcript search
- Source-grounded flashcards
- Spaced repetition
- Learning-progress tracking

### Version 4 — Advanced Features
- Local-first Obsidian vault sync
- Multi-device sync
- Browser-history-aware recommendations
- Collaborative study spaces

---

## License

MIT License.
