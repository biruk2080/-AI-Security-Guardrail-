
🛡️ Prompt Injection / Jailbreak Guardrail

An **AI input-safety layer** built with **LangChain**, **Chroma**, and **GPT-4o-mini**, featuring a **three-layer detection pipeline** and a simple **Gradio** interface. Every user message is screened for prompt injection and jailbreak attempts before it's allowed to reach a downstream LLM agent.

## 🚀 Overview

This project screens incoming user input through three independent detection layers regex, vector semantic search with RAG, and an LLM classifier before deciding whether to allow or block a request. The regex identify the fraud phrase from the prompt and block earlier before even pass to the second layer. The second layer run semantic search prompt against the business specific prompt injection/ jailbreak attempt vectore database and detect based on similarity score. The 3rd layer is LLM classifyer which use LLM model as identifyer and classify prompt as Jailbreak or prompt injection. Each layer runs in order and short-circuits as soon as one flags the input as unsafe. If any layer flags the input, returns a block message instead of proceeding to normal processing. Fianlly, all proces send to Splunk in each step for monitoring.

## Architecture

<img width="800" height="600" alt="architecture" src="https://github.com/user-attachments/assets/194688ad-b160-4910-8fbe-ee9cef9d1453" />
<svg width="100%" viewBox="0 0 1000 780" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <g style="font-family:'Anthropic Sans', -apple-system, 'system-ui', 'Segoe UI', sans-serif;">
  </g>
</svg>
 
## ✨ Features

- 🛡️ **Three-layer security guardrail** — regex, vector similarity, and LLM-based injection detection running in sequence on every input
- 📚 **Vector store** — a Chroma store built from a known injection/jailbreak dataset powers the similarity check
- 📊 **Splunk logging** — a ready-to-enable hook forwards blocked-request events with layer, score, and matched document
- 🖥️ **Gradio UI** — minimal chat-style text interface with optional public shareable link

## 🧠 Tech Stack

| Layer | Technology |
|---|---|
| LLM | OpenAI GPT-4o-mini |
| Agent Framework | LangChain |
| Retrieval / Vector Store | LangChain + PyPDFLoader + OpenAI Embeddings + Chroma |
| Security — Rule-based | Regex pattern matching |
| Security — Semantic | Chroma vector similarity |
| Security — LLM | GPT-4o-mini binary classifier (SAFE / INJECTION) |
| Monitoring | Splunk HEC (HTTP Event Collector) |
| UI | Gradio |

---

**Layer responsibilities:**

| Layer | Function | Trigger Condition |
|---|---|---|
| `rule_based_check` | Regex match against known injection phrases | Pattern match found |
| `vector_check` | Embeds input, compares to jailbreak vector store | Similarity score < 0.3 |
| `llm_check` | GPT-4o-mini classifies input as SAFE or INJECTION | Classifier returns "INJECTION" |

---

## 📊 Monitoring & Observability (Splunk)

`splunk_logger.py` provides a `send_to_splunk` helper that posts structured JSON events to a Splunk HEC endpoint. The call is currently commented out inside `run_agent` in `guardrail.py` — uncomment it to log blocked requests (layer, score, matched document) in real time.

---

## ⚙️ Installation

### 1. Clone the repo

```bash
git clone https://github.com/your-username/guardrail-chatbot.git
cd guardrail-chatbot
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate      # Mac / Linux
venv\Scripts\activate         # Windows
```

### 3. Install dependencies

```bash
pip install gradio langchain langchain-openai langchain-community langchain-chroma langchain-text-splitters python-dotenv requests
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```
OPENAI_API_KEY=your-openai-api-key
```

> ⚠️ **Security note:** `App.py` and `splunk_logger.py` currently contain hardcoded API keys / HEC tokens. Move these to environment variables and rotate the exposed keys before committing or deploying this project.

---

## ▶️ Usage

### 1. Build the vector stores (run once)

```bash
python Vector_DB.py
```

Loads source PDFs, splits them into chunks, embeds them with OpenAI embeddings, and persists two Chroma stores: `chroma_data` (general reference docs) and `chroma_jailbreak` (used by the guardrail's vector check).

### 2. Start the app

```bash
python App.py
```

Gradio launches a local interface (with `share=True`, so a public shareable link is also generated) where you can type input and see whether it's flagged as a prompt injection.

---

## 🗂️ Project Structure

```
guardrail-chatbot/
│
├── App.py               # Gradio interface entry point, wired to run_agent
├── guardrail.py          # Prompt injection detection (all 3 layers) + run_agent
├── Vector_DB.py           # Embeddings + Chroma vector store setup
├── splunk_logger.py       # Splunk HEC logging integration
├── data/                  # Source PDFs used to build vector stores
├── .env                   # Environment variables (not committed)
└── README.md
```

