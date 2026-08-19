# 🚀 DeepWriter AI: Autonomous Multi-Agent Technical Article Platform

[![Python Version](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![LangGraph](https://img.shields.io/badge/Orchestration-LangGraph-orange.svg)](https://www.langchain.com/langgraph)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-green.svg)](https://fastapi.tiangolo.com/)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-336791.svg)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**DeepWriter AI** is an autonomous, production-grade multi-agent technical writing platform built with **LangGraph**, **FastAPI**, and **PostgreSQL**.

It automates the complete technical content creation workflow—from real-time web research and structured outline planning to parallel section generation, content reduction, and technical visual/diagram planning.

---

## 🧠 What is DeepWriter AI?

Traditional LLM-based writing systems often rely on a single prompt and a single generation step. This can lead to:

* Context drift
* Hallucinations
* Repetitive content
* Poor section organization
* Limited research grounding
* Difficult-to-control long-form generation

DeepWriter AI solves these problems using a **multi-agent architecture** orchestrated through **LangGraph**.

The system dynamically routes the user's request, performs research when required, creates a structured writing plan, distributes sections across parallel worker agents, and finally combines everything into a publication-ready technical article.

---

## 🏗️ Architecture Overview

DeepWriter AI uses a state-machine architecture with dynamic parallel fan-out and a nested reducer subgraph.

```text
                       [User Input: Topic]
                                │
                                ▼
                       ┌─────────────────┐
                       │   Router Node   │
                       └────────┬────────┘
                                │
              ┌─────────────────┴─────────────────┐
              ▼                                   ▼
       [Open Book / Hybrid]                 [Closed Book]
              │                                   │
              ▼                                   │
     ┌───────────────────┐                        │
     │   Research Node   │                        │
     │  (Tavily Search)  │                        │
     └─────────┬─────────┘                        │
               │                                  │
               └────────────────┬─────────────────┘
                                ▼
                  ┌─────────────────────────┐
                  │ Orchestrator / Planner  │
                  │                         │
                  │ Dynamic Fan-Out via     │
                  │ LangGraph Send API      │
                  └────────────┬────────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
       ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
       │ Worker Node 1│ │ Worker Node 2│ │ Worker Node N│
       │              │ │              │ │              │
       │ Section      │ │ Section      │ │ Section      │
       │ Generation   │ │ Generation   │ │ Generation   │
       └──────┬───────┘ └──────┬───────┘ └──────┬───────┘
              │                │                │
              └────────────────┼────────────────┘
                               ▼
                  ┌─────────────────────────┐
                  │    Reducer Subgraph     │
                  │                         │
                  │ • Merge Content         │
                  │ • Preserve Ordering     │
                  │ • Plan Technical Visuals │
                  │ • Place Diagrams        │
                  └────────────┬────────────┘
                               │
                               ▼
                  [Final Structured Article]
                               │
                               ▼
                         [Markdown .md]
```

---

## ✨ Key Features

### 🧭 Intelligent Routing

Classifies incoming prompts into:

* `closed_book`
* `hybrid`
* `open_book`

This allows the system to determine whether external web research is required.

### 🌐 Automated Web Grounding

Uses the **Tavily Search API** to:

* Search the web for relevant information
* Retrieve real-world sources
* Gather research papers and technical references
* Deduplicate research results
* Provide source citations

### 📋 Deterministic Outline Planning

Uses strict **Pydantic V2 schemas** to generate structured article plans containing:

* Section titles
* Section goals
* Target word counts
* Section-specific bullet points
* Non-overlapping content requirements

This provides stronger control over long-form generation.

### ⚡ Dynamic Parallel Workers

Uses LangGraph's **`Send` API** to dynamically create multiple writer workers.

Each worker independently generates a specific article section, allowing multiple sections to be processed concurrently.

### 🔄 Nested Reducer Subgraph

The reducer subgraph:

* Collects parallel worker outputs
* Reconstructs the correct section order
* Merges generated content
* Plans technical visuals
* Places diagrams/images where appropriate
* Produces the final structured article

### 💾 Durable State Persistence

Uses PostgreSQL with **`PostgresSaver`** to persist LangGraph execution states.

This enables:

* Thread recovery
* Persistent graph state
* Long-running workflow support
* More reliable agent execution

### 📡 Real-Time SSE Streaming

The FastAPI backend uses **Server-Sent Events (SSE)** to stream agent activity directly to the frontend.

Users can observe:

* Agent stage transitions
* Research progress
* Retrieved citations
* Outline generation
* Section completion
* Final article assembly

in real time.

---

## 🛠️ Tech Stack

| Category                | Technology                    |
| ----------------------- | ----------------------------- |
| **Language**            | Python 3.11                   |
| **Agent Orchestration** | LangGraph                     |
| **LLM Framework**       | LangChain Core                |
| **LLM Providers**       | Google Gemini, Groq, Ollama   |
| **Web Research**        | Tavily Search API             |
| **Backend**             | FastAPI                       |
| **Server**              | Uvicorn                       |
| **Streaming**           | Server-Sent Events (SSE)      |
| **Database**            | PostgreSQL                    |
| **Persistence**         | `psycopg` v3, `PostgresSaver` |
| **Validation**          | Pydantic V2                   |
| **Frontend**            | HTML, CSS, JavaScript         |

---

## 🤖 Supported LLM Providers

DeepWriter AI is designed to work with multiple LLM providers.

### Google Gemini

* Gemini 2.5 Flash
* Google AI Studio API

### Groq

* Llama 3.3 70B
* Groq Cloud API

### Local Models

* Ollama-compatible local models

This provider flexibility makes it possible to switch between cloud and local inference depending on requirements.

---

## 🚀 Getting Started

### 1. Prerequisites

Make sure you have the following installed:

* Python 3.11+
* PostgreSQL
* Git
* A supported LLM API key

You will also need API keys for:

* **Google AI Studio** for Gemini
* **Groq Cloud** for Groq models
* **Tavily** for web research
* **LangSmith** *(optional, for observability and graph tracing)*

---

### 2. Clone the Repository

```bash
git clone https://github.com/ahmadnawaz01/Deep-Writer-AI-LangGraph-Ollama-Tavily-Subgraphs-FastApi-.git
cd Deep-Writer-AI-LangGraph-Ollama-Tavily-Subgraphs-FastApi-
```

---

### 3. Create a Virtual Environment

#### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

#### macOS / Linux

```bash
python -m venv venv
source venv/bin/activate
```

---

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 🔐 Environment Configuration

Create a `.env` file in the root directory of the project.

```env
# ==========================================
# LLM Providers
# ==========================================

GOOGLE_API_KEY=AIzaSy_your_gemini_api_key

# Optional
# GROQ_API_KEY=gsk_your_groq_api_key


# ==========================================
# Search & Tools
# ==========================================

TAVILY_API_KEY=tvly-your_tavily_api_key


# ==========================================
# PostgreSQL Checkpointer
# ==========================================

# Local PostgreSQL
DATABASE_URL=postgresql://postgres:password@localhost:5432/deepwriterai

# Cloud PostgreSQL example
# DATABASE_URL=postgresql://user:password@host.render.com/dbname?sslmode=require


# ==========================================
# LangSmith Observability (Optional)
# ==========================================

LANGCHAIN_TRACING_V2=true
LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
LANGCHAIN_API_KEY=lsv2_pt_your_langsmith_api_key
LANGCHAIN_PROJECT=deep-writer-ai
```

> **Important:** Never commit your `.env` file or expose API keys publicly.

Add the following to `.gitignore`:

```gitignore
.env
venv/
__pycache__/
*.pyc
```

---

## 🗄️ PostgreSQL Setup

DeepWriter AI uses PostgreSQL for durable LangGraph state persistence.

Create a PostgreSQL database:

```sql
CREATE DATABASE deepwriterai;
```

Then configure your connection string:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/deepwriterai
```

For cloud deployments, you can use a managed PostgreSQL provider such as Render or another PostgreSQL-compatible service.

---

## ▶️ Running the Application

### Start the FastAPI Server

```bash
python app.py
```

Or, if your application exposes an ASGI app named `app`:

```bash
uvicorn app:app --reload
```

The application should be available at:

```text
http://127.0.0.1:8000
```

Open the URL in your browser.

---

## 🖥️ Using DeepWriter AI

1. Open the web interface.
2. Enter a technical topic.
3. Click **Run Agent**.
4. The router determines the research strategy.
5. Research agents collect relevant information when required.
6. The planner creates a structured outline.
7. Worker agents generate article sections in parallel.
8. The reducer combines and organizes the generated sections.
9. Technical visuals and diagrams are planned.
10. The final Markdown article is generated.

The progress of the workflow is streamed to the browser in real time through SSE.

---

## 📂 Project Structure

```text
deep-writer-ai/
│
├── backend.py
│   └── Main LangGraph graph, state schemas, nodes,
│       routing, orchestration, and agent logic
│
├── app.py
│   └── FastAPI server and SSE streaming endpoints
│
├── requirements.txt
│   └── Python package dependencies
│
├── .env.example
│   └── Environment variable template
│
├── templates/
│   └── index.html
│       └── Interactive frontend UI
│
├── static/
│   ├── css/
│   │   └── style.css
│   │       └── Frontend styling
│   │
│   └── js/
│       └── app.js
│           └── EventSource / SSE client
│
├── images/
│   └── Generated technical diagrams and visuals
│
└── outputs/
    └── Generated publication-ready Markdown files
```

---

## 🔁 Workflow

The complete workflow can be summarized as:

```text
User Topic
    │
    ▼
Router
    │
    ├── Closed Book ─────────────┐
    │                            │
    └── Open Book / Hybrid       │
            │                    │
            ▼                    │
       Tavily Research           │
            │                    │
            └──────────┬─────────┘
                       ▼
                 Article Planner
                       │
                       ▼
                Dynamic Fan-Out
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
    Writer 1       Writer 2       Writer N
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                  Reducer
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       Content Merge       Visual Planning
             │                   │
             └─────────┬─────────┘
                       ▼
                Final Markdown
```

---

## 🧩 Multi-Agent Design

DeepWriter AI separates responsibilities across specialized stages instead of asking a single LLM to generate the entire article.

### Router Agent

Determines the appropriate research strategy.

```text
closed_book
hybrid
open_book
```

### Research Agent

Responsible for gathering external knowledge through Tavily.

### Planner Agent

Creates the article structure and determines:

* Sections
* Goals
* Word counts
* Writing requirements
* Section boundaries

### Worker Agents

Each worker focuses on one specific section.

This reduces context overload and enables parallel execution.

### Reducer Agent

Combines the independently generated sections into a coherent final article.

It also handles technical visual planning and final structural organization.

---

## ⚡ Why Parallel Execution?

A traditional sequential workflow may look like:

```text
Section 1 → Section 2 → Section 3 → Section 4
```

DeepWriter AI instead uses dynamic fan-out:

```text
             ┌── Section 1 ──┐
             │               │
             ├── Section 2 ──┤
Planner ─────┼── Section 3 ──┼──► Reducer
             │               │
             └── Section N ──┘
```

This architecture allows independent sections to be generated concurrently, improving scalability and reducing overall workflow latency.

---

## 📡 Real-Time Streaming

The FastAPI backend exposes an SSE-based streaming mechanism.

The frontend receives events such as:

```text
router_started
research_started
research_completed
planning_started
worker_started
worker_completed
reducer_started
article_completed
```

This creates a live agent-monitoring experience instead of forcing the user to wait for one large final response.

---

## 🔍 Research & Grounding

For `open_book` and `hybrid` workflows, DeepWriter AI uses Tavily to retrieve external information.

The research pipeline is designed to:

1. Generate search queries.
2. Search relevant sources.
3. Collect results.
4. Remove duplicate information.
5. Pass grounded context to downstream agents.
6. Preserve source references for citations.

This helps reduce hallucinations and improves the factual grounding of technical articles.

---

## 🛡️ Structured Output

DeepWriter AI uses **Pydantic V2** schemas to enforce structured outputs from planning and orchestration agents.

Instead of relying entirely on free-form text, the system expects predictable structures.

For example:

```text
Article Plan
│
├── Section
│   ├── Title
│   ├── Goal
│   ├── Target Word Count
│   └── Bullet Points
│
├── Section
│   ├── Title
│   ├── Goal
│   ├── Target Word Count
│   └── Bullet Points
│
└── ...
```

This improves reliability when passing information between multiple agents.

---

## 💾 Durable Execution

LangGraph state is persisted using PostgreSQL and `PostgresSaver`.

This provides a foundation for:

* Persistent conversations
* Thread-based execution
* Workflow recovery
* Long-running agent workflows
* State inspection
* More reliable production execution

---

## 📊 Observability

DeepWriter AI can optionally integrate with **LangSmith** for monitoring and debugging.

LangSmith can help inspect:

* Agent executions
* LLM calls
* Graph transitions
* Tool calls
* Latency
* Errors
* Token usage
* Intermediate states

Enable it through the `.env` configuration.

---

## 🔮 Future Improvements

Potential future enhancements include:

* [ ] Authentication and user accounts
* [ ] Multiple article export formats
* [ ] PDF and DOCX export
* [ ] Advanced citation management
* [ ] Custom writing styles
* [ ] User-defined agent workflows
* [ ] More LLM providers
* [ ] Streaming token-level generation
* [ ] Automatic fact verification
* [ ] Article quality scoring
* [ ] SEO optimization
* [ ] Automated plagiarism detection
* [ ] Human-in-the-loop review
* [ ] Cloud deployment
* [ ] Background job processing
* [ ] Agent performance analytics

---

## 🤝 Contributing

Contributions are welcome.

To contribute:

```bash
git checkout -b feature/your-feature
```

Make your changes, test them locally, and submit a pull request.

Please make sure your changes do not expose API keys or sensitive configuration.

---

## 📜 License

This project is licensed under the **Apache License 2.0**.

See the [`LICENSE`](LICENSE) file for more information.

---

## 👨‍💻 Author

**DeepWriter AI**

An autonomous multi-agent technical article generation platform powered by **LangGraph, LLMs, FastAPI, Tavily, and PostgreSQL**.

---

## ⭐ If You Like This Project

If you find **DeepWriter AI** useful or interesting, consider giving the repository a ⭐ on GitHub!

```text
Research → Plan → Parallel Write → Reduce → Visualize → Publish
```

**DeepWriter AI — Turning a single topic into a complete technical article through autonomous multi-agent collaboration.**
