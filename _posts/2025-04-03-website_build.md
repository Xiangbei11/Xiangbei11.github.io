---
layout: post
title: What I Talk About When I Talk About Building Personal Website
date: 2025-04-03 
description: A practical guide and reflection on building a personal academic website using al-folio
#tags: website jekyll al-folio github
#categories: notes
#pinned: false
#related_posts: false
#toc: 
#  beginning: true

---

This post serves as a **summary** and **guide** based on my experience building a personal website using the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme, hosted on GitHub Pages. It includes key setup steps, useful tips, and things I learned during the process.

---

## Repository Structure: **Main** vs **gh-pages**

Your GitHub repository will typically have two branches:

1. **main**: This is where you develop and manage your website’s source code (content, layouts, configurations, etc.).
2. **gh-pages**: This is where the **static** version of your site gets deployed after building it with Jekyll. GitHub serves this branch as the live site.

### How It Works:
If you’re using **GitHub Actions** or any build script, the process is as follows:
- **Main**: You work on the site by editing markdown files, configurations, and layouts.
- **gh-pages**: Once you push changes to `main`, GitHub will rebuild the site and serve the static files in the `gh-pages` branch.

**In short**:
- `main`: Where you edit and develop.
- `gh-pages`: What the world sees when visiting your site.

### To Confirm Automatic Deployment:
1. Check if you have GitHub Actions set up: Look for `.github/workflows` folder in your repo.
2. If you see files like `deploy.yml` or `pages.yml`, the deployment is set up to automatically build and deploy to `gh-pages` when changes are pushed to `main`.

---

## Reducing Menu Options

If you want to hide certain pages from the navigation bar, you can use the `nav: false` flag in the **front matter** of each page file (e.g., `cv.md`, `projects.md`).

Example:

```yaml
nav: false
```

This will **exclude the page from the navbar**, even if it exists in the `_pages/` directory.

You can also **adjust the order** of pages by using `nav_order`:

```yaml
nav_order: 1.5  # This will place the page between 1 and 2 in the navigation menu
```

**Note**: If you set `nav: false`, the `nav_order` value will be ignored since the page won't be in the navigation menu.

---

## Highlighting Your Name in Publications

To bold your name in publications (for example, in **BibTeX**), you can use the `highlight_author` field in `_config.yml`.

Steps:
1. Open `_config.yml`.
2. Add your name like this:

```yaml
scholar:
  last_name: [Liu]
  first_name: [Xiangbei, X.]
```

This will automatically **bold your name** in the publication section whenever it matches your name in the BibTeX `author` field.

---

## Adding Social Media Links

To display social media links (e.g., LinkedIn, GitHub) in your website:

1. Edit `_data/socials.yml`.
2. Use the **social network name** and **URL**:

```yaml
linkedin:
  url: https://www.linkedin.com/in/xiangbei-liu-208a70207
github:
  url: https://github.com/xiangbei11
```

You can use the **username** directly instead of the full URL in most cases.

---
## SSH key

### Setting Up SSH for GitHub

To securely push code to GitHub using SSH, follow these steps:

1. **Generate an SSH key**:

```bash
ssh-keygen -t ed25519 -C "xiangbei.liu.th@dartmouth.edu"
```

2. **Copy your public key**:

```bash
cat ~/.ssh/id_ed25519.pub
```

3. **Add the key to GitHub**:
   - Go to **GitHub → Settings → SSH and GPG Keys**.
   - Click **New SSH Key**, paste your public key, and save.

4. **Update your Git remote to use SSH**:

```bash
git remote set-url origin git@github.com:xiangbei11/xiangbei11.github.io.git
```

5. **Test the connection**:

```bash
ssh -T git@github.com
```

If everything is set up, you should see:

```
Hi xiangbei11! You've successfully authenticated...
```

Now, you're ready to **push changes**:

```bash
git push origin main
```

### Fixing "Host Key" Warnings for SSH

If you see a warning about "Host key verification failed" when connecting to GitHub via SSH, it's most likely due to a change in the SSH key. To fix it:

1. Remove the old key:

```bash
ssh-keygen -R github.com
```

2. Reconnect to GitHub:

```bash
ssh -T git@github.com
```

Type `yes` when prompted, and the connection will be reset.

---

## Working with Font Sizes and New Lines in `_config.yml`

- **Multiline strings in YAML**: The `>` symbol allows you to format multiline strings, like the `blog_description`.

```yaml
blog_description: >
  <em>“When you come out of the storm, you won't be the same person...”</em> — Haruki Murakami
  This blog is my place to collect and make sense of what I’m learning...
```

- **New Paragraphs**: Use `<p>` tags for new paragraphs. To force a line break within the same paragraph, use `<br>`.

---

## Understanding `rem` in CSS

- `rem` stands for **root em**. It’s a relative unit based on the **root** font size (usually 16px by default).
  
Example:
- `1rem = 16px`
- `0.9rem = 14.4px`

`rem` makes it easier to scale font sizes across your site.

---

## Changing the Default Font Size of the Blog Page

To modify the default font size of the blog page, you'll need to:

1. Locate the theme's CSS file (typically `assets/css/main.scss`).
2. Override the font size for the blog page:

```scss
.post-content {
  font-size: 0.9rem;  /* Or any size you prefer */
}
```

This will reduce the font size for the content of the blog.

---

## Handling External Blog Posts

External blog posts are often included in your `_config.yml` under `external_sources`. To remove them:

1. Open `_config.yml`.
2. Comment out or delete the following block:

```yaml
external_sources:
  - name: medium.com
    rss_url: https://medium.com/@al-folio/feed
  - name: Google Blog
    posts:
      - url: https://blog.google/technology/ai/google-gemini-update-flash-ai-assistant-io-2024/
        published_date: 2024-05-14
```

This will stop these external posts from showing up on your blog.

---

## Table of Contents (TOC) Configuration

The `toc:` option in the front matter is used to generate a Table of Contents for the post:

- **`beginning: true`**: Places the TOC at the start of the post.
- **`end: true`**: Places the TOC at the end of the post.
- **`sidebar: true`**: Moves the TOC to the sidebar.

Example:

```yaml
toc:
  beginning: true
  max_level: 2
```

This will place the TOC at the beginning and include only the first two levels of headings.

---

## TODO
* Submit the site to Google Search Console
* Change # of more authors to "et al."
* Change the theme color

---

---

This guide captures the how-to steps that helped me set up and refine my site. If you're building yours with al-folio, I hope this saves you time and effort!



