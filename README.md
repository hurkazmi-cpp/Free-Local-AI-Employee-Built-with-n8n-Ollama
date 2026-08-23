# 🤖 Free Local AI Employee — Built with n8n + Ollama

A fully local, **100% free** AI agent system — no API keys, no monthly subscriptions. Everything runs on your own machine using open-source tools.

🎥 **Demo video:** https://youtu.be/wLOBpZkRQpg

---

## ✨ What it can do

- 🧠 **Think** — powered by a local LLM (Llama 3.1) via [Ollama](https://ollama.com), running fully offline
- 🛠️ **Use tools** — Calculator and a live weather API (Open-Meteo)
- 📚 **Read documents (RAG)** — search and answer questions from your own PDFs, using [Qdrant](https://qdrant.tech) as a local vector database
- 🗂️ **Remember** — holds conversation context using persistent memory
- 💬 **Chat** — published as a live, shareable web chatbot
- 👥 **Delegate work** — a Manager agent routes tasks to specialized sub-agents (Researcher, Writer, Support) and combines their answers

---

## 🧱 Architecture

```
                 [Chat Trigger]
                       │
              [AI Agent — Ollama Brain]
                       │
       ┌───────────────┼──────────────────┐
   [Calculator]   [Weather API]   [Qdrant Vector Store]
                                     (RAG search over PDFs)
                       │
                  [Memory]

        ── Multi-Agent Layer ──

                [Manager Agent]
                       │
        ┌──────────────┼──────────────┐
   [Researcher]     [Writer]      [Support]
```

- **Main chatbot workflow** — handles direct chat, tools, RAG, and memory
- **Document loader workflow** — uploads a PDF, splits it, embeds it, and stores it in Qdrant
- **Manager Agent workflow** — delegates tasks to three specialized sub-agent workflows

---

## 🛠️ Tech Stack

| Piece | Tool | Cost |
|---|---|---|
| Orchestration | [n8n](https://n8n.io) | Free (self-hosted) |
| LLM | [Ollama](https://ollama.com) (`llama3.1`) | Free |
| Embeddings | Ollama (`nomic-embed-text`) | Free |
| Vector DB | [Qdrant](https://qdrant.tech) (Docker) | Free |
| Memory | Simple Memory / Postgres | Free |

---

## 🚀 Setup

### 1. Prerequisites
- [Docker](https://www.docker.com/) installed
- [Ollama](https://ollama.com) installed

### 2. Pull the local models
```bash
ollama pull llama3.1
ollama pull nomic-embed-text
```

### 3. Run n8n and Qdrant
```bash
docker run -p 5678:5678 n8nio/n8n
docker run -p 6333:6333 qdrant/qdrant
```

### 4. Import the workflows
- Open n8n at `http://localhost:5678`
- Import the workflow JSON files from the `/workflows` folder
- Point all Ollama nodes to `http://localhost:11434`
- Point all Qdrant nodes to `http://localhost:6333`

### 5. Load a document
- Open the **Document Loader** workflow
- Submit a PDF through the form trigger
- Confirm it appears under Collections at `http://localhost:6333/dashboard`

### 6. Publish and chat
- Publish the main chatbot workflow
- Open the generated chat link and start talking to your AI employee

---

## 📂 Repo Structure
```
/workflows
  ├── main-chatbot.json
  ├── document-loader.json
  ├── manager-agent.json
  ├── researcher-agent.json
  ├── writer-agent.json
  └── support-agent.json
README.md
```

---

## 💡 Notes
- All models run locally — response speed depends on your CPU/GPU.
- Voice input/output (Whisper + Piper) is a possible future addition, not included in this version.
- MCP (Model Context Protocol) support was explored but not included in the final build.

---

## 📜 License
MIT — free to use, modify, and share.
