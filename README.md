# RAG_Testing_3.0

100% local-first PDF RAG (Retrieval-Augmented Generation) system for document question-answering.

## Tech Stack
- **n8n** — Workflow automation (self-hosted)
- **Ollama** — Local embeddings (`nomic-embed-text`) + fallback LLM (`gemma2:2b`)
- **Gemini 2.0 Flash** — Primary LLM for answer generation
- **Qdrant** — Local vector database
- **Flask** — Web UI backend
- **PyMuPDF** — PDF text extraction

## Quick Start

```bash
# 1. Start Qdrant
./scripts/start-qdrant.ps1

# 2. Start Ollama (ensure OLLAMA_HOST=0.0.0.0:11434)
ollama serve

# 3. Start n8n
n8n start

# 4. Start Web UI
python server.py
```

Open **http://localhost:3000** — upload PDFs and chat!

## Architecture
```
PDF Upload → PyMuPDF Extract → Chunk (500 chars) → Ollama Embed → Qdrant Store
Question → Ollama Embed → Qdrant Search → Gemini/Ollama Answer → Response
```

## Endpoints
| Endpoint | Method | Description |
|---|---|---|
| `http://localhost:3000` | GET | Web UI |
| `/api/upload` | POST | Upload & ingest PDF |
| `/api/chat` | POST | Ask a question |
| `/api/documents` | GET | List documents |
| `/api/documents/<id>` | DELETE | Remove document |

## Ports
- **3000** — Web UI (Flask)
- **5678** — n8n
- **6333** — Qdrant
- **11434** — Ollama
