---
layout: post
title: What I Talk About When I Talk About Building Personal Website
date: 2025-04-03 9:00:00-0400
description: A practical guide and reflection on building a personal academic website using al-folio
tags: website jekyll al-folio github
categories: notes
giscus_comments: true
related_posts: false
toc:
  beginning: true
---

This post is a hands-on summary of how I built and customized my academic website using the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme, hosted via GitHub Pages. It's a mix of key setup steps, configuration tips, and things I learned by doing.

## Using GitHub and Git

Basic Git workflow:

```bash
git clone <repo-url>
git checkout -b <new-branch>  # optional
# Make changes
git add .
git commit -m "Your message"
git push origin main
```

To avoid authentication issues, use **SSH** instead of HTTPS. You can generate a key using:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Then add it to GitHub under **Settings → SSH and GPG Keys**.

## Editing `_config.yml`

The `_config.yml` file controls global site behavior. For example:

```yaml
title: Xiangbei Liu
subtitle: <a href="https://engineering.dartmouth.edu/">Thayer School of Engineering, Dartmouth</a>. Hanover, NH.
```

### Multi-line text

Use the `>` symbol to create multi-line strings, such as a custom blog description:

```yaml
blog_description: >
  *“When you come out of the storm...”* — Haruki Murakami

  This blog is a record of my learning journey in machine learning...
```

## Navigation Menu

Navbar items are auto-generated from page front matter. To hide a page:

```yaml
nav: false
```

Include this in the YAML front matter of any page, like `_pages/blog.md`.

To reorder pages, use:

```yaml
nav_order: 2
```

## Social Links

Add links in `_data/socials.yml`. Example:

```yaml
linkedin:
  url: https://www.linkedin.com/in/your-profile
```

You don’t need `linkedin_username` unless your theme explicitly references it.

## Disabling Blog Tags & Categories

In `_config.yml`, disable tag/category display like this:

```yaml
display_tags: []
display_categories: []
```

## Pinned Posts

To avoid posts being pinned to the top, remove or set:

```yaml
pinned: false
```

in the front matter of each post. You can also disable the logic in the layout file if needed.

## Making Your Site Discoverable

To show up in Google search results:

- Submit your site to [Google Search Console](https://search.google.com/search-console)
- Add SEO fields to `_config.yml`:

```yaml
description: Machine learning researcher at Dartmouth
keywords: machine learning, generative design, Dartmouth, PhD
serve_og_meta: true
serve_schema_org: true
```

- Add backlinks to your site from LinkedIn, GitHub, etc.

---

This guide captures the how-to steps that helped me set up and refine my site. If you're building yours with al-folio, I hope this saves you time and effort!



