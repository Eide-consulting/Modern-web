# Authoring posts

This site is built with [Eleventy](https://www.11ty.dev/). Posts are plain
Markdown files — write in a text editor, preview locally, then deploy.

## The AI-assisted workflow

1. Dump your rough notes into a text file (bullet points are fine).
2. Ask your AI assistant: *"Turn this into a blog post for my Eleventy site,
   following `src/posts/_TEMPLATE.md`."*
3. Save the result as `src/posts/<slug>.md`. The **file name becomes the URL**
   (`https://arnfinn.cloud/<slug>/`), so choose the slug carefully — it should
   be lowercase words separated by hyphens.
4. Preview with `npm run serve`, then publish per `DEPLOY.md`.

## Front matter

Every post starts with a YAML block (see `src/posts/_TEMPLATE.md`):

| Field         | Required | Notes                                                        |
| ------------- | -------- | ------------------------------------------------------------ |
| `title`       | yes      | Shown as the page `<h1>` and in the browser tab.             |
| `date`        | yes      | `YYYY-MM-DD`. Controls ordering (newest first).              |
| `description` | yes      | 1–2 sentences. Used on the home list, RSS, and OG previews.  |
| `tags`        | no       | A list, e.g. `azure`, `powershell`. No categories.           |
| `image`       | no       | Featured image path, used as the social/OG preview image.    |
| `draft`       | no       | `true` excludes the post from the home list, feed, and sitemap; it does not prevent page generation. |
| `permalink`   | no       | Set to `false` to prevent page generation while drafting. Remove the line to use the automatic URL; do not set it to `true`. |

The `layout` and `permalink` are set automatically for everything in
`src/posts/` (see `src/posts/posts.json`) — you don't repeat them per post.

## Conventions

- **Slugs / URLs:** the file name is the URL. Don't change an existing slug
  without a redirect — it breaks links and search rankings.
- **Images:** put them in `src/assets/images/<slug>/` and reference with an
  absolute path, e.g. `![alt](/assets/images/<slug>/screenshot.png)`.
- **Code blocks:** use fenced blocks with a language for highlighting, e.g.
  ` ```powershell `, ` ```kql `, ` ```yaml `, ` ```bicep `.
- **Drafts:** set both `draft: true` and `permalink: false` to keep a post
  unpublished. This prevents page generation in both `npm run build` and
  `npm run serve`, and excludes the post from the home list, feed, and sitemap.
- **Publishing:** remove `permalink: false` and remove `draft: true` (or set
  `draft: false`). The page URL is then assigned automatically from the filename.
- **Using the template:** when copying `src/posts/_TEMPLATE.md` for a real post,
  remove `eleventyExcludeFromCollections: true`; it is only there to exclude
  the template itself from collections.

## Commands

```bash
npm install      # first time only
npm run serve    # local preview at http://localhost:8080
npm run build    # generate the static site into _site/
```
