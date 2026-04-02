# Your Colorful Path — Podcast Toolkit

A GitHub Pages site with tools for publishing, formatting, and sharing episodes of the **Your Colorful Path** podcast by Charu Boeckel.

Live at: `https://willolovesyou.github.io/charu/`

## Pages

- **index.html** — Libsyn & YouTube Show Notes Formatter (default/home page)
- **bookmarklet.html** — Blog Post Bookmarklet (Libsyn → Shopify)
- **guest-email.html** — Guest Email Generator
- **caption-generator.html** — Instagram Caption Generator (calls Anthropic API with user's key)

## Nav order (consistent across all pages)

1. Libsyn & YouTube Show Notes Formatter
2. Blog Post Bookmarklet
3. Guest Email Generator
4. Caption Generator

## Key conventions

- All pages share a consistent nav bar, header style (`.header h1` at `1.5rem`), and footer
- Color palette: gold `#d49633`, dark `#2d2a26`, cream `#faf8f5`
- Email copy uses `<br><br>` for paragraph spacing (not `<p>` tags) for Flodesk compatibility
- Rich text copy uses `ClipboardItem` with `text/html` blob
- Favicon is an SVG retro microphone (`favicon.svg`)
- Footer format: "Created by Willo Sana for Charu" / "Divine Amuleto • Your Colorful Path Podcast" / "© 2026 Willo&Co LLC • EvolveYourGenius.com"

## Guest Email Generator — data sources & architecture

The guest email page fetches from **4 live feeds** on page load, plus uses fallback lookup tables where live data isn't available.

### Live feeds (auto-discover new episodes)

| Source | URL | What it provides | CORS |
|--------|-----|------------------|------|
| **Libsyn RSS** | `rss.libsyn.com/shows/521038/destinations/4475083.xml` | Episode list, titles, descriptions, GUIDs (primary source) | ✅ |
| **iTunes API** | `itunes.apple.com/lookup?id=1790327657` | Apple Podcasts episode URLs | ✅ |
| **YouTube RSS** | `youtube.com/feeds/videos.xml?channel_id=UCzFbwY7hFNFIgTNpdUhFpgQ` | YouTube video IDs (15 most recent only) | ✅ |
| **Shopify Atom** | `divineamuleto.com/blogs/your-colorful-path-podcast.atom` | Blog post handles/URLs | ✅ |

### Fallback lookup tables (in guest-email.html)

| Table | Why it's needed |
|-------|-----------------|
| `YOUTUBE_FALLBACK` | YouTube RSS only returns 15 most recent uploads. This catches older episodes (#1–#8) and ones without episode numbers in the title (#26). Live feed always takes priority. |
| `SPOTIFY_EPISODES` | Spotify has no public feed and blocks CORS. Must be updated manually. Hint on page links to Spotify show page for easy copy-paste. |

### Resolution priority for each field

- **Episode list & titles** → Libsyn RSS (always live)
- **Apple Podcasts** → iTunes API (always live)
- **YouTube** → YouTube RSS feed → `YOUTUBE_FALLBACK` table → extract from Libsyn description
- **Blog Post URL** → Shopify Atom feed → `buildHandle()` auto-generated slug
- **Spotify** → `SPOTIFY_EPISODES` table → manual paste
- **pod.link** → auto-generated from episode GUID
- **Guest name** → parsed from episode title (after "with")

### YouTube coverage

As of March 2026: 13 of 28 episodes are on YouTube. Episodes #4, #6, #7, #9–20 are not on YouTube. The `YOUTUBE_FALLBACK` table contains all 13 known video IDs.

### What Charu needs to do for a new episode

1. **Nothing** for most fields — they auto-populate from live feeds
2. **Spotify** — if not in lookup table, click the "Open Spotify show page" link on the page, find the episode, Share → Copy Link, paste into the field
3. **YouTube fallback** — only needs updating if the video falls outside the 15 most recent uploads (unlikely for new episodes)

## Show Notes Formatter (index.html)

Paste draft show notes, get consistently formatted output for both Libsyn and YouTube. Handles social media links, timestamps, guest info, and episode structure.

## Blog Post Bookmarklet (bookmarklet.html)

A drag-to-bookmarks-bar tool that runs on a Libsyn episode page and extracts content for pasting into Shopify. Includes a "Batch Mode" collapsible section for processing all episodes at once.

## Workflow

- User preference: always push after changes ("ALWAYS PUSH")
- Repo: `https://github.com/WilloLovesYou/charu.git`
- Branch: `main`
- Deployed via GitHub Pages
