---
title: The end of WordPress. For my blog, anyway.
date: 2026-10-02
description: "arnfinn.cloud is now a static Eleventy site. AI made the migration practical, Markdown replaced the CMS, and my hosting bill dropped from 69 to 39 kr a month."
tags:
  - eleventy
  - static-sites
  - wordpress
  - ai
draft: false
# Remove draft and permalink when ready to publish.
---

The end of PHP and WordPress is here.

For my blog, anyway.

[arnfinn.cloud](https://arnfinn.cloud) is live on a new setup: Markdown files, Eleventy, and static HTML and CSS hosted at Domeneshop. No PHP application serving the blog. No production database. No WordPress administration panel.

The web feels simple again.

That is not a prediction that WordPress will disappear. It is a question about what a personal technical blog actually needs. Mine needs to publish text, images, and code examples. I no longer think that requires a database-backed application running on the server.

## Why I left

Security concerns, hosting costs, and maintenance were the main reasons.

A WordPress installation means keeping the application, themes, and plugins updated, and paying attention to the hosting environment underneath them. PHP is not inherently insecure, and neither is every WordPress site. But each additional component brings something else to maintain.

For a blog I want to spend my time writing, not administering.

The cost difference is concrete. I stayed with Domeneshop and downgraded my hosting plan from **69 kr to 39 kr per month**. That is 30 kr less each month, or 360 kr a year: roughly a 43% reduction in the hosting bill.

That does not mean databases universally cost 30 kr a month. It means my blog no longer needs the hosting package I previously used.

The saving is welcome. The smaller operational footprint matters more.

## Static sites are not new. The way I got here is.

Developers have been publishing static blogs for years. Markdown and static-site generators are not a breakthrough.

What changed for me was AI making the migration practical.

AI helped build the replacement site and migration setup: the platform that turns the posts into a working static blog. Domeneshop hosts the resulting files; AI is not a hosting service.

The result is not a new runtime dependency on an AI model. Readers do not need an AI service to respond before they can open a post. Once built and uploaded, the site is just files.

AI also gives me another option for authoring. I can write a Markdown post myself, or start with rough notes and ask AI to produce a draft. Either way, I review the content and take responsibility for what I publish.

That distinction matters for technical writing. A fluent explanation is not necessarily a correct one.

## What actually runs

The workflow is deliberately ordinary:

**Markdown posts → Eleventy build → static files → upload to Domeneshop.**

Each post lives in the repository as a Markdown file with a small front-matter block:

```markdown
---
title: A note worth keeping
date: 2026-10-02
description: A short explanation of what this post covers.
tags:
  - web
---

Write the post here.
```

I save it under `src/posts/<slug>.md`, preview it locally with `npm run serve`, and run `npm run build` to generate the site into `_site/`. Then I upload those generated files over SSH using SCP or rsync.

There is still a build toolchain to maintain. Eleventy and its Node.js dependencies do not magically stop needing updates. The difference is that this tooling builds the site rather than running the blog for every visitor.

The site keeps the useful basics: existing WordPress post URLs, self-hosted images, syntax-highlighted code, an RSS/Atom feed, a sitemap, and social-preview metadata.

Simple does not have to mean stripped of everything useful.

## What I gave up

There is no browser-based WordPress editor now. For someone who wants to log into a dashboard and publish without touching files, this would be a real drawback. For me, files are a good fit.

Comments are another deliberate omission. They can create useful conversations, but moderation is work and discussions can turn toxic. I would rather discuss a post on LinkedIn than operate a comment system on the blog.

WordPress plugins are no longer an install button away. Custom features are possible, including with AI assistance, but they still need engineering and maintenance. A feature requiring server-side state would also change the simplicity argument.

Scheduled publishing is useful, and this setup does not currently provide it. Build-and-deploy automation could handle that later. I am not treating an idea for a future feature as something already delivered.

## A smaller solution to the actual problem

WordPress still makes sense for many sites: editorial teams, richer publishing workflows, and projects that need its ecosystem.

My personal blog does not need all of that.

It needs a reliable way to turn writing into pages people can read. Markdown, Eleventy, and ordinary static hosting do that. AI helped make the move achievable without becoming part of the serving infrastructure.

The new [arnfinn.cloud](https://arnfinn.cloud) is live. For this blog, PHP and the database are gone, the bill is smaller, and there is less to look after.

Does your personal blog need a CMS, or just a way to publish?
