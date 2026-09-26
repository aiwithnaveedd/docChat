# DocChat 🐥: Multi-Agent RAG System

DocChat is a verification-driven, multi-agent Retrieval-Augmented Generation (RAG) system built to parse complex, dense documents (such as research papers, financial filings, and technical reports) and provide fact-checked, hallucination-free answers.

Powered by **Docling**, **LangGraph**, **ChromaDB**, and **IBM watsonx.ai**, DocChat moves beyond naive RAG workflows by coordinating specialized agents to retrieve, analyze, fact-check, and self-correct responses.

---

## 🌟 Key Features

- **Advanced Document Parsing (Docling):** Accurately extracts structured tables, headers, and text from complex PDFs, DOCX, TXT, and Markdown files.
- **Hybrid Retrieval:** Combines lexical BM25 search with dense vector embeddings via ChromaDB and IBM watsonx embeddings (`ibm/slate-125m-english-rtrvr-v2`) to capture exact keyword matches alongside deep semantic context.
- **Multi-Agent Orchestration (LangGraph):**
  - **Relevance Checker:** Classifies incoming questions against document context (`CAN_ANSWER`, `PARTIAL`, `NO_MATCH`) to drop out-of-scope queries early.
  - **Research Agent:** Drafts an initial factual response using the most relevant retrieved passages.
  - **Verification Agent:** Evaluates the draft for factual grounding, lists unsupported claims or contradictions, and triggers self-correction loops when needed.
- **Interactive UI (Gradio):** Features a web interface with document uploads, sample questions, and a real-time verification report breakdown.

---

## 🏗️ Architecture


User Query + Documents
│
▼
[DocumentProcessor]  ── (Docling parsing & Markdown chunking)
│
▼
[Hybrid Retriever]    ── (BM25 Lexical + ChromaDB Semantic Vector Search)
│
▼
[Agent Workflow Graph]
│
├─► 1. Relevance Checker (Filters out-of-scope prompts)
│
├─► 2. Research Agent (Generates initial draft)
│
└─► 3. Verification Agent (Fact-checks output against source)
│



---

## 📂 Project Structure

```text
docchat/
├── agents/
│   ├── relevance_checker.py    # Classifies query-document scope
│   ├── research_agent.py       # Generates draft answers with watsonx Granite
│   ├── verification_agent.py   # Validates facts, detects hallucinations
│   └── workflow.py             # LangGraph state machine orchestration
├── config/
│   ├── constants.py            # Global limits and supported file extensions
│   └── settings.py             # Model and path configurations
├── document_processor/
│   └── file_handler.py         # Docling conversion, chunking, and file caching
├── retriever/
│   └── builder.py              # ChromaDB + BM25 EnsembleRetriever setup
├── utils/
│   └── logging.py              # Application logging setup
├── app.py                      # Main entrypoint and Gradio web interface
├── requirements.txt            # Project dependencies
└── README.md
├── If Unsupported / Contradictions ──► [Loop back to Research]
└── If Fully Supported              ──► [Return Answer & Report]

🚀 Getting Started
1. Prerequisites
Python 3.11+

Git

git clone [https://github.com/aiwithnaveedd/docChat.git](https://github.com/aiwithnaveedd/docChat.git)
cd docChat

Set Up Virtual Environment
Bash
python3 -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

Install Dependencies
Bash
pip install --upgrade pip
pip install -r requirements.txt


Note on PyTorch / Torchvision: If running on a CPU-only environment or encounter torch.hub issues, install matching CPU builds:
pip install torch torchvision --index-url [https://download.pytorch.org/whl/cpu](https://download.pytorch.org/whl/cpu)


Launch the Application
Bash
python app.py

Open your browser at http://127.0.0.1:5000 to interact with the Gradio interface.

🛠️ Tech Stack
Orchestration: LangGraph, LangChain

Document Processing: Docling

Vector Database: ChromaDB

Foundation Models: IBM watsonx.ai (ibm/granite-4-h-small, ibm/slate-125m-english-rtrvr-v2)

Frontend / UI: Gradio
