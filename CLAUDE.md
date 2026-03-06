# Your Colorful Path — Podcast Toolkit

A GitHub Pages site with tools for publishing, formatting, and sharing episodes of the **Your Colorful Path** podcast by Charu Boeckel.

Live at: `https://willolovesyou.github.io/charu/`

## Pages

- **index.html** — Libsyn & YouTube Show Notes Formatter (default/home page)
- **bookmarklet.html** — Blog Post Bookmarklet (Libsyn → Shopify)
- **guest-email.html** — Guest Email Generator

## Key conventions

- All pages share a consistent nav bar, header style, and footer
- Color palette: gold `#d49633`, dark `#2d2a26`, cream `#faf8f5`
- Static lookup tables (`BLOG_HANDLES`, `YOUTUBE_EPISODES`, `SPOTIFY_EPISODES`) are used in guest-email.html because external APIs lack CORS — update these manually when new episodes drop
- Email copy uses `<br><br>` for paragraph spacing (not `<p>` tags) for Flodesk compatibility
- Rich text copy uses `ClipboardItem` with `text/html` blob
- Favicon is an SVG retro microphone (`favicon.svg`)

## External data sources

- **iTunes API** — episode lookup (has CORS)
- **Libsyn RSS** — episode titles, descriptions, GUIDs (has CORS)
- **Shopify blog** — no CORS, uses `BLOG_HANDLES` lookup table
- **YouTube channel** — no CORS, uses `YOUTUBE_EPISODES` lookup table
- **Spotify** — no CORS, uses `SPOTIFY_EPISODES` lookup table

## Workflow

- User preference: always push after changes ("ALWAYS PUSH")
- Repo: `https://github.com/WilloLovesYou/charu.git`
- Branch: `main`
