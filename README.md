<<<<<<< HEAD
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
- Auto-creates a small FAISS index on first run
- No external outbound API calls
- Uses sentence-transformers for embeddings and FAISS for local search
- Entry point: `app.py` (forces `DEMO_MODE=true` for hosted platforms)

## Deploy
- Streamlit Community Cloud: set the main file to `app.py`, Python 3.10
- Hugging Face Spaces: SDK = Streamlit, `app_file = app.py`
=======
# VaultMind-Demo-
>>>>>>> 5d7c9cd668f247080625ddb9a25dcf376135abb6
