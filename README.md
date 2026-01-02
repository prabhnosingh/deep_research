# Deep Research
 
## Tech Used
- Gradio (UI)
- python-dotenv (load environment variables)
- SendGrid (`sendgrid`) to send emails
- Pydantic (data models)
- asyncio (async orchestration)
- Internal `agents` helpers (wrappers around language models and tools)
- OpenAI-compatible models (used via the `agents` abstractions)

## Purpose
Deep Research is a small pipeline that automates web-research tasks: it plans searches for a query, performs web searches, synthesizes a long Markdown report, and emails the report. It also exposes a minimal Gradio UI to trigger the workflow.

## How it works (short)
1. The `ResearchManager` coordinates the pipeline: it asks the `planner_agent` for search terms, runs `search_agent` on each term, collects results, asks `writer_agent` to produce a full report, then calls `email_agent` to send the report.
2. Each agent is an `Agent` object from the local `agents` package; some agents use tools (e.g., `WebSearchTool`) and the writer/planner use structured Pydantic output models.
3. The Gradio app (`deep_research.py`) exposes a textbox and button; when run it starts the `ResearchManager` and streams progress and final report to the UI.

## Run locally (Windows)
1. Create a virtual environment and activate it (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install -r requirements.txt
```

3. Set required environment variables (example for SendGrid):

```powershell
$env:SENDGRID_API_KEY = "your_sendgrid_api_key_here"
```

Or create a `.env` file in the project root with:

```
SENDGRID_API_KEY=your_sendgrid_api_key_here
```

4. Launch the app:

```powershell
python deep_research.py
```

This opens the Gradio UI in your browser. Enter a query and run the pipeline.

## Notes & Next steps
- `requirements.txt` lists the detected external packages but does not pin versions; pin them by running `pip freeze > requirements.txt` after verifying compatibility.
- The project depends on a local `agents` package and access to an OpenAI-compatible service; ensure credentials/configuration for that service are provided.
- If you want, I can pin dependency versions and run a quick install test locally.
