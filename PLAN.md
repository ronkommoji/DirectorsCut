# DirectorsCut — Full Application Build Plan

## Current state

- **Docs:** prd.md, actian_vector_db.md, imbd_scraper.md
- **Existing DB (Supabase):** user_reviews (36k+ rows, 25 movies), critic_reviews (23k+ rows)
- **MVP scope:** 25 movies; search required; full pipeline: OMDB → user reviews → critic lookup → embed → Actian

---

## Part 1: Backend

### 1.1 Database architecture

- **user_reviews:** IMDb scraper only; key: movie_id (IMDb ID)
- **critic_reviews:** Read-only; lookup by imdb_id or title
- **movies:** From OMDB API; no re-fetch; placeholder if no Poster
- **plot_summary:** Separate table; Wikipedia Plot section (plain text)
- **plot_beats, complaint_clusters, cluster_examples, what_if_suggestions, generations, votes**

**Actian:** Embed user_reviews and critic_reviews (both); error handling when no critics; chunk 1–3 sentences.

### 1.2 Search-triggered pipeline

1. OMDB — fetch by title/year; show multiple results for user to pick
2. IMDb user reviews — use existing; check DB first; no duplications
3. Critic reviews — search by title; continue if none
4. Embed, chunk, vector — both sources; cluster right after (dynamic, max 7; frontend shows 5)
5. Loading: single long-running request; 5 min timeout

### 1.3 Story engine

- One Gemini call per step (previous choices in context)
- Narrative under 8 sentences; tone adapts to plot/movie
- Exactly 3 what-if from top clusters
- Theme Coverage via Gemini (not Sphinx)

### 1.4 Wikipedia plot scraper

- Fetch HTML and parse (web scraping); Plot section only
- Store in plot_summary table; no max length
- Fall back to OMDB Plot if no Wikipedia plot

### 1.5 Decisions locked

- Featured: all 25 movies, sorted by genre (Netflix-like), poster + RT score
- Share button: Coming soon (not functional)
- Explore/leaderboard: sort by votes

---

## Part 2: Frontend

F1 Shell/design | F2 Discovery (25 movies, genre sort, search) | F3 Analysis dashboard | F4 Rewrite flow (typing, 3 steps) | F5 Ending/score | F6 Community (save, vote, explore by votes, share = Coming soon)

---

## Diagram

```
IMDb → user_reviews ─┐
                      ├→ review_chunks → clusters → what_if
critic_reviews ──────┘
OMDB → movies
Wiki → plot_summary → plot_beats → what_if
```
