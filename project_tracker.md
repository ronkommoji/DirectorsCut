# DirectorsCut — Project Tracker

Backend (FastAPI, Supabase, Actian, Gemini) and frontend (refined elegance UI) with phases and mini milestones below.

---

## Backend

### B1 — DB and scrapers
- Document user_reviews and critic_reviews schema and keys
- Add movies table (full OMDB response) and plot_summary table (Wikipedia plot, keyed by movie_id)
- OMDB API integration (get-or-fetch by imdb_id or title; env OMDB_API_KEY)
- Add plot_beats, complaint_clusters, cluster_examples, what_if_suggestions, generations, votes
- IMDb scraper (POST scrape, GET reviews → user_reviews)
- Wikipedia plot scraper (fetch HTML, parse Plot section only); store in plot_summary
- Integration tests for DB and scrapers

### B2 — Embeddings and Actian
- Chunk user_reviews and critic_reviews (both); error handling when no critics
- Actian ingest (get-or-create, batch upsert by movie_id)
- FastAPI route(s) for embeddings and vector search
- Integration test (or mock)

### B3 — Clustering and what-if
- Dynamic clustering right after embedding; max 7 clusters per movie (frontend shows 5)
- Gemini cluster labeling; store clusters and examples
- Exactly 3 what-if from top clusters
- GET clusters and what-if by movie_id

### B4 — Plot and beats (Gemini)
- Feed Wikipedia plot (or OMDB fallback) to Gemini → expanded plot + structured beats
- Store and serve by movie_id

### B5 — Story engine (Gemini)
- One Gemini call per step (previous choices in context)
- Narrative under 8 sentences; tone adapts to plot/movie
- Strict JSON per step; Theme Coverage Score via Gemini (not Sphinx)

### B6 — Community
- Save generation; Share = Coming soon (not functional)
- Vote and leaderboard routes (sort by votes)

---

## Frontend

### F1 — Shell and design
- App shell and routing; design system (refined elegance palette)
- Visible loading for all backend-heavy flows

### F2 — Discovery and search
- Home: all 25 movies sorted by genre (Netflix-like); poster + Rotten Tomatoes score
- Search page and client-side movie match

### F3 — Analysis dashboard
- Movie analysis view; cluster/review/beats/what-if UI

### F4 — Rewrite flow
- Typing animation and 3-step choice flow
- Integration with story API

### F5 — Ending and score
- Ending page and score breakdown; evidence panel

### F6 — Community
- Save/vote; explore page sorted by votes; share button = Coming soon

---

## Testing

- Backend: Unit tests (scrapers, parsing); integration tests (Supabase, Actian or mock)
- Frontend: Key flows (select movie → analysis → what-if → rewrite → ending)

---

## Definition of done

25 movies; search; poster component (placeholder if no Poster); 3-step branching; Theme Coverage via Gemini; save/vote/explore (share = Coming soon); leaderboard by votes; FastAPI and scrapers as in plan.
