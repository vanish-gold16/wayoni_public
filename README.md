<p align="center">
  <img src="frontend/public/images/wayoni/wayoni.webp" width="180" alt="Wayoni mascot" />
</p>

<h1 align="center">Wayoni</h1>

<p align="center">
  <b>Find where to go by what you actually like</b><br>
  Pick mountains, medieval streets, or "nowhere touristy" — and see which cities deliver,
  with what it costs to get there.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-21-orange" alt="Java 21" />
  <img src="https://img.shields.io/badge/Spring_Boot-4.0-green" alt="Spring Boot 4" />
  <img src="https://img.shields.io/badge/React-19-blue" alt="React 19" />
  <img src="https://img.shields.io/badge/TypeScript-6-blue" alt="TypeScript" />
  <img src="https://img.shields.io/badge/PostgreSQL-✓-336791" alt="PostgreSQL" />
</p>

---

## The problem

Every flight aggregator answers *"how much to fly to Rome?"* — but goes quiet when the question is *"where should I even go?"*

Picking a destination is the hardest part of planning a trip.
You scroll blogs, watch reels, ask friends, and end up choosing between the same five cities everyone already knows.

## What Wayoni does

Wayoni flips the search: **you describe the trip you want, and the map fills in the rest.**

1. **Pick your interests** — 20+ tags across four groups: Nature & Outdoors, City Life, Eras & Styles, and Vibe.
2. **Every destination is scored** against those interests on a 0–1 scale, aggregated from city → region → country data.
3. **Results are ranked** by a match percentage with a tiered system — places that satisfy all your "strict" interests always rank above partial matches, regardless of score.
4. **Transport is real** — each result card shows what it actually costs to fly or take a bus there.

### Two search modes

| Mode | What it does |
|---|---|
| **Random** | Pick interests, get matched destinations sorted by fit — "show me beach + medieval + not touristy" |
| **Route** | Pick origin → destination, get transport options with prices and dates |

### Interest system

Tags aren't just labels. Each one has a **strictness level** that controls how aggressively it filters:

| Strictness | Behaviour | Example |
|---|---|---|
| **Open** | A wish — lifts places that fit, doesn't punish the rest | Nature, Food, Romantic |
| **Selective** | Places without it fall well down the list | Nightlife, Medieval, Fashion |
| **Strict** | Binary — a place either has it or doesn't | Beach, Volcano, Vikings |

> Tags describe what a **trip** to a place gives you, not what sits inside the city limits.
> Mountains an hour away count — which is why a flat city can turn up under "Mountains."

## Coverage

- **67 cities** across Europe, plus select destinations in the Middle East, Asia, and beyond
- **41 countries** with detailed profiles
- **50+ origin cities** for departure — auto-detected by geolocation
- **26 curated collections** (e.g., "Hidden gems," seasonal picks, interest-based)
- **Bus routes** data for ground transport pricing

## Tech stack

### Backend
- **Java 21** / **Spring Boot 4.0**
- **PostgreSQL** with **Flyway** migrations
- **Spring Security** with JWT (cookie-based, not header) + Google OAuth
- **Spring Cache** (Caffeine) for scoring and search results
- **Seed data** — structured JSON: cities, regions, countries, bus routes, stations
- **Admin panel** — audit log, user management, content stats, daily analytics

### Frontend
- **React 19** with **TypeScript 6**
- **Vite 8** with lazy-loaded routes (admin, auth, account pages load separately)
- **React Router 7** for client-side routing
- **Custom interest picker** with scatter visualization on the hero
- **SEO pipeline** — build-time generation of `robots.txt`, `sitemap.xml`, prerendered HTML, OG tags, Schema.org markup, and `llms.txt` for AI crawlers
- **Share cards** generated client-side with `html-to-image`

### Infrastructure
- **Nginx** reverse proxy with API passthrough
- **Let's Encrypt** TLS
- **Contabo** VPS hosting
- **Puppeteer** prerendering for SEO-critical pages

## Architecture

```
┌──────────────────────────────────────────────────┐
│                     Nginx                        │
│         TLS · gzip · static assets               │
│    ┌────────────┐        ┌────────────────┐      │
│    │  /assets/* │        │    /api/*       │      │
│    │  (1y cache)│        │  → :8080        │      │
│    └────────────┘        └────────────────┘      │
└──────────────────────────────────────────────────┘
         │                         │
         ▼                         ▼
┌─────────────────┐     ┌──────────────────────┐
│   React SPA     │     │   Spring Boot API    │
│   Vite build    │     │                      │
│                 │     │  • Search / scoring   │
│  • Interest     │     │  • Auth (JWT+Google)  │
│    picker       │     │  • User accounts      │
│  • Result cards │     │  • Saved trips        │
│  • Place pages  │     │  • Booking clicks     │
│  • Country      │     │  • Admin dashboard    │
│    profiles     │     │  • Flyway migrations  │
│  • Collections  │     │                      │
│  • SEO layer    │     └──────────┬───────────┘
└─────────────────┘                │
                                   ▼
                          ┌────────────────┐
                          │   PostgreSQL   │
                          │   + seed data  │
                          └────────────────┘
```

## Data sources

Wayoni integrates with multiple travel data APIs for real pricing:

| Source | Purpose |
|---|---|
| **Travelpayouts / Aviasales** | Cached flight prices, "cheapest from city X" |
| **OpenStreetMap + GTFS** | Map data and public transit schedules |

## Project status

🚧 **In active development** — this is a real product being built and shipped, not a demo.

The source code is in a private repository. This public repo serves as documentation and project overview.

## License

All rights reserved. This repository contains project documentation only.
Source code is proprietary and not available for redistribution.

---

<p align="center">
  <img src="frontend/public/images/wayoni/wayoni_explorer.webp" width="120" alt="Wayoni explorer" />
  <br>
  <sub>Built with ☕ and mass transit data</sub>
</p>
