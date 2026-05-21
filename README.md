# sujwalboppana.github.io

Personal portfolio of **Sujwal Boppana** — Java backend engineer, Hyderabad.

Single-file HTML/CSS/JS. No build step. Hosted on GitHub Pages.

> Live: **https://sujwalboppana.github.io/**

---

## Stack

- **HTML/CSS/JS** — one file, inline styles + scripts
- **Google Fonts** — Inter + JetBrains Mono (only external runtime dependency)
- **GitHub Pages** — static hosting from the `main` branch root

No bundler. No package.json. No node_modules. The site you edit is the site you ship.

---

## Project structure

```
.
├── index.html          # the entire site (HTML + CSS + JS, single file)
├── og-image.png        # 1200×630 social share card (Open Graph + Twitter)
├── robots.txt          # crawler directives
├── sitemap.xml         # search engine sitemap (with image sitemap)
├── .gitignore          # OS, editor, Claude Code locals
├── README.md           # this file
└── design/             # design source bundle (not deployed-critical)
    ├── README.md       #   notes from the design tool
    ├── chats/          #   transcripts of the iteration with the design assistant
    └── project/        #   the HTML prototypes (v1 + v2) the live site was forked from
```

The `design/` folder is reference material. GitHub Pages serves it but nothing links to it. If you ever want to drop it from the deploy, add `design/` to `.gitignore` and remove the existing commit.

---

## Deploy to GitHub Pages

### One-time setup

1. Create a new GitHub repo named **`SujwalBoppana.github.io`** (the repo name must match your username exactly — this makes it a user site rather than a project site, so it serves at the root path).
2. Push this directory:

   ```sh
   git init
   git add .
   git commit -m "Initial portfolio"
   git branch -M main
   git remote add origin git@github.com:SujwalBoppana/SujwalBoppana.github.io.git
   git push -u origin main
   ```

3. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `/ (root)` → Save**.
4. Wait ~1 minute. The site is live at **https://sujwalboppana.github.io/**.

### Updating

```sh
# edit index.html
git add index.html
git commit -m "tweak copy"
git push
```

Pages rebuilds in ~30s on each push to `main`.

### Custom domain (optional)

If you buy a domain later (e.g. `sujwal.dev`):

1. Add a `CNAME` file at repo root containing just the apex/subdomain:
   ```
   sujwal.dev
   ```
2. At your DNS provider, point `A` records to GitHub Pages IPs (`185.199.108.153`, `.109.153`, `.110.153`, `.111.153`) and a `CNAME` for `www` → `sujwalboppana.github.io`.
3. **Settings → Pages → Custom domain → enter `sujwal.dev` → Enforce HTTPS**.
4. **Find-replace `https://sujwalboppana.github.io/` → `https://sujwal.dev/`** across `index.html`, `robots.txt`, and `sitemap.xml`. Commit.

---

## Local preview

Any static server works. The simplest:

```sh
# Python 3 (built-in on macOS / most Linux)
python3 -m http.server 8000

# then open http://localhost:8000/
```

Alternatives:

```sh
npx serve .           # Node, if you have it
php -S localhost:8000 # PHP, also fine
```

Pure `file://` opening of `index.html` mostly works too, but `localhost` is closer to how Pages will serve it (correct MIME types, no CORS oddities).

---

## Updating content

All content lives directly in [index.html](index.html). No CMS, no JSON file — search for the section you want.

| Want to change | Where |
| --- | --- |
| Name / role / location | `<header class="hero">` (around line 374) |
| About copy | `<section id="about">` |
| Principles (the `// …` lines) | `<section id="principles">` |
| Metrics & sparkline values | `<section id="metrics">` (`data-target` attrs) |
| Experience commits | `<section id="experience">` — each role is an `<article class="commit">` |
| Skills + proficiency dots | `<section id="skills">` — `data-cat`, `data-level` on each chip |
| Education line | `<section id="education">` |
| Case studies | `<section id="case-studies">` |
| Contact rows | `<section id="contact">` |
| Terminal commands | the `commands` object in the inline `<script>` near the bottom |

After updating, also bump:

- `<meta property="og:description">` if you change the hero copy
- The JSON-LD `Person` block in `<head>` if you change job title / skills / employers
- `sitemap.xml` `<lastmod>` to today's date

---

## SEO checklist (post-deploy)

This site is already wired with: title + description, Open Graph, Twitter Cards, JSON-LD (`Person` + `WebSite` + `ProfilePage`), canonical URL, favicons, theme-color, robots/sitemap, skip-link, semantic landmarks. To activate it:

1. **[Google Search Console](https://search.google.com/search-console)** — add `https://sujwalboppana.github.io/` as a property, verify (DNS or HTML meta tag), submit `sitemap.xml`.
2. **[Bing Webmaster Tools](https://www.bing.com/webmasters)** — same flow. Picks up Yahoo, DuckDuckGo, Ecosia by proxy.
3. **[Google Rich Results Test](https://search.google.com/test/rich-results)** — paste the live URL, confirm the `Person` schema parses without warnings.
4. **[PageSpeed Insights](https://pagespeed.web.dev/)** — should score 95+ on both mobile and desktop. If LCP is slow, self-host the fonts (see below).
5. **[Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)** & **[LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)** — force a re-scrape of `og-image.png` so the social preview caches correctly.
6. Add the canonical URL to your LinkedIn profile, GitHub bio, dev.to/Hashnode profile, Twitter bio. Backlinks from authoritative profiles are the single biggest SEO lever for a personal site.

### Optional: self-host fonts for best Core Web Vitals

Google Fonts costs ~150–250ms LCP. To eliminate:

1. Download Inter (400, 500, 600, 700) and JetBrains Mono (400, 500, 600) `.woff2` files into `fonts/`.
2. Replace the `<link href="https://fonts.googleapis.com/…">` with inline `@font-face` rules pointing at the local files.
3. Add `<link rel="preload" as="font" type="font/woff2" crossorigin>` for the two most-used weights.

Skip unless you actually need the points. The current setup is good enough for personal-site SEO.

---

## Regenerating the OG image

If you change the hero copy and want the social card to match:

```sh
# install Pillow once
python3 -m pip install Pillow
```

Then edit and run the generator script (see git history for `_make_og.py`), or open `og-image.png` in any image editor and tweak directly. Keep it 1200×630 PNG.

---

## Accessibility

- `prefers-reduced-motion` respected (kills terminal typing animation, counter animations, pulse halo)
- Visible focus states on all interactive elements
- Semantic HTML5 (`<main>`, `<section>`, `<article>`, `<nav>`, `<header>`, `<footer>`)
- Skip-to-content link for keyboard users
- ARIA labels on the terminal, mode switcher, shortcut modal
- Keyboard shortcuts: `g` then `h/a/r/m/e/s/d/p/c` to jump sections; `/` to focus terminal; `?` for the shortcut sheet

---

## License

No license. © Sujwal Boppana. All rights reserved.

The code is public on GitHub for transparency and inspectability, but is not open-source — please don't copy the design wholesale. Reach out if you want to fork or learn from a piece of it.
