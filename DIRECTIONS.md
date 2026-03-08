# PowerfulThirst.com - Setup & Usage Directions

## Overview

PowerfulThirst.com is a Hugo static site with a built-in CMS admin panel for creating and editing articles through your browser. The code lives on GitHub and can auto-deploy when you push changes.

**GitHub Repo:** https://github.com/ThirstMetrics/powerfulthirst

---

## Site Structure

The site has all original content restored from the Wayback Machine archive:

| Section | URL | Content |
|---------|-----|---------|
| Features (Blog) | /blog/ | 12 industry articles |
| Drink Recipes | /recipes/ | 10 cocktail recipes |
| Spirits | /spirits/ | Bourbon, Whiskey, reviews |
| Podcasts & Videos | /media/ | Podcast + video interviews |
| TiWin Data | /tiwin-data/ | Business tools & check-ups |
| About | /about/ | About John Paddon |

---

## HOSTING: Two Options

### Option 1: Cloudflare Pages (Free - Recommended)

Cloudflare Pages gives you free hosting with automatic deploys from GitHub. No server to manage.

1. Create a free account at https://dash.cloudflare.com/
2. In the left sidebar, click **Workers & Pages**
3. Click **Create** > **Pages** tab > **Connect to Git**
4. Authorize Cloudflare to access your GitHub, select the `powerfulthirst` repo
5. Configure the build:
   - **Build command:** `hugo --gc --minify`
   - **Build output directory:** `public`
   - **Environment variable:** Add `HUGO_VERSION` = `0.142.0`
6. Click **Save and Deploy**
7. Site goes live at `powerfulthirst.pages.dev` within minutes

**Connect your domain (powerfulthirst.com):**
- In Cloudflare Pages > your project > **Custom domains**
- Add `powerfulthirst.com` and follow the DNS instructions

### Option 2: Bluehost / Any Web Host

Since Hugo builds to static HTML files, you can host them anywhere:

1. Build the site locally:
   ```bash
   cd ~/Claude_PowerfulThirst/powerfulthirst
   ~/Claude_PowerfulThirst/hugo --gc --minify
   ```
2. Upload the entire `public/` folder contents to your web host's `public_html` directory
3. You'd need to rebuild and re-upload each time you add content (no auto-deploy)

---

## HOW TO POST NEW ARTICLES

### Method 1: CMS Admin Panel (Visual Editor)

Once hosting is set up with Cloudflare Pages + GitHub OAuth:

1. Go to `https://powerfulthirst.com/admin/`
2. Log in with your GitHub account
3. Pick a section: **Feature Articles**, **Drink Recipes**, **Spirits**, or **Podcasts & Videos**
4. Click **New [Article Type]**
5. Fill in title, date, categories, tags
6. Write using the rich text editor (or paste Markdown)
7. Upload images by dragging them in
8. Click **Publish**

**To enable the CMS admin panel, you need GitHub OAuth:**
1. Go to https://github.com/settings/developers
2. Click **New OAuth App**
   - Name: `PowerfulThirst CMS`
   - Homepage URL: `https://powerfulthirst.com`
   - Callback URL: `https://api.netlify.com/auth/done`
3. Note the **Client ID** and generate a **Client Secret**
4. Create a free Netlify account (just for auth, not hosting) at https://netlify.com
5. In Netlify: **Site configuration > Access & security > OAuth > Install provider**
6. Select GitHub, paste your Client ID and Client Secret
7. Update `static/admin/config.yml` - change the backend section to:
   ```yaml
   backend:
     name: github
     repo: ThirstMetrics/powerfulthirst
     branch: main
   ```

### Method 2: Edit Directly on GitHub (Simplest)

1. Go to https://github.com/ThirstMetrics/powerfulthirst
2. Navigate to the right folder:
   - `content/blog/` for feature articles
   - `content/recipes/` for drink recipes
   - `content/spirits/` for spirits content
   - `content/media/` for podcasts & videos
3. Click **Add file > Create new file**
4. Name it `your-article-title.md`
5. Start with this template:

```markdown
---
title: "Your Article Title"
date: 2026-03-07
categories:
  - Industry Analysis
tags:
  - Whiskey
  - Market Trends
description: "Brief description for SEO"
---

Your article content goes here.

## Subheading

More content with **bold** and *italic* text.
```

6. Click **Commit changes** - if on Cloudflare Pages, the site auto-deploys in ~60 seconds

### Method 3: From Your Mac (Terminal)

```bash
cd ~/Claude_PowerfulThirst/powerfulthirst

# Create a new article
cat > content/blog/my-new-article.md << 'EOF'
---
title: "My New Article"
date: 2026-03-07
categories:
  - Industry Analysis
tags:
  - Whiskey
---

Article content here.
EOF

# Preview locally
~/Claude_PowerfulThirst/hugo server

# Push to deploy
git add -A
git commit -m "New article: My New Article"
git push
```

---

## File Reference

```
powerfulthirst/
  content/                  # All content
    blog/                   # Feature articles (12 articles)
    recipes/                # Drink recipes (10 recipes)
    spirits/                # Spirits content
      bourbon/              # Bourbon subsection
      whiskey/              # Whiskey subsection
    media/                  # Podcasts & videos
    tiwin-data/             # Business tools
    about/                  # About page
  static/
    images/                 # Images (logo.png)
    admin/                  # CMS admin panel
  themes/powerfulthirst-theme/
    assets/css/main.css     # All styling
    layouts/                # HTML templates
  hugo.yaml                 # Site config (menus, title, etc.)
```

## Front Matter Fields

| Field            | Required | Example                          |
|-----------------|----------|----------------------------------|
| `title`         | Yes      | `"The State of Craft Spirits"`   |
| `date`          | Yes      | `2026-03-07`                     |
| `categories`    | No       | `- Industry Analysis`            |
| `tags`          | No       | `- Whiskey`                      |
| `description`   | No       | `"Brief SEO description"`        |
| `featured_image`| No       | `"/images/my-photo.jpg"`         |
| `draft`         | No       | `true` (hides from live site)    |

---

## Quick Commands

```bash
# Preview locally
cd ~/Claude_PowerfulThirst/powerfulthirst
~/Claude_PowerfulThirst/hugo server
# Open http://localhost:1313/

# Build static files
~/Claude_PowerfulThirst/hugo --gc --minify

# Deploy (push to GitHub, auto-deploys if Cloudflare Pages is set up)
git add -A && git commit -m "Update" && git push
```
