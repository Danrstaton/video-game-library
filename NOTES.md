# Video Game Library — Project state

## Repo
- App: index.html (single-file React via Babel standalone + Tailwind CDN)
- Cloudflare Worker: worker.js → deployed at https://vgl-news.danrstaton.workers.dev
- Static site hosted via GitHub Pages from main branch

## Storage keys (localStorage)
- vgl.games.v4              — library JSON
- vgl.news.v2               — cached worker response
- vgl.readArticles.v1       — set of article IDs marked read
- vgl.dismissedBanners.v1   — set of event banner IDs dismissed
- vgl.gistSync.v1           — { token, gistId, gistUrl, lastSyncedAt }

## Worker endpoints
- GET /         health check
- GET /news     full bundle: { fetchedAt, headlines, podcasts, events }
- GET /article  ?url=... fetches + extracts article body
- GET /debug    diagnostics for State of Play / Direct detection

## RAWG
- API key inlined in index.html
- Auto-enriches games on first load (covers, dates, platforms)
- Manual COVER_OVERRIDES map handles RAWG mismatches

## Done so far
- Library sections: Top 50 with Cover Flow / Playing / Upcoming / Rumored / Recommended / Played
- 10-category rating with auto-rerank on score change (Masterpiece 100 / Amazing 90-99 / Great 80-89)
- Top 50 floor: games dropping below 80 auto-lose their topListRank
- News: live feed from Worker (Nintendo Life, PlayStation Blog, Polygon, IGN, Engadget, Push Square, GamesRadar+, Vice) + Wikipedia events + KFGD/KFG from KF YouTube channel
- In-app article reader (.article-body CSS), Mark as Read, 🎮 image fallback
- GitHub Gist auto-sync backup (5 sec debounce after any library change)
- Pull-to-refresh on News, error boundary wrapping the app
- Library matching (gold star on headlines mentioning tracked games)
- Filter chips: All / In Library / Nintendo / PlayStation / Reviews / Upcoming / Hardware
- Editorial design: Lora serif + Inter sans, dark glass aesthetic, source-colored badges
- Custom 🎮 PWA icon (180/167/192/512), manifest.json, service worker (no real push yet)

## Still planned (in priority order)
1. **Stats page** — new "Stats" section in Library segmented control. Hero numbers (total played, avg score, total hours from RAWG playtime, Top 50 avg, # Masterpieces), score distribution histogram, taste-profile radar across the 10 rubric categories, by-platform bars, by-year line, top publishers/devs, completion %, score vs release-year scatter. All pure local computation.
2. **"Recommended for you"** — restructure existing Recommended section into two rows: "For you" (RAWG-driven using user's library signal — weight platforms by score sum, publishers/devs by Top 50 presence, Metacritic ≥75) and "Saved for later" (existing manual list). Tap recommended → Save for later / Dismiss.
3. **In-app YouTube/Spotify player** — embedded YouTube IFrame Player in reader sheet with custom controls (play/pause, ±15 sec skip, scrubber). Media Session API for lockscreen handlers (best effort — iOS PWA + iframe is hit-or-miss). Spotify is removed for now; if revived, embed iframe has native iOS lockscreen integration.

## Open questions / known issues
- IGN's games-all feed occasionally lets through entertainment crossover (e.g. Game of Thrones references). NON_GAMING_TITLE_RE in worker.js catches the common ones but isn't exhaustive.
- Worker /article extraction works best on Polygon, IGN, Engadget, Nintendo Life, PlayStation Blog. Sites with unusual layouts may return sparse content — extend the content-pattern list in extractArticleContent() if needed.
- Worker has a `_debug` field in podcast responses. Could strip for production size optimization (~5 min cleanup).
- Vice's gaming feed is essentially defunct since Waypoint shut down; VICE_KEEP URL filter is strict so most Vice items get dropped now. Could remove Vice from RSS_SOURCES entirely.
- Lockscreen Media Session for YouTube iframe on iOS PWA is the biggest unknown for feature #3.

## Worker structure (for fresh-context reference)
- RSS_SOURCES: array with { source, url, dedicated: bool }. Dedicated sources trust the feed; mixed sources require GAMING_SIGNALS_RE match in title/excerpt/URL.
- PODCAST_SOURCES: array with { youtubeHandle, titlePatterns }. Handle resolved once via channel page scrape, channel RSS fetched once per channel (shared between shows on same channel).
- WIKIPEDIA_EVENT_SOURCES: Nintendo_Direct + State_of_Play_(video_program). Parser walks every <tr>, finds future-dated rows, picks soonest.
- Falls back to scanning headlines for "State of Play" / "Nintendo Direct" + parseable date if Wikipedia is stale.
- Edge-cached 30 min for /news, 7 days for /article.

## How to resume
Start a fresh chat with:
> Continue building my video game library app at ~/video-game-library. Read NOTES.md for context, then let's build [Stats page | Recommended for you | YouTube player].
