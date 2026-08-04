
🛡️ Prompt Injection / Jailbreak Guardrail

An **AI input-safety layer** built with **LangChain**, **Chroma**, and **GPT-4o-mini**, featuring a **three-layer detection pipeline** and a simple **Gradio** interface. Every user message is screened for prompt injection and jailbreak attempts before it's allowed to reach a downstream LLM agent.

## 🚀 Overview

This project screens incoming user input through three independent detection layers regex, vector semantic search with RAG, and an LLM classifier before deciding whether to allow or block a request. The regex identify the fraud phrase from the prompt and block earlier before even pass to the second layer. The second layer run semantic search prompt against the business specific prompt injection/ jailbreak attempt vectore database and detect based on similarity score. The 3rd layer is LLM classifyer which use LLM model as identifyer and classify prompt as Jailbreak or prompt injection. Each layer runs in order and short-circuits as soon as one flags the input as unsafe. If any layer flags the input, returns a block message instead of proceeding to normal processing. Fianlly, all proces send to Splunk in each step for monitoring.
<img width="400" height="800" alt="architecture" src="https://github.com/user-attachments/assets/194688ad-b160-4910-8fbe-ee9cef9d1453" />
<svg width="100%" viewBox="0 0 1000 780" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <g style="font-family:'Anthropic Sans', -apple-system, 'system-ui', 'Segoe UI', sans-serif;">

  <!-- Title -->
  <text x="500" y="28" text-anchor="middle" style="fill:rgb(41,41,38);font-size:17px;font-weight:600;">Prompt Injection / Jailbreak Guardrail — Architecture</text>

  <!-- User Input -->
  <rect x="420" y="52" width="160" height="44" rx="8" stroke-width="0.5" style="fill:rgb(241,239,232);stroke:rgb(95,94,90);"/>
  <text x="500" y="74" text-anchor="middle" dominant-baseline="central" style="fill:rgb(68,68,65);font-size:14px;font-weight:500;">User input</text>

  <line x1="500" y1="96" x2="500" y2="128" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>

  <!-- Gradio UI -->
  <rect x="380" y="128" width="240" height="48" rx="8" stroke-width="0.5" style="fill:rgb(241,239,232);stroke:rgb(95,94,90);"/>
  <text x="500" y="147" text-anchor="middle" dominant-baseline="central" style="fill:rgb(68,68,65);font-size:14px;font-weight:500;">Gradio UI</text>
  <text x="500" y="163" text-anchor="middle" dominant-baseline="central" style="fill:rgb(95,94,90);font-size:11px;">App.py</text>

  <line x1="500" y1="176" x2="500" y2="208" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>

  <!-- run_agent -->
  <rect x="360" y="208" width="280" height="56" rx="8" stroke-width="0.5" style="fill:rgb(238,237,254);stroke:rgb(83,74,183);"/>
  <text x="500" y="230" text-anchor="middle" dominant-baseline="central" style="fill:rgb(60,52,137);font-size:14px;font-weight:500;">run_agent()</text>
  <text x="500" y="248" text-anchor="middle" dominant-baseline="central" style="fill:rgb(83,74,183);font-size:12px;">guardrail.py</text>

  <!-- run_agent -> Layer 1 -->
  <path d="M500 264 L500 288 L195 288 L195 310" fill="none" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>

  <!-- Layer 1 -->
  <rect x="80" y="310" width="230" height="60" rx="8" stroke-width="0.5" style="fill:rgb(225,245,238);stroke:rgb(15,110,86);"/>
  <text x="195" y="332" text-anchor="middle" dominant-baseline="central" style="fill:rgb(8,80,65);font-size:14px;font-weight:500;">Layer 1 — Rule-based</text>
  <text x="195" y="350" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:12px;">rule_based_check() · regex</text>

  <!-- Layer 1 -> Layer 2 -->
  <line x1="310" y1="340" x2="384" y2="340" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <text x="347" y="326" text-anchor="middle" style="fill:rgb(95,94,90);font-size:10px;">not flagged</text>

  <!-- Layer 2 -->
  <rect x="385" y="310" width="230" height="60" rx="8" stroke-width="0.5" style="fill:rgb(225,245,238);stroke:rgb(15,110,86);"/>
  <text x="500" y="330" text-anchor="middle" dominant-baseline="central" style="fill:rgb(8,80,65);font-size:14px;font-weight:500;">Layer 2 — Vector similarity</text>
  <text x="500" y="347" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:12px;">vector_check() · score &lt; 0.3</text>
  <text x="500" y="362" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:10px;">chroma_jailbreak store</text>

  <!-- Layer 2 -> Layer 3 -->
  <line x1="615" y1="340" x2="689" y2="340" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <text x="652" y="326" text-anchor="middle" style="fill:rgb(95,94,90);font-size:10px;">not flagged</text>

  <!-- Layer 3 -->
  <rect x="690" y="310" width="230" height="60" rx="8" stroke-width="0.5" style="fill:rgb(225,245,238);stroke:rgb(15,110,86);"/>
  <text x="805" y="330" text-anchor="middle" dominant-baseline="central" style="fill:rgb(8,80,65);font-size:14px;font-weight:500;">Layer 3 — LLM classifier</text>
  <text x="805" y="347" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:12px;">llm_check() · GPT-4o-mini</text>
  <text x="805" y="362" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:10px;">SAFE / INJECTION</text>

  <!-- Chroma vector store (offline build), dashed -->
  <line x1="500" y1="370" x2="500" y2="404" style="stroke:rgba(31,30,29,0.35);stroke-width:1.5px;stroke-dasharray:4 3;"/>
  <rect x="400" y="404" width="200" height="44" rx="8" stroke-width="0.5" stroke-dasharray="4 3" style="fill:rgb(250,238,218);stroke:rgb(133,79,11);"/>
  <text x="500" y="422" text-anchor="middle" dominant-baseline="central" style="fill:rgb(99,56,6);font-size:12px;font-weight:500;">Chroma vector store</text>
  <text x="500" y="437" text-anchor="middle" dominant-baseline="central" style="fill:rgb(133,79,11);font-size:10px;">built offline by Vector_DB.py</text>

  <!-- Layer 1 down to decision -->
  <line x1="195" y1="370" x2="195" y2="530" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <!-- Layer 3 down to decision -->
  <line x1="805" y1="370" x2="805" y2="530" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>

  <!-- Decision diamond -->
  <polygon points="500,494 572,532 500,570 428,532" stroke-width="0.5" style="fill:rgb(241,239,232);stroke:rgb(95,94,90);"/>
  <text x="500" y="536" text-anchor="middle" dominant-baseline="central" style="fill:rgb(68,68,65);font-size:12px;font-weight:500;">Blocked?</text>

  <line x1="195" y1="530" x2="428" y2="532" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <line x1="805" y1="530" x2="572" y2="532" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>

  <!-- Blocked outcome -->
  <line x1="428" y1="522" x2="230" y2="522" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <text x="330" y="512" text-anchor="middle" style="fill:rgb(95,94,90);font-size:10px;">yes</text>
  <rect x="60" y="546" width="200" height="52" rx="8" stroke-width="0.5" style="fill:rgb(253,237,237);stroke:rgb(183,28,28);"/>
  <text x="160" y="565" text-anchor="middle" dominant-baseline="central" style="fill:rgb(127,29,29);font-size:14px;font-weight:500;">Blocked</text>
  <text x="160" y="581" text-anchor="middle" dominant-baseline="central" style="fill:rgb(183,28,28);font-size:11px;">warning returned to user</text>

  <!-- Allowed outcome -->
  <line x1="572" y1="522" x2="740" y2="522" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <text x="656" y="512" text-anchor="middle" style="fill:rgb(95,94,90);font-size:10px;">no</text>
  <rect x="740" y="546" width="200" height="52" rx="8" stroke-width="0.5" style="fill:rgb(225,245,238);stroke:rgb(15,110,86);"/>
  <text x="840" y="565" text-anchor="middle" dominant-baseline="central" style="fill:rgb(8,80,65);font-size:14px;font-weight:500;">Allowed</text>
  <text x="840" y="581" text-anchor="middle" dominant-baseline="central" style="fill:rgb(15,110,86);font-size:11px;">passed to downstream agent</text>

  <!-- Splunk log (optional), branching from Blocked -->
  <line x1="160" y1="598" x2="160" y2="630" marker-end="url(#arrow)" style="stroke:rgb(115,114,108);stroke-width:1.5px;"/>
  <rect x="60" y="630" width="200" height="52" rx="8" stroke-width="0.5" style="fill:rgb(250,238,218);stroke:rgb(133,79,11);"/>
  <text x="160" y="649" text-anchor="middle" dominant-baseline="central" style="fill:rgb(99,56,6);font-size:14px;font-weight:500;">Splunk log</text>
  <text x="160" y="665" text-anchor="middle" dominant-baseline="central" style="fill:rgb(133,79,11);font-size:11px;">splunk_logger.py · optional</text>

  </g>
</svg>


 
## ✨ Features

- 🛡️ **Three-layer security guardrail** — regex, vector similarity, and LLM-based injection detection running in sequence on every input
- 📚 **Vector store** — a Chroma store built from a known injection/jailbreak dataset powers the similarity check
- 📊 **Splunk logging** — a ready-to-enable hook forwards blocked-request events with layer, score, and matched document
- 🖥️ **Gradio UI** — minimal chat-style text interface with optional public shareable link

## Architecture

![Architecture Diagram](architecture.svg)

`User Input → Rule-based Regex Check → Vector Similarity Check → LLM Classifier → Allow / Block`

 The vector similarity layer draws on a Chroma store that is built offline by `Vector_DB.py`, and blocked requests can optionally be forwarded to Splunk.

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

