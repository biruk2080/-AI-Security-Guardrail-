<img width="242" height="150" alt="architecture" src="https://github.com/user-attachments/assets/c4d5a157-8214-4c58-b7dc-3535e39c30ea" />
🛡️ Prompt Injection / Jailbreak Guardrail

An **AI input-safety layer** built with **LangChain**, **Chroma**, and **GPT-4o-mini**, featuring a **three-layer detection pipeline** and a simple **Gradio** interface. Every user message is screened for prompt injection and jailbreak attempts before it's allowed to reach a downstream LLM agent.

## 🚀 Overview

This project screens incoming user input through three independent detection layers regex, vector semantic search with RAG, and an LLM classifier before deciding whether to allow or block a request. The regex identify the fraud phrase from the prompt and block earlier before even pass to the second layer. The second layer run semantic search prompt against the business specific prompt injection/ jailbreak attempt vectore database and detect based on similarity score. The 3rd layer is LLM classifyer which use LLM model as identifyer and classify prompt as Jailbreak or prompt injection. Each layer runs in order and short-circuits as soon as one flags the input as unsafe. If any layer flags the input, returns a block message instead of proceeding to normal processing. Fianlly, all proces send to Splunk in each step for monitoring.
<svg viewBox="0 0 1000 620" xmlns="http://www.w3.org/2000/svg" font-family="Helvetica, Arial, sans-serif">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#4b5563"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="1000" height="620" fill="#ffffff"/>

  <text x="500" y="35" text-anchor="middle" font-size="22" font-weight="700" fill="#111827">Prompt Injection / Jailbreak Guardrail — Architecture</text>

  <!-- User -->
  <rect x="420" y="65" width="160" height="50" rx="8" fill="#eef2ff" stroke="#6366f1" stroke-width="2"/>
  <text x="500" y="95" text-anchor="middle" font-size="14" fill="#312e81">User Input</text>

  <!-- Gradio UI -->
  <rect x="400" y="140" width="200" height="50" rx="8" fill="#fef3c7" stroke="#d97706" stroke-width="2"/>
  <text x="500" y="170" text-anchor="middle" font-size="14" fill="#78350f">Gradio UI (App.py)</text>

  <line x1="500" y1="115" x2="500" y2="140" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>

  <!-- run_agent -->
  <rect x="380" y="215" width="240" height="50" rx="8" fill="#f3f4f6" stroke="#374151" stroke-width="2"/>
  <text x="500" y="245" text-anchor="middle" font-size="14" fill="#111827">run_agent() — guardrail.py</text>
  <line x1="500" y1="190" x2="500" y2="215" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>

  <!-- Layer 1 -->
  <rect x="80" y="310" width="230" height="65" rx="8" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="195" y="335" text-anchor="middle" font-size="13" font-weight="600" fill="#14532d">Layer 1 — Rule-based</text>
  <text x="195" y="353" text-anchor="middle" font-size="11" fill="#166534">Regex pattern match</text>

  <!-- Layer 2 -->
  <rect x="385" y="310" width="230" height="65" rx="8" fill="#dbeafe" stroke="#2563eb" stroke-width="2"/>
  <text x="500" y="332" text-anchor="middle" font-size="13" font-weight="600" fill="#1e3a8a">Layer 2 — Vector Similarity</text>
  <text x="500" y="350" text-anchor="middle" font-size="11" fill="#1e40af">score &lt; 0.3 → flagged</text>
  <text x="500" y="366" text-anchor="middle" font-size="10" fill="#1e40af">(Chroma: chroma_jailbreak)</text>

  <!-- Layer 3 -->
  <rect x="690" y="310" width="230" height="65" rx="8" fill="#fee2e2" stroke="#dc2626" stroke-width="2"/>
  <text x="805" y="332" text-anchor="middle" font-size="13" font-weight="600" fill="#7f1d1d">Layer 3 — LLM Classifier</text>
  <text x="805" y="350" text-anchor="middle" font-size="11" fill="#991b1b">GPT-4o-mini</text>
  <text x="805" y="366" text-anchor="middle" font-size="10" fill="#991b1b">SAFE / INJECTION</text>

  <!-- sequential arrows between layers -->
  <line x1="500" y1="265" x2="500" y2="280" stroke="#4b5563" stroke-width="2"/>
  <line x1="500" y1="280" x2="195" y2="280" stroke="#4b5563" stroke-width="2"/>
  <line x1="195" y1="280" x2="195" y2="310" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>

  <line x1="310" y1="342" x2="385" y2="342" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="347" y="332" text-anchor="middle" font-size="10" fill="#4b5563">not flagged</text>

  <line x1="615" y1="342" x2="690" y2="342" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="652" y="332" text-anchor="middle" font-size="10" fill="#4b5563">not flagged</text>

  <!-- Chroma vector store box below layer 2 -->
  <rect x="405" y="410" width="190" height="45" rx="6" fill="#eff6ff" stroke="#93c5fd" stroke-width="1.5" stroke-dasharray="4 3"/>
  <text x="500" y="437" text-anchor="middle" font-size="11" fill="#1e40af">Chroma: chroma_jailbreak</text>
  <line x1="500" y1="375" x2="500" y2="410" stroke="#93c5fd" stroke-width="1.5" stroke-dasharray="4 3"/>

  <!-- Vector_DB.py build step -->
  <rect x="130" y="410" width="230" height="45" rx="6" fill="#f0fdf4" stroke="#86efac" stroke-width="1.5" stroke-dasharray="4 3"/>
  <text x="245" y="432" text-anchor="middle" font-size="11" fill="#166534">Built offline by Vector_DB.py</text>
  <text x="245" y="447" text-anchor="middle" font-size="9" fill="#166534">(source PDFs → chunks → embeddings)</text>
  <line x1="245" y1="410" x2="245" y2="375" stroke="#86efac" stroke-width="1.5" stroke-dasharray="4 3"/>

  <!-- Decision diamond -->
  <polygon points="500,470 570,510 500,550 430,510" fill="#fafafa" stroke="#374151" stroke-width="2"/>
  <text x="500" y="514" text-anchor="middle" font-size="12" fill="#111827">Blocked?</text>

  <line x1="195" y1="375" x2="195" y2="510" stroke="#4b5563" stroke-width="2"/>
  <line x1="195" y1="510" x2="430" y2="510" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>

  <line x1="805" y1="375" x2="805" y2="510" stroke="#4b5563" stroke-width="2"/>
  <line x1="805" y1="510" x2="570" y2="510" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>

  <!-- Yes -> Block message -->
  <rect x="60" y="485" width="200" height="50" rx="8" fill="#fee2e2" stroke="#dc2626" stroke-width="2"/>
  <text x="160" y="507" text-anchor="middle" font-size="12" font-weight="600" fill="#7f1d1d">⚠️ Blocked</text>
  <text x="160" y="523" text-anchor="middle" font-size="10" fill="#991b1b">Warning returned to user</text>
  <line x1="430" y1="500" x2="260" y2="500" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="345" y="490" text-anchor="middle" font-size="10" fill="#4b5563">yes</text>

  <!-- No -> Proceed -->
  <rect x="740" y="485" width="200" height="50" rx="8" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="840" y="507" text-anchor="middle" font-size="12" font-weight="600" fill="#14532d">✅ Allowed</text>
  <text x="840" y="523" text-anchor="middle" font-size="10" fill="#166534">Proceeds to downstream agent</text>
  <line x1="570" y1="500" x2="740" y2="500" stroke="#4b5563" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="655" y="490" text-anchor="middle" font-size="10" fill="#4b5563">no</text>

  <!-- Splunk logging (optional, dashed) -->
  <rect x="60" y="570" width="220" height="45" rx="6" fill="#fdf4ff" stroke="#a855f7" stroke-width="1.5" stroke-dasharray="4 3"/>
  <text x="170" y="592" text-anchor="middle" font-size="11" fill="#6b21a8">Splunk HEC (optional)</text>
  <text x="170" y="606" text-anchor="middle" font-size="9" fill="#6b21a8">splunk_logger.py — currently disabled</text>
  <line x1="160" y1="535" x2="160" y2="570" stroke="#a855f7" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#arrow)"/>
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

