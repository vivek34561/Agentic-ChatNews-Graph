# ✨Agentic AI Chatbot with LangGraph🧑‍💻

## 📌 Project Overview

This project is a **Streamlit-based Agentic AI application** that demonstrates **graph-driven AI workflows** using **LangGraph and LangChain**, powered by **GROQ LLMs** and integrated external tools.

It showcases how modern agentic systems can **reason, route, and act** across multiple capabilities instead of behaving like a single static chatbot.

The application includes **three core use cases**:

- 💬 **Basic Chatbot**  
  A simple conversational agent powered by an LLM.

- 🌐 **Chatbot with Web Tools**  
  A tool-augmented agent capable of invoking web/search tools when required.

- 📰 **AI News Agent**  
  Fetches recent AI-related news using Tavily, summarizes it with an LLM, and saves structured Markdown reports.

All core logic lives under `src/langgraphagenticai/`, while `app.py` bootstraps the Streamlit UI.

---

## 🧰 Tech Stack

- 🐍 **Python 3.13** (virtual environment in `chatbot_env/`)
- 🎛 **Streamlit** – interactive UI for chat and results
- 🔗 **LangGraph + LangChain** – graph-based agent orchestration
- ⚡ **GROQ LLM** – fast LLM inference backend  
  (configured in `src/langgraphagenticai/LLMS/groqllm.py`)
- 🔍 **Tavily API** – AI news search
- 📦 **Optional Libraries**  
  FAISS (vector ops), Jinja2, httpx/aiohttp, etc.

🔐 Environment variables (GROQ, Tavily API keys) are loaded from `.env`.  
**Do not commit secrets.**

---

## 🏗 Architecture

### 🔁 High-Level Flow

- `app.py`  
  - Launches Streamlit  
  - Loads UI controls  
  - Reads user input and use-case selection  
  - Configures LLMs  
  - Builds and executes the LangGraph workflow

- `graph_builder.py`  
  - Defines graph structures per use case  
  - Compiles graphs into executable workflows

- `nodes/`  
  - Contains task-specific agent nodes  
  - Each node exposes callable methods used as graph steps

- `ui/streamlitui/`  
  - UI helpers for inputs and result rendering

---

## 🧠 Graph Composition (Per Use Case)

### 💬 Basic Chatbot
- Single node: `BasicChatbotNode.process`
- Flow: `START → Chatbot → END`

### 🌐 Chatbot with Web
- Chatbot node + Tool node
- Conditional edges route between reasoning and tool execution

### 📰 AI News Pipeline
A structured **three-step agentic workflow**:

1. `AINewsNode.fetch_news` → Tavily search  
2. `AINewsNode.summarize_news` → LLM summarization  
3. `AINewsNode.save_result` → Save Markdown to  
   `AINews/<frequency>_summary.md`

---

## 🖥 Data & UI Handling

- 🎚 User input is collected via Streamlit controls (`LoadStreamlitUI`)
- 📄 Results are rendered using `DisplayResultStreamlit`
  - Chat messages for chatbot modes
  - Markdown previews for AI News summaries
- 🗂 AI News outputs are stored in the `AINews/` directory

---

## 📂 Directory Highlights

- `app.py`
- `src/langgraphagenticai/graph/graph_builder.py`
- `src/langgraphagenticai/nodes/ai_news_node.py`
- `src/langgraphagenticai/ui/streamlitui/display_result.py`

---

## ▶️ Running Locally

1️⃣ Create and activate a virtual environment (optional if using `chatbot_env/`)

2️⃣ Install dependencies:
```bash
pip install -r requirements.txt
````

3️⃣ Set required environment variables in `.env`
(example: GROQ and Tavily API keys)

4️⃣ Start the application:

```bash
streamlit run app.py
```

### 🪟 Windows Encoding Fix (if needed)

If you encounter encoding issues on Windows:

```powershell
$env:PYTHONUTF8=1
streamlit run app.py
```

```
```
