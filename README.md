# App Store Charts Tracker 🕹️

Day 96 of my 100 Days of Code. A single-page web app that pulls **live App Store top charts** from Apple's official RSS feed and lets you flip between storefronts (countries), Top Free / Top Paid, and a games-only lens.

**Live demo:** https://voyageplanetgames.github.io/App-Store-Charts-Tracker/

![Built by Chen](assets/chen.jpeg)

## What it does

- Fetches the top apps for any of 12 storefronts straight from the browser (no backend).
- Toggles between **Top Free** and **Top Paid** charts, 25 / 50 / 100 deep.
- **🎮 Games only** filter — client-side filtering on Apple's genre data (genreId `6014`).
- Ranked leaderboard with artwork, developer, genre chips, and a deep-link to each App Store listing.
- Loading skeletons, empty-state and error handling.

## The API — two Apple feeds

Apple exposes two free, no-key RSS feeds, and this app uses both because they have different strengths:

**Modern (v2)** — used for plain Top Free / Top Paid:
```
https://rss.applemarketingtools.com/api/v2/{country}/apps/{feed}/{limit}/apps.json
```
Clean JSON, but **only** serves overall free/paid charts and its genre data is too thin to reliably isolate games.

**Legacy** — used for Games-only and Top Grossing:
```
https://itunes.apple.com/{country}/rss/{slug}/limit={n}[/genre=6014]/json
```
- `{slug}` — `topfreeapplications`, `toppaidapplications`, or `topgrossingapplications`
- `genre=6014` — native filter that returns **top games directly** (no guesswork)
- adds **Top Grossing**, which v2 doesn't offer

`{country}` is an ISO storefront code (`us`, `gb`, `jp`, `cn`, …). The two feeds use slightly different JSON shapes, so the app normalizes both into one app object before rendering.

Each result includes `name`, `artistName`, `artworkUrl100`, `genres[]`, `releaseDate`, and `url`.

**CORS note:** Apple's RSS endpoint does **not** send `Access-Control-Allow-Origin` headers, so a static page (like GitHub Pages) can't call it directly — the browser blocks it with a "Load failed" error. The app tries the direct call first, then falls back through public CORS proxies (`allorigins`, `corsproxy.io`, `thingproxy`) until one succeeds. For a production tool you'd run your own tiny proxy or cache the feed server-side.

## Run locally

It's a static site — just open `index.html`, or serve it:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy (GitHub Pages)

```bash
git add .
git commit -m "Day 96: App Store Charts Tracker"
git push origin main
```

Then in the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `(root)`**. The site goes live at `https://voyageplanetgames.github.io/App-Store-Charts-Tracker/`.

## Tech

Vanilla HTML/CSS/JS, no dependencies. Fonts: Syne (display), Hanken Grotesk (body), Space Mono (data). Arcade-neon dark theme.

---

☕ If this is useful: https://buymeacoffee.com/chenbuilds
