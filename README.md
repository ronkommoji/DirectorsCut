<p align="center">
  <h1 align="center">🎬 DirectorsCut</h1>
  <p align="center"><strong>What if critics could rewrite the ending?</strong></p>
  <p align="center">AI-powered alternate movie endings grounded in real critic reviews</p>
</p>

<p align="center">
  <a href="#-the-idea">The Idea</a> •
  <a href="#-how-it-works">How It Works</a> •
  <a href="#-try-it">Try It</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-whats-next">What's Next</a>
</p>

---

## 💡 The Idea

**Problem:** Audiences often agree with critics — rushed endings, plot holes, unsatisfying resolutions. But studios don’t have a way to turn that feedback into concrete alternatives.

**Solution:** DirectorsCut uses **vector search** to find the most relevant critic complaints for a movie, then **Gemini AI** to generate **3 alternate endings** grounded in that feedback. Every suggestion traces back to real reviews.

---

## ✨ How It Works

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Pick a     │────▶│  Vector search   │────▶│  Top 5 critic   │
│  movie      │     │  over 5,600+     │     │  review chunks  │
└─────────────┘     │  Rotten Tomatoes │     └────────┬────────┘
                    │  reviews         │              │
                    └──────────────────┘              ▼
                                             ┌─────────────────┐
                                             │  Pick a chunk   │
                                             └────────┬────────┘
                                                      │
                                                      ▼
┌─────────────────┐     ┌──────────────────┐  ┌─────────────────┐
│  3 alternate    │◀────│  Gemini 2.5      │◀─│  Top 3 similar  │
│  endings        │     │  Flash           │  │  chunks as      │
└─────────────────┘     └──────────────────┘  │  evidence       │
                                              └─────────────────┘
```

1. **Choose a movie** — We support 25 films (Jurassic Park, Avengers, Interstellar, La La Land, etc.) with 5,600+ critic excerpts.
2. **Get top critic complaints** — Semantic search finds reviews matching themes like *"ending rushed plot holes unsatisfying resolution"*.
3. **Generate endings** — Pick a chunk; we retrieve the top 3 similar reviews and ask Gemini to write 3 alternate endings grounded in that evidence.
4. **Trace the evidence** — Every ending links back to the exact review chunk it was based on.

---

## 🚀 Try It

### Live API (when server is running)

**Swagger UI:** [http://localhost:8000/docs](http://localhost:8000/docs)

### Quick Start (local)

```bash
cd backend
python3.11 -m venv .venv
source .venv/bin/activate

pip install ../actian-vectorAI-db-beta/actiancortex-0.1.0b1-py3-none-any.whl
pip install -r requirements.txt
pip install "protobuf>=6.31"   # if needed

cp .env.example .env   # Add GEMINI_API_KEY

uvicorn app:app --host 0.0.0.0 --port 8000
```

Then open **http://localhost:8000/docs** and try:

- **GET** `/movies/tt0107290/top-chunks` — Jurassic Park
- **GET** `/movies/Avengers%20Endgame/top-chunks` — by title
- **POST** `/movies/tt0107290/generate-ending` with `{"chunk_index": 0}`

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| API | FastAPI |
| Embeddings | Sentence-transformers (`all-MiniLM-L6-v2`, 384-dim, cosine) |
| Vector DB | Actian VectorAI DB (optional; falls back to in-memory mock) |
| LLM | Google Gemini 2.5 Flash |

**Data:** 5,600+ Rotten Tomatoes critic reviews across 25 popular films.

---

## 🎯 What's Next

- [ ] **Full frontend** — Movie selector, chunk browser, ending cards
- [ ] **User voting** — Rate which alternate ending is best
- [ ] **More films** — Expand beyond the top 25
- [ ] **Custom queries** — Let users specify what kind of feedback to search for
- [ ] **Export** — Save or share generated endings

---

## 📁 Repo Structure

```
DirectorsCut/
├── backend/           # FastAPI + vector search + Gemini
│   ├── app.py         # API server
│   └── ingest.py      # Load data into Actian
├── t25_critic_reviews.json   # 5,600+ critic excerpts
├── actian-vectorAI-db-beta/  # Actian VectorAI DB (Docker)
└── README.md
```

---

## 🏆 Built for Hackathon

DirectorsCut — *where critic feedback meets AI-powered rewrites.*

*Full rebuild spec: [SYSTEM_PROMPT_REBUILD.md](./SYSTEM_PROMPT_REBUILD.md)*
