# PowerfulThirst.com - Setup & Usage Directions

## Overview

PowerfulThirst.com is a Hugo static site with a built-in CMS admin panel (Decap CMS) for creating and editing articles through your browser. When you push changes to GitHub, the site automatically builds and deploys to Cloudflare Pages.

---

## STEP 1: Create the GitHub Repository

1. Go to https://github.com/new (log in as ThirstMetrics)
2. Repository name: `powerfulthirst`
3. Set it to **Private** (or Public, your choice)
4. Do NOT check "Add a README" or any other options
5. Click **Create repository**

## STEP 2: Push Your Code to GitHub

Open Terminal on your Mac and run these commands one at a time:

```bash
cd ~/Claude_PowerfulThirst/powerfulthirst

git remote add origin https://github.com/ThirstMetrics/powerfulthirst.git

git push -u origin main
```

You may be prompted to log in to GitHub. If so, you'll need a **Personal Access Token**:

1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Give it a name like "PowerfulThirst Mac"
4. Check the `repo` scope
5. Click Generate, copy the token
6. Use it as your password when prompted in Terminal

## STEP 3: Set Up Cloudflare Pages (Free Hosting)

1. Go to https://dash.cloudflare.com/ and sign in (or create a free account)
2. In the left sidebar, click **Workers & Pages**
3. Click **Create** then **Pages** tab, then **Connect to Git**
4. Authorize Cloudflare to access your GitHub, select the `powerfulthirst` repo
5. Configure the build:
   - **Build command:** `hugo --gc --minify`
   - **Build output directory:** `public`
   - **Environment variable:** Add `HUGO_VERSION` = `0.142.0`
6. Click **Save and Deploy**
7. Your site will be live at `powerfulthirst.pages.dev` within minutes

### Connect Your Domain (powerfulthirst.com)

1. In Cloudflare Pages > your project > **Custom domains**
2. Click **Set up a custom domain**
3. Enter `powerfulthirst.com`
4. Follow the DNS instructions (add a CNAME record pointing to your Pages project)

## STEP 4: Set Up the CMS Admin Panel

The admin panel at `powerfulthirst.com/admin/` needs GitHub authentication to work. There are two options:

### Option A: Use Cloudflare Pages + GitHub OAuth (Recommended)

Since Decap CMS needs a backend, set up GitHub as the backend:

1. Edit the file `static/admin/config.yml`
2. Replace the `backend` section at the top with:

```yaml
backend:
  name: github
  repo: ThirstMetrics/powerfulthirst
  branch: main
```

3. Register an OAuth App on GitHub:
   - Go to https://github.com/settings/developers
   - Click **New OAuth App**
   - Application name: `PowerfulThirst CMS`
   - Homepage URL: `https://powerfulthirst.com`
   - Authorization callback URL: `https://api.netlify.com/auth/done`
   - Click Register, then note your **Client ID** and generate a **Client Secret**

4. Set up an auth provider (needed for the OAuth flow):
   - Create a free Netlify account at https://netlify.com (just for auth, not hosting)
   - Go to **Site configuration > Access & security > OAuth**
   - Under **Authentication providers**, click **Install provider**
   - Select **GitHub**, paste your Client ID and Client Secret
   - Save

5. Add this line to `static/admin/config.yml`:
```yaml
base_url: https://api.netlify.com
```

6. Commit and push your changes. The admin panel will be live.

### Option B: Edit Files Directly in GitHub

If you prefer simplicity, just go to https://github.com/ThirstMetrics/powerfulthirst and:

1. Navigate to `content/blog/`
2. Click **Add file > Create new file**
3. Name it like `my-new-article.md`
4. Use this template at the top:

```markdown
---
title: "Your Article Title"
date: 2026-03-07
categories:
  - Industry Analysis
tags:
  - Spirits
  - Market Trends
description: "Brief description for SEO"
featured_image: "/images/your-image.jpg"
---

Your article content goes here in Markdown format.

## Subheading

More content...
```

5. Commit the file - the site auto-deploys in about 60 seconds

---

## Day-to-Day: How to Write a New Article

### Using the CMS Admin Panel (after Step 4 Option A)

1. Go to `https://powerfulthirst.com/admin/`
2. Log in with your GitHub account
3. Click **Blog Posts** in the sidebar
4. Click **New Blog Post**
5. Fill in the title, date, categories, tags
6. Write your article using the rich text editor
7. Upload images by dragging them in
8. Click **Publish** when ready

### Using GitHub Directly (Option B)

1. Go to `https://github.com/ThirstMetrics/powerfulthirst/tree/main/content/blog`
2. Create a new `.md` file with the front matter template above
3. Commit - site deploys automatically

---

## Project Structure - Key Files

```
powerfulthirst/
  content/              # All your content lives here
    _index.md           # Homepage content
    about/_index.md     # About page
    blog/               # Blog articles (add new .md files here)
    spirits/            # Spirits section
    tiwin-data/         # TiWin Data section
  static/
    images/             # Upload images here (logo.png is here)
    admin/              # CMS admin panel config
  themes/powerfulthirst-theme/
    assets/css/main.css # Site styling
    layouts/            # HTML templates
  hugo.yaml             # Main site configuration
  .github/workflows/    # Auto-deploy pipeline
```

## Article Front Matter Reference

Every article needs this block at the top:

| Field            | Required | Example                          |
|-----------------|----------|----------------------------------|
| `title`         | Yes      | `"The State of Craft Spirits"`   |
| `date`          | Yes      | `2026-03-07`                     |
| `categories`    | No       | `- Industry Analysis`            |
| `tags`          | No       | `- Whiskey`                      |
| `description`   | No       | `"Brief SEO description"`        |
| `featured_image`| No       | `"/images/my-photo.jpg"`         |
| `draft`         | No       | `true` (hides from site)         |

## Running the Site Locally (Optional)

If you want to preview changes before publishing:

```bash
cd ~/Claude_PowerfulThirst/powerfulthirst
~/Claude_PowerfulThirst/hugo server
```

Then open http://localhost:1313/ in your browser.

---

## Quick Reference Commands

```bash
# Preview the site locally
cd ~/Claude_PowerfulThirst/powerfulthirst
~/Claude_PowerfulThirst/hugo server

# Build the site (output goes to public/)
~/Claude_PowerfulThirst/hugo --gc --minify

# Push changes to deploy
git add -A
git commit -m "Added new article"
git push
```

## Need Help?

- Hugo docs: https://gohugo.io/documentation/
- Decap CMS docs: https://decapcms.org/docs/
- Cloudflare Pages docs: https://developers.cloudflare.com/pages/
