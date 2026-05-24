<p align="center">
  <img src="https://img.shields.io/badge/OmniAgent-v2.0-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/FastAPI-0.109-009688?style=for-the-badge&logo=fastapi" />
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react" />
  <img src="https://img.shields.io/badge/Claude-Sonnet-purple?style=for-the-badge" />
  <img src="https://img.shields.io/badge/RAG-ChromaDB-orange?style=for-the-badge" />
</p>

# 🤖 OmniAgent — AI-Powered Life Assistant

> **Control Instagram, LinkedIn, Telegram, WhatsApp, GitHub, Email, and Calendar from a single chat window — powered by Claude AI with RAG-augmented knowledge.**

OmniAgent is a production-ready multi-platform AI assistant that combines **Retrieval-Augmented Generation (RAG)**, **conversation memory**, and **multi-agent orchestration** to autonomously execute actions across 9+ platforms from natural language commands.

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 🧠 **RAG Knowledge Base** | Upload documents → automatically retrieved as context during AI chat |
| 💬 **Multi-Platform Control** | Telegram, WhatsApp, Instagram, LinkedIn, GitHub, Email, Calendar |
| 🔄 **Conversation Memory** | Sliding window + summary compression for long conversations |
| ⚡ **Real-Time WebSocket** | Bidirectional chat with typing indicators and optimistic UI |
| 🔐 **Enterprise Security** | JWT auth, Fernet encryption, bcrypt hashing, rate limiting |
| 📊 **Analytics Dashboard** | Usage stats, platform activity, agent metrics |
| 🐳 **Docker Ready** | One-command deployment with docker-compose |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      OMNIAGENT v2.0                              │
├───────────────┬─────────────────────────────────────────────────┤
│   Frontend    │  React 18 + Vite + TailwindCSS + Zustand        │
│               │  ├── Chat (WebSocket real-time)                  │
│               │  ├── Knowledge Base (upload/search/manage)       │
│               │  ├── Dashboard (analytics + platform status)     │
│               │  └── Auth (register/login/JWT)                   │
├───────────────┼─────────────────────────────────────────────────┤
│   API Layer   │  FastAPI + middleware stack                       │
│               │  ├── /api/v1/auth/*                              │
│               │  ├── /api/v1/chat/* (WebSocket + REST)           │
│               │  ├── /api/v1/knowledge/* (RAG CRUD)              │
│               │  ├── /api/v1/platforms/*                         │
│               │  ├── /api/v1/analytics/*                         │
│               │  └── /api/v1/payments/*                          │
├───────────────┼─────────────────────────────────────────────────┤
│   AI Core     │  Claude Sonnet + RAG Pipeline                    │
│               │  ├── ChromaDB Vector Store (persistent)          │
│               │  ├── Sentence-Transformers Embeddings            │
│               │  ├── Semantic Retriever (MMR)                    │
│               │  ├── Conversation Memory Manager                 │
│               │  └── JSON Tool-Calling Agent                     │
├───────────────┼─────────────────────────────────────────────────┤
│   Data Layer  │  MongoDB + ChromaDB + Redis                      │
│               │  ├── Users, Chats, Tokens (MongoDB)              │
│               │  ├── Vector Embeddings (ChromaDB)                │
│               │  └── Cache + Task Queue (Redis)                  │
└───────────────┴─────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites
- Python 3.9+
- Node.js 18+
- [Anthropic API Key](https://console.anthropic.com) (required for AI)

### 1. Clone & Setup Backend

```bash
cd omniagent-backend

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env → add your ANTHROPIC_API_KEY
```

### 2. Start Backend

```bash
cd omniagent-backend
source venv/bin/activate
uvicorn app.main:app --reload --port 8000
```

You'll see:
```
🚀 Starting OmniAgent Backend v2.0...
✅ Database ready
✅ Vector store ready (ChromaDB)
✅ Embedding model ready
🎯 OmniAgent Backend ready — docs at /docs
```

### 3. Start Frontend

```bash
cd omniagent-frontend
npm install
npm run dev
```

### 4. Open the App

→ **Frontend:** http://localhost:5173  
→ **API Docs:** http://localhost:8000/docs  
→ **Health Check:** http://localhost:8000/health

---

## 🧠 RAG Pipeline

OmniAgent uses **Retrieval-Augmented Generation** to give the AI persistent knowledge:

```
Document Upload → Text Splitting → Embedding → ChromaDB Storage
                                                      ↓
User Query → Semantic Search → Top-K Retrieval → Context Injection → Claude → Response
```

### How It Works

1. **Upload** documents via the Knowledge Base UI or API
2. Documents are **chunked** (512 tokens, 64 overlap) and **embedded** using `all-MiniLM-L6-v2`
3. Embeddings are stored in **ChromaDB** (persistent, per-user collections)
4. During chat, relevant chunks are **automatically retrieved** and injected as context
5. Claude responds with **knowledge-augmented answers** and cites sources

### Supported Formats
- Plain text (`.txt`)
- Markdown (`.md`)
- Paste text directly in the UI

### API Endpoints

```
POST /api/v1/knowledge/ingest/text   → Upload text document
POST /api/v1/knowledge/ingest/file   → Upload file (txt/md)
GET  /api/v1/knowledge/documents     → List all documents
POST /api/v1/knowledge/search        → Semantic search
DEL  /api/v1/knowledge/documents/:id → Delete document
GET  /api/v1/knowledge/stats         → Collection stats
```

---

## 💬 Conversation Memory

OmniAgent maintains context across conversations using a **hybrid memory strategy**:

| Component | Purpose |
|-----------|---------|
| **Sliding Window** | Last 12 messages kept verbatim for immediate context |
| **Summary Compression** | Older messages compressed into bullet-point summaries |
| **LLM Summarization** | Optional Claude-powered compression for richer context |

---

## 🔌 Platform Agents

| Platform | Actions |
|----------|---------|
| **Telegram** | Send messages, channel posts, group summaries, danger detection, auto-reply |
| **WhatsApp** | Send messages, smart reply, status posts, group summaries |
| **Instagram** | Post images, generate captions, content ideas, video scripts, insights |
| **LinkedIn** | Post updates, profile rewriting, job search, auto-apply, grow impressions |
| **GitHub** | Create/close issues, PR deadlines, README generation, code error detection |
| **Email** | Send/read emails, search, draft generation, thread summaries |
| **Calendar** | Create events, find free slots, generate agendas |

---

## 📁 Project Structure

```
omniagent/
├── omniagent-backend/
│   ├── app/
│   │   ├── main.py                 # FastAPI app with middleware
│   │   ├── config.py               # Pydantic settings
│   │   ├── database.py             # MongoDB + in-memory fallback
│   │   ├── dependencies.py         # FastAPI dependency injection
│   │   ├── exceptions.py           # Custom exception hierarchy
│   │   ├── security.py             # JWT, bcrypt, sanitization
│   │   ├── ai/
│   │   │   ├── brain.py            # AI Brain (RAG + memory + Claude)
│   │   │   ├── prompts.py          # Centralized prompt templates
│   │   │   ├── rag/
│   │   │   │   ├── embeddings.py   # Sentence-transformers adapter
│   │   │   │   ├── vector_store.py # ChromaDB manager
│   │   │   │   ├── ingestion.py    # Document chunking pipeline
│   │   │   │   └── retriever.py    # Semantic retrieval
│   │   │   └── memory/
│   │   │       └── manager.py      # Conversation memory
│   │   ├── agents/                 # Platform agents
│   │   ├── middleware/             # Error handler, request logger
│   │   ├── models/                 # Pydantic schemas
│   │   ├── routers/                # API endpoints
│   │   │   ├── auth.py
│   │   │   ├── chat.py             # WebSocket + REST
│   │   │   ├── knowledge.py        # RAG document management
│   │   │   ├── analytics.py        # Usage statistics
│   │   │   ├── platforms.py
│   │   │   └── payments.py
│   │   └── utils/
│   │       └── encryption.py       # Fernet token encryption
│   ├── chroma_data/                # ChromaDB persistent storage
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .env
│
├── omniagent-frontend/
│   ├── src/
│   │   ├── App.jsx                 # Routes + auth guards
│   │   ├── pages/
│   │   │   ├── Landing.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Chat.jsx            # WebSocket AI chat
│   │   │   └── KnowledgeBase.jsx   # RAG document management
│   │   ├── components/
│   │   │   └── Sidebar.jsx         # Collapsible navigation
│   │   ├── services/
│   │   │   ├── api.js              # Axios + JWT interceptors
│   │   │   └── websocket.js        # WebSocket client
│   │   └── store/
│   │       └── useStore.js         # Zustand state management
│   ├── Dockerfile
│   └── package.json
│
└── docker-compose.yml              # Full stack deployment
```

---

## 🔧 Environment Variables

```env
# Required
ANTHROPIC_API_KEY=your_key_here     # Claude AI
JWT_SECRET=random_secret_string      # Auth
ENCRYPTION_KEY=fernet_key            # Token encryption

# Optional (for persistent storage)
MONGODB_URL=mongodb+srv://...        # Falls back to in-memory DB

# Optional (for platform connections)
TELEGRAM_BOT_TOKEN=
GITHUB_TOKEN=
WHATSAPP_TOKEN=
META_ACCESS_TOKEN=
LINKEDIN_CLIENT_ID=
```

---

## 🐳 Docker Deployment

```bash
# Build and start all services
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker-compose logs -f backend
```

---

## 🌐 Deploy to Cloud

### Railway (Backend)
1. Connect your GitHub repo
2. Set environment variables in Railway dashboard
3. Railway auto-detects the `Procfile`

### Vercel (Frontend)
1. Import the `omniagent-frontend` directory
2. Set `VITE_API_URL` to your Railway backend URL
3. Deploy

---

## 📊 API Documentation

Once running, visit:
- **Swagger UI:** http://localhost:8000/docs
- **ReDoc:** http://localhost:8000/redoc

---

## 🛡 Security

| Feature | Implementation |
|---------|---------------|
| Password hashing | bcrypt via passlib |
| Auth tokens | JWT (HS256, 24h expiry) |
| Token encryption | Fernet symmetric encryption |
| Input sanitization | HTML/script tag stripping |
| Rate limiting | slowapi (100 req/min API, 20 req/min auth) |
| CORS | Whitelist-based origin policy |

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  Built with ❤️ using FastAPI, React, Claude AI, and ChromaDB
</p>
