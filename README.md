# Video Game Library

A personal video game journal — track upcoming releases, rate finished games on a 10-category rubric (out of 100), and curate a Top 50.

Built as a single-file PWA. Saves data locally in IndexedDB / localStorage. No backend.

## Run locally

Open `index.html` directly in any modern browser, or serve it:

```
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Deploy

This is a static site. Push to a public GitHub repo, enable GitHub Pages on the `main` branch, and the live URL becomes `https://<username>.github.io/video-game-library/`. Add to iPhone home screen for an app-like experience.

## Status

**Phase 1 (in progress):**
- [x] Library with state filters (Top 50, Playing, Tracking, Backlog, Rumored)
- [x] Game Detail with 10-category spider chart + breakdown
- [x] Upcoming release calendar
- [x] Editorial dark mode design
- [x] Seed data (50 ranked games, 18 tracking, 27 rumored, 10 backlog, 1 playing)
- [ ] Add / Edit game flows
- [ ] RAWG metadata + cover art integration
- [ ] Export to JSON (backup)
- [ ] Push notifications for releases

**Phase 2:**
- [ ] News feed (Nintendo, PlayStation, Polygon, Kinda Funny)
- [ ] In-app YouTube embeds + podcast playback
