# Agentic-AI-Chatbot-LangGraph

## Project Overview

This project is a Streamlit app that demonstrates agentic workflows built with LangGraph/LangChain, powered by GROQ LLMs and integrated tools. It includes three use cases:
- Basic Chatbot: simple conversational agent.
- Chatbot With Web: tool-augmented agent that can call search tools.
- AI News: fetches recent AI-related news via Tavily, summarizes with an LLM, and saves a Markdown report.

Key components live under `src/langgraphagenticai/`, with `app.py` bootstrapping the Streamlit UI.

## Tech Stack

- Python: 3.13 (virtual env in `chatbot_env/`)
- Streamlit: UI for chat and results
- LangGraph + LangChain: graph-based agent orchestration
- GROQ LLM: model provider (configured in `src/langgraphagenticai/LLMS/groqllm.py`)
- Tavily API: news search for the AI News use case
- Optional libs: FAISS (vector ops), Jinja2, httpx/aiohttp, etc.

Environment configuration is loaded from `.env` (API keys such as GROQ and Tavily). Do not commit secrets.

## Architecture

Top-level flow:
- `app.py`: Starts Streamlit, loads UI, reads user selections and input, configures LLM, and builds the graph.
- `src/langgraphagenticai/graph/graph_builder.py`: Defines graph setup per use case and compiles it.
- `src/langgraphagenticai/nodes/`: Task-specific nodes—each node exposes methods used as graph steps.
- `src/langgraphagenticai/ui/streamlitui/`: UI helpers (load controls, render chat/results).

Graph composition (per use case):
- Basic Chatbot: `BasicChatbotNode.process` is the single node; edges: START → chatbot → END.
- Chatbot With Web: chatbot node + tool node; conditional edges route between chatbot and tools.
- AI News: three-step pipeline:
  1) `AINewsNode.fetch_news` → Tavily search
  2) `AINewsNode.summarize_news` → LLM summary
  3) `AINewsNode.save_result` → writes Markdown to `AINews/<frequency>_summary.md`

Data & UI:
- UI input is collected via Streamlit controls (`LoadStreamlitUI`).
- Results are displayed using `DisplayResultStreamlit`, which renders chat messages or Markdown files.
- AI News output files reside in `AINews/`.

### Directory Highlights

- `app.py`
- `src/langgraphagenticai/graph/graph_builder.py`
- `src/langgraphagenticai/nodes/ai_news_node.py`
- `src/langgraphagenticai/ui/streamlitui/display_result.py`

### Running Locally

1) Create/activate a virtual environment (optional if `chatbot_env/` is used).
2) Install dependencies:
	```bash
	pip install -r requirements.txt
	```
3) Set required environment variables in `.env` (e.g., GROQ and Tavily API keys).
4) Start the app:
	```bash
	streamlit run app.py
	```

If using Windows and you encounter encoding issues, consider forcing UTF-8:
```powershell
$env:PYTHONUTF8=1
streamlit run app.py
```
