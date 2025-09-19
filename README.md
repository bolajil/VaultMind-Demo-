# VaultMind Demo

Run a zero-config demo of VaultMind GenAI Knowledge Assistant.

## Quick Start

Windows PowerShell:

```powershell
$env:DEMO_MODE='true'; streamlit run enhanced_streamlit_app.py
```

macOS/Linux:

```bash
DEMO_MODE=true streamlit run enhanced_streamlit_app.py
```

## About Demo Mode
- Auto-creates a tiny local FAISS index from `utils/demo_mode.py` on first run (no external services required)
- Retrieval uses sentence-transformers (`all-MiniLM-L6-v2`) + FAISS for fast local similarity search
- Ingestion and outbound API calls are disabled for a safe, zero-config public demo
- Optional: LLM synthesis can be enabled via Streamlit Secrets or a local `.env` (see below)
- Entry point: `app.py` forces `DEMO_MODE=true` on hosted platforms

## What is VaultMIND (Demo)?
A lightweight Streamlit UI that demonstrates knowledge retrieval over a small built‑in corpus. It showcases:
- Clean UI/UX with navigation: Home, Ingest (disabled in demo), Search, Documents, Analytics
- Local vector search backed by FAISS with precomputed embeddings
- Optional LLM answer synthesis layered on top of retrieved sources

## Features
- Demo index auto-creation at startup under `data/indexes/demo_index/`
- Document listing with chunk counts and metadata
- Query UI with adjustable Top‑K and relevance threshold
- Optional LLM answer synthesis using OpenAI‑compatible APIs (OpenAI, DeepSeek, Groq)
- System Status panel with Vector DB and LLM readiness

## Project Structure
- `app.py` — Hosted entrypoint that sets `DEMO_MODE=true` and runs the UI
- `enhanced_streamlit_app.py` — Main Streamlit app (pages, status, search, LLM synthesis)
- `utils/` — Demo utilities
  - `demo_mode.py` — Builds the tiny demo FAISS index from an embedded text
  - `vector_search_with_embeddings.py` — FAISS load/search helpers
  - `embedding_generator.py` — Chunking, embedding generation, index build
- `requirements.txt` — Minimal pinned dependencies for the demo

## Run Locally
1) Create/activate a Python 3.10+ environment
2) `pip install -r requirements.txt`
3) Start the app:
    - PowerShell: `$env:DEMO_MODE='true'; streamlit run enhanced_streamlit_app.py`
    - macOS/Linux: `DEMO_MODE=true streamlit run enhanced_streamlit_app.py`
4) The first run creates the demo index; subsequent runs reuse it

## Enable LLM Answer Synthesis (optional)
The demo supports OpenAI‑compatible chat APIs without extra SDKs. Provide one or more keys:
- `OPENAI_API_KEY` or `DEEPSEEK_API_KEY` or `GROQ_API_KEY`
- Optional model names: `OPENAI_MODEL` (default `gpt-4o-mini`), `DEEPSEEK_MODEL` (default `deepseek-chat`), `GROQ_MODEL` (default `llama-3.1-70b-versatile`)

Ways to provide secrets:
- Streamlit Cloud: App → Settings → Secrets → add keys; app redeploys automatically
- Local: put keys in the repo root `.env`. The app automatically loads it via `dotenv`

Once a key is present, the sidebar will show `LLM Service: ready` and the Search page will display an `Answer` above sources.

## Troubleshooting
- Vector DB shows Not Ready in sidebar: click `Refresh Status`. The main panel is authoritative.
- No results returned: lower the `Relevance Threshold` to 0.3–0.4 and retry. The app also falls back to the top result in demo mode.
- No `Answer` appears: ensure at least one LLM key is configured and pick the matching provider in Search Settings.
- Slow first run: sentence‑transformers may download a small model once; subsequent runs are fast.

## Security Notes
- Do not commit API keys. Use Streamlit Secrets or a local `.env` ignored by git.
- If a key is ever exposed, rotate it immediately in the provider dashboard.

## Deploy
- Streamlit Community Cloud: main file = `app.py`, Python 3.10
- Hugging Face Spaces: SDK = Streamlit, `app_file = app.py`
