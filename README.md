# arnfinn.cloud

The static site for [arnfinn.cloud](https://arnfinn.cloud) — "Notes to myself".

Migrated from WordPress to a plain static site built with
[Eleventy](https://www.11ty.dev/). No PHP, no database — just HTML and CSS,
hosted on domene.shop and uploaded over SCP.

## Quick start

```bash
npm install      # first time only
npm run serve    # local preview at http://localhost:8080
npm run build    # build the static site into _site/
```

## Writing a post

Add a Markdown file to `src/posts/<slug>.md`. The file name becomes the URL.
See **[AUTHORING.md](AUTHORING.md)** for the front-matter fields, conventions,
and the AI-assisted authoring workflow, and `src/posts/_TEMPLATE.md` for a
starting point.

## Deploying

Build, then upload `_site/` to domene.shop. See **[DEPLOY.md](DEPLOY.md)** for
copy-paste SCP/rsync commands (SSH key or password).

## Project layout

```
eleventy.config.js          # Eleventy configuration
src/
  _data/site.json           # site title, URL, author, defaults
  _includes/                # base + post layouts
  assets/css/style.css      # all styling
  assets/images/<slug>/     # per-post images
  index.njk                 # home page (post list)
  about-me.md               # About page
  posts/                    # blog posts (Markdown)
    _TEMPLATE.md            # copy this for new posts
  feed.njk / sitemap.njk / robots.njk / 404.njk
```

## Features

- Preserved WordPress URLs (`/<slug>/`)
- Self-hosted images, syntax-highlighted code blocks
- RSS/Atom feed, sitemap, robots.txt
- Open Graph / Twitter Card metadata, favicon
- Light/dark theme via `prefers-color-scheme`
