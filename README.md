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

## The API

Apple's free, no-key, CORS-enabled RSS generator:

```
https://rss.applemarketingtools.com/api/v2/{country}/apps/{feed}/{limit}/apps.json
```

- `{country}` — ISO storefront code, e.g. `us`, `gb`, `jp`, `cn`
- `{feed}` — `top-free` or `top-paid`
- `{limit}` — number of results

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
