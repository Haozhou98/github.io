# Research Webpage Development Setup Guide (VS Code Edition)
## Preparing for Claude Code: Haozhou98.github.io
### With Detailed Explanations & Educational Context

---

## 🎯 Project Overview: What Are We Building?

Before diving into setup, understand **what you're creating and why**:

### What is a Jekyll Site?
- **Jekyll** = A static site generator (converts Markdown → HTML)
- **Static** = No database, no backend server; just files served directly
- **Why?** Fast, secure, free hosting on GitHub Pages, version-controlled content
- **Your site** = Your resume/CV/research portfolio, always online and up-to-date

### Why GitHub Pages + Jekyll?
- **Free hosting** (no servers to manage)
- **Automatic deployment** (push code → site updates instantly)
- **Version control** (all changes tracked in Git, easy rollback)
- **Academic friendly** (clean, minimal, professional look)
- **Maintenance-free** (GitHub handles infrastructure)

### The Workflow
```
You write Markdown → Jekyll converts to HTML → GitHub Pages hosts it → World sees it
```

---

## Phase 1: Preparation & VS Code Setup

### Why This Matters
VS Code is your editor. We're setting it up with extensions and configurations to make Jekyll development smoother.

---

### Step 1.1: Install Required Software

#### Ruby & Bundler
**What are they?**
- **Ruby** = A programming language (Jekyll is written in Ruby)
- **Bundler** = A dependency manager (installs exact versions of Jekyll + plugins)

**Check if you have Ruby:**
```bash
ruby --version
```

**Expected output:** `ruby 3.x.x` or higher

**If you don't have it (or need to update):**
```bash
# macOS with Homebrew
brew install ruby

# Verify installation
ruby --version
```

**Why Bundler?**
Think of it like a "requirements.txt" for Ruby. It locks dependency versions so Claude Code and you use the exact same Jekyll version (no "works on my machine" problems).

---

### Step 1.2: Install VS Code Extensions (Optional but Recommended)

**Why?** These make Jekyll development faster and easier.

1. **Open VS Code**
2. Go to **Extensions** (Cmd+Shift+X on Mac)
3. Search and install:
   - **"Jekyll Run"** (by Devin Lindauer) — one-click Jekyll serve
   - **"Markdown All in One"** (by Yu Zhang) — markdown syntax highlighting
   - **"YAML"** (by RedHat) — YAML config syntax highlighting
   - **"Live Server"** (by Ritwick Dey) — live reload for HTML/CSS

**What do they do?**
- **Jekyll Run:** Saves typing `bundle exec jekyll serve` every time
- **Markdown:** Makes `.md` files easier to read and edit
- **YAML:** Helps you see syntax errors in `_config.yml` (before they break the build)
- **Live Server:** Instant browser reload when you save files

---

### Step 1.3: Create Project Folder in VS Code

**Why organize this way?**
Keep your project in a dedicated folder so all related files are together and easy to manage.

```bash
# In Terminal
cd ~/Developer
mkdir Haozhou98.github.io
cd Haozhou98.github.io

# Open in VS Code
code .
```

**What just happened?**
- Created folder: `~/Developer/Haozhou98.github.io/`
- Opened it in VS Code (the `.` means "current directory")

---

## Phase 2: GitHub Repository Creation

### Why Do This First?
GitHub is the source of truth. Your local code syncs to GitHub, and GitHub Pages automatically deploys from there. If you set it up correctly now, you save hours of troubleshooting later.

---

### Step 2.1: Create GitHub Repository

**The most important part: Naming**

GitHub Pages requires an **exact naming convention** for personal sites:
```
Haozhou98.github.io
```

Why? GitHub looks for this specific name and auto-enables Pages hosting.

**Steps:**
1. Go to **github.com** (logged in as yourself)
2. Click **"+"** (top right) → **"New repository"**
3. Fill in:
   - **Repository name:** `Haozhou98.github.io` (MUST be exact)
   - **Description:** "Personal research webpage for Haozhou Zhou"
   - **Visibility:** **Public** (required for free GitHub Pages)
   - **Initialize with:** Add README (checkbox)
   - **Add .gitignore:** Select **Ruby**
   - **License:** MIT
4. Click **"Create repository"**

**What GitHub just did:**
- Created an empty repo with placeholder README and Ruby .gitignore
- Enabled GitHub Pages (automatic, because of the repo name)
- Set up a `main` branch (where your code lives)

---

### Step 2.2: Clone Repository Locally

**Why clone?** You're downloading GitHub's repo to your computer so you can edit it locally.

In VS Code Terminal (Ctrl+`):
```bash
cd ~/Developer
git clone https://github.com/Haozhou98/Haozhou98.github.io.git
cd Haozhou98.github.io
code .
```

**What happened:**
- Downloaded repo from GitHub → `~/Developer/Haozhou98.github.io/`
- Opened it in VS Code
- Now you have a `.git` folder (hidden) that tracks all changes

**Why `.git` matters:** It's how Git knows "this is a repo." Never delete it, and add it to `.gitignore` so you don't accidentally commit it twice.

---

## Phase 3: Dependency Management with Bundler

### Why This Step?
Bundler ensures everyone (you, Claude Code, GitHub's build system) uses the **exact same versions** of Jekyll and plugins. This prevents mysterious "works locally but fails on GitHub" errors.

---

### Step 3.1: Create Gemfile

**What is Gemfile?**
A text file that lists all Ruby dependencies (called "gems"). Like `requirements.txt` in Python.

In VS Code:
1. **File** → **New File**
2. Name it: `Gemfile` (no extension, case-sensitive)
3. Paste this:

```ruby
source "https://rubygems.org"

# Jekyll and theme
gem "jekyll", "~> 4.3.0"
gem "minima", "~> 2.5"

# Plugins
gem "jekyll-feed", "~> 0.12"
gem "jekyll-seo-tag", "~> 2.8"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag"
end

# Windows support
platforms :mingw, :x64_mingw, :mswin do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", platforms: [:mingw, :x64_mingw, :mswin]
```

**What each part means:**

| Line | Purpose |
|------|---------|
| `source "https://rubygems.org"` | Where to download gems from (official Ruby repository) |
| `gem "jekyll", "~> 4.3.0"` | Install Jekyll version 4.3.x (the `~>` means "4.3 or newer, but not 5.0") |
| `gem "minima"` | The theme (how your site looks) |
| `gem "jekyll-feed"` | Plugin that generates RSS feed (people can subscribe to your posts) |
| `gem "jekyll-seo-tag"` | Plugin that auto-generates SEO metadata (helps search engines find your site) |
| `group :jekyll_plugins` | These gems are only used during Jekyll builds, not in production |
| `platforms :mingw...` | Windows-specific support (safe to ignore if on Mac, but good to have) |

---

### Step 3.2: Install Dependencies

In VS Code Terminal:
```bash
gem install bundler  # One-time: installs Bundler itself
bundle install       # Reads Gemfile and installs all gems
```

**What happened:**
- `bundle install` read `Gemfile`
- Downloaded Jekyll, minima theme, plugins
- Created `Gemfile.lock` (a snapshot of exact versions used)
- Stored them in a local folder (`.bundle/` or `/usr/local/var/...`)

**Why Gemfile.lock matters:**
When Claude Code clones your repo and runs `bundle install`, they'll get the **exact same versions** because Bundler reads `Gemfile.lock`. This is crucial for reproducibility.

**Expected output:**
```
Bundle complete! 11 Gemfile dependencies, 37 gems now installed.
```

---

## Phase 4: Jekyll Project Structure

### Why Organize Files This Way?
Jekyll has conventions. Files in specific folders (`_layouts/`, `_posts/`, etc.) are automatically processed. Files outside those folders are copied as-is. This organization lets Jekyll know which files to transform and which to serve directly.

---

### Step 4.1: Create Project Skeleton

In VS Code, create these folders (right-click → New Folder):
- `_layouts/` — HTML templates for different page types
- `_includes/` — Reusable HTML snippets (e.g., header, footer)
- `assets/css/` — Stylesheets
- `assets/images/` — Photos, logos, etc.
- `_posts/` — Blog posts (optional, but good to have)

**Why each folder?**

| Folder | Purpose | Example |
|--------|---------|---------|
| `_layouts/` | Templates Jekyll uses to wrap your content | `default.html` wraps all pages |
| `_includes/` | Reusable components | `header.html` included on every page |
| `assets/css/` | Stylesheets (CSS files) | `style.css` — defines colors, fonts, layout |
| `assets/images/` | Images served as-is | Your headshot.jpg, logos |
| `_posts/` | Blog posts (optional) | Future articles about your research |

**Jekyll's build process:**
```
Source files (_layouts, _includes, content.md)
        ↓
   Jekyll processes
        ↓
Generated HTML files
        ↓
Copied to _site/ folder
        ↓
GitHub Pages serves _site/ to the world
```

---

### Step 4.2: Create _config.yml

**What is this file?**
The "brain" of your Jekyll site. Every setting goes here: title, description, theme, plugins, etc.

Create new file: `_config.yml` (in root directory, NOT in any subfolder)

Paste:
```yaml
# ========================================
# Site Settings
# ========================================

title: "Haozhou Zhou"
description: "Explainable AI for Predictive Maintenance & Edge Deployment"
author: "Haozhou Zhou"
email: "zhou.haoz@northeastern.edu"
url: "https://haozhou98.github.io"
baseurl: ""  # Leave empty for username.github.io

# ========================================
# Theme
# ========================================

theme: minima  # The 'minima' theme handles most styling

# ========================================
# Plugins
# ========================================

plugins:
  - jekyll-feed       # Generates RSS feed
  - jekyll-seo-tag    # Auto-adds SEO meta tags

# ========================================
# Build Settings
# ========================================

# Files/folders to EXCLUDE from the build
exclude:
  - Gemfile           # Ruby dependency file
  - Gemfile.lock      # Lock file
  - README.md         # GitHub readme
  - WEBPAGE_DEV_*.md  # Development docs (not part of site)
  - .gitignore        # Git config

# Files to INCLUDE even if they start with underscore
include: []

# ========================================
# Markdown Processor
# ========================================

markdown: kramdown

# ========================================
# Output Settings
# ========================================

permalink: /:title/  # URL structure for posts (optional)
```

**Key explanations:**

| Setting | What It Does | Example |
|---------|-------------|---------|
| `title` | Site name (appears in browser tab, header) | "Haozhou Zhou" |
| `description` | Short tagline (used in search engines) | "Explainable AI..." |
| `url` | Your live site URL | "https://haozhou98.github.io" |
| `theme: minima` | Uses minima's default CSS (save time, clean look) | Provides fonts, colors, layout |
| `plugins` | Extra features | jekyll-feed auto-generates RSS |
| `exclude` | Files Jekyll ignores | Gemfile, documentation, etc. |

---

## Phase 5: Create Core Pages & Layouts

### Why This Structure?
- **Layouts** = HTML skeleton (header, footer, navigation)
- **Content** = Your Markdown files (hero, bio, publications, etc.)
- **Jekyll merges them** = Markdown content + HTML layout → final page

---

### Step 5.1: Create index.md (Homepage)

Create new file: `index.md` (in root directory)

Paste:
```markdown
---
layout: home
title: Haozhou Zhou
description: Explainable AI for Predictive Maintenance & Edge Deployment
---

# Haozhou Zhou

**PhD Candidate, Industrial Engineering**  
Northeastern University | Boston, MA

---

## Research Focus

Develop explainable and resource-aware AI models for reliability with focus on predictive maintenance (PdM) and edge deployment. Deliver interpretable predictors that meet latency and memory budgets and are engineered for transfer into industrial environments.

---

## Current Position

**Northeastern University — Research Assistant (2024 – Present)**

- Led a 4-person research team applying AI to improve equipment reliability; published three first-author papers.
- Co-developed NSF-funded online data science curriculum; coordinated pilot program with Oregon State faculty.
- Presented research at three prestigious industrial engineering conferences (RAMS 2025, IISE 2024 & 2025).

---

## Teaching

**IE4530/31 Manufacturing Systems and Techniques — Instructor (Spring 2026)**

**Northeastern University — Graduate Teaching Assistant & Lab Instructor (2022 – 2024)**
- Instructed 140+ students in data mining and hypothesis testing.
- Hosted the Cyber-Physical Manufacturing Lab for 5 semesters; trained 150+ students on PLC/robotics.

---

## Selected Publications

*Coming soon — Claude Code will populate this section with full citations and links.*

---

## Education

- **Ph.D. in Industrial Engineering**, Northeastern University (Expected 2026)
  - GPA: 3.84/4.0 | Research in PdM, control systems, and applied AI

- **M.Sc. in Data Analytics Engineering**, Northeastern University (2022)
  - GPA: 3.83/4.0 | Recipient of outstanding master's student leadership award

- **B.S. in Statistics**, Dongbei University of Finance and Economics (2020)
  - Top 3% in national college entrance exam | Recipient of university scholarship

---

## Contact

- **Email:** [zhou.haoz@northeastern.edu](mailto:zhou.haoz@northeastern.edu)
- **GitHub:** [github.com/Haozhou98](https://github.com/Haozhou98)
- **LinkedIn:** [linkedin.com/in/haozhouzhou9810](https://linkedin.com/in/haozhouzhou9810)
- **Phone:** 857-869-3848
```

**What's that YAML at the top?**
```yaml
---
layout: home
title: Haozhou Zhou
---
```

This is **front matter** — Jekyll metadata telling it:
- `layout: home` — Use the `home` layout (Jekyll will look for `_layouts/home.html`)
- `title` — Page title (appears in browser tab, override site.title)

---

### Step 5.2: Create _layouts/default.html

This is the **base template** that wraps all pages.

Create new file: `_layouts/default.html`

Paste:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ page.title | default: site.title }}</title>
    <meta name="description" content="{{ page.description | default: site.description }}">
    
    <!-- Link to stylesheet -->
    <link rel="stylesheet" href="{{ '/assets/css/style.css' | relative_url }}">
    
    <!-- SEO plugin (auto-generated) -->
    {% seo %}
</head>
<body>
    <!-- Header -->
    <header>
        <h1><a href="/">{{ site.title }}</a></h1>
        <p>{{ site.description }}</p>
    </header>

    <!-- Main content -->
    <main>
        {{ content }}
    </main>

    <!-- Footer -->
    <footer>
        <p>&copy; 2025 {{ site.author }}. All rights reserved.</p>
    </footer>
</body>
</html>
```

**What's happening here?**

| Code | Meaning | Example |
|------|---------|---------|
| `{{ page.title }}` | Insert page title from front matter | "Haozhou Zhou" |
| `{{ site.title }}` | Insert site title from `_config.yml` | "Haozhou Zhou" |
| `{{ content }}` | Insert the processed Markdown content | Your homepage content |
| `{% seo %}` | Plugin auto-adds `<meta>` tags for SEO | Helps Google index your site |
| `\| default:` | If empty, use default | If page.title missing, use site.title |
| `\| relative_url` | Convert `/assets/...` to work on any domain | Safer than hardcoding URLs |

---

### Step 5.3: Create assets/css/style.css

This file controls **how your site looks** (colors, fonts, spacing).

Create new file: `assets/css/style.css`

Paste:
```css
/* ==========================================================
   RESET & BASE STYLES
   ========================================================== */

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* ==========================================================
   BODY & LAYOUT
   ========================================================== */

body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #ffffff;
    padding: 40px 20px;
    max-width: 900px;
    margin: 0 auto;
}

/* Responsive: smaller padding on mobile */
@media (max-width: 600px) {
    body {
        padding: 20px 15px;
    }
}

/* ==========================================================
   HEADER (Top section with title)
   ========================================================== */

header {
    text-align: center;
    margin-bottom: 50px;
    padding-bottom: 30px;
    border-bottom: 2px solid #e0e0e0;
}

header h1 {
    font-size: 2.5em;
    margin-bottom: 10px;
    font-weight: 600;
}

header h1 a {
    color: #000;
    text-decoration: none;
}

header h1 a:hover {
    color: #0066cc;
}

header p {
    color: #666;
    font-size: 1.1em;
    margin: 0;
}

/* ==========================================================
   MAIN CONTENT
   ========================================================== */

main {
    margin-bottom: 60px;
    line-height: 1.8;
}

/* ==========================================================
   HEADINGS (h2, h3, etc.)
   ========================================================== */

h2 {
    font-size: 1.8em;
    margin-top: 40px;
    margin-bottom: 20px;
    color: #000;
    border-bottom: 1px solid #ddd;
    padding-bottom: 10px;
    font-weight: 600;
}

h3 {
    font-size: 1.3em;
    margin-top: 25px;
    margin-bottom: 15px;
    color: #222;
    font-weight: 600;
}

h4 {
    font-size: 1.1em;
    margin-top: 20px;
    margin-bottom: 10px;
    color: #333;
}

/* ==========================================================
   PARAGRAPHS & TEXT
   ========================================================== */

p {
    margin-bottom: 15px;
    text-align: justify;
}

strong {
    font-weight: 600;
    color: #000;
}

em {
    font-style: italic;
    color: #555;
}

/* ==========================================================
   LINKS
   ========================================================== */

a {
    color: #0066cc;
    text-decoration: none;
    border-bottom: 1px solid transparent;
    transition: all 0.2s ease;
}

a:hover {
    border-bottom: 1px solid #0066cc;
    color: #004499;
}

/* ==========================================================
   LISTS
   ========================================================== */

ul, ol {
    margin-left: 25px;
    margin-bottom: 20px;
}

li {
    margin-bottom: 10px;
    line-height: 1.7;
}

/* ==========================================================
   FOOTER
   ========================================================== */

footer {
    text-align: center;
    color: #999;
    font-size: 0.95em;
    padding-top: 30px;
    border-top: 1px solid #ddd;
    margin-top: 60px;
}

/* ==========================================================
   RESPONSIVE DESIGN (Mobile-friendly)
   ========================================================== */

@media (max-width: 768px) {
    header h1 {
        font-size: 1.8em;
    }

    h2 {
        font-size: 1.5em;
    }

    main {
        font-size: 0.95em;
    }
}
```

**Why these CSS choices?**

| CSS Rule | Why It Matters |
|----------|---------------|
| `font-family: -apple-system...` | Uses system fonts (no slow downloads), looks native on each OS |
| `max-width: 900px` | Readable line length (research shows 60-90 chars per line is optimal) |
| `margin: 0 auto` | Centers content on wide screens |
| `line-height: 1.6` | Extra space between lines (easier to read than default) |
| `border-bottom: 1px solid #ddd` | Subtle line under headers (clean separation) |
| `@media (max-width: 600px)` | Mobile responsive (text smaller on phones) |
| `color: #0066cc` for links | Professional blue, stands out without being garish |

---

## Phase 6: Test Locally

### Why This Step?
You want to see your site **before** pushing to GitHub. Local testing catches errors fast.

---

### Step 6.1: Serve Jekyll Locally

In VS Code Terminal:
```bash
bundle exec jekyll serve
```

**What happened?**
- `bundle exec` — Run the Jekyll command using your bundled gems (not system Ruby)
- `jekyll serve` — Start a local web server

**Expected output:**
```
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

---

### Step 6.2: View in Browser

1. Open browser (Chrome, Safari, Firefox, etc.)
2. Go to: **http://localhost:4000**
3. You should see your homepage

**What you're seeing:**
- Jekyll converted `index.md` → `index.html`
- Applied the `default.html` layout
- Styled it with `style.css`
- Served it locally on port 4000

---

### Step 6.3: Live Editing

**Try this:**
1. Edit `index.md` — change "Haozhou Zhou" to "Test Name"
2. Save the file (Cmd+S)
3. Refresh browser (Cmd+R)
4. See the change instantly

**Why?** Jekyll watches your files. When you save, it rebuilds automatically (2-3 seconds).

---

### Step 6.4: Stop Server

Press **Ctrl+C** in Terminal to stop Jekyll.

---

## Phase 7: Git Setup & Initial Commit

### Why Version Control?
Git tracks changes so you can:
- See what changed and when
- Revert to old versions if something breaks
- Collaborate (Claude Code can see your commits)

---

### Step 7.1: Check .gitignore

**What is .gitignore?**
A file telling Git which files to **ignore** (not track).

Open `.gitignore` (created by GitHub):

It should have Ruby defaults. Add these lines to the end:
```
# Jekyll
_site/
.jekyll-cache/
.jekyll-metadata
Gemfile.lock

# IDEs & OS
.vscode/
.DS_Store
Thumbs.db
*.swp
*.swo

# Development docs
WEBPAGE_DEV_*.md
PROJECT.md
```

**Why exclude these?**

| File/Folder | Why Ignore |
|-------------|-----------|
| `_site/` | Auto-generated; don't version control generated files |
| `Gemfile.lock` | Git should track it, but let me explain below |
| `.vscode/` | Your personal editor config; not part of the project |
| `.DS_Store` | macOS junk file |
| `WEBPAGE_DEV_*.md` | Documentation, not part of the site |

**About Gemfile.lock:** Actually, **you should commit it** (remove from .gitignore). This ensures Claude Code uses exact same gem versions. Let me correct that in the file.

---

### Step 7.2: Create Initial Commit

In VS Code Terminal:
```bash
# Check status
git status

# Stage all changes
git add .

# Commit with message
git commit -m "Initial Jekyll scaffolding with minima theme"

# Push to GitHub
git push origin main
```

**What each command does:**

| Command | Purpose |
|---------|---------|
| `git status` | Shows what changed (modified, new, deleted files) |
| `git add .` | Stage all changes (prepare to commit) |
| `git commit -m "..."` | Save changes with a message (like a checkpoint) |
| `git push origin main` | Send commits to GitHub's `main` branch |

**Verify on GitHub:**
1. Go to github.com/Haozhou98/Haozhou98.github.io
2. Refresh page
3. You should see all your files

---

## Phase 8: Create Documentation for Claude Code

### Why Document?
Claude Code will read these docs to understand what to build and why.

---

### Step 8.1: Create PROJECT.md

Create new file: `PROJECT.md` (in root directory)

Paste:
```markdown
# Haozhou Research Webpage Project

## Overview
Building a minimal academic research webpage using Jekyll + GitHub Pages.

## Tech Stack
- **Generator:** Jekyll 4.3.0
- **Theme:** Minima
- **Plugins:** jekyll-feed, jekyll-seo-tag
- **Hosting:** GitHub Pages (automatic deployment)
- **Language:** Markdown (content) + HTML/CSS (templates)

## Project Structure
```
Haozhou98.github.io/
├── _layouts/
│   └── default.html          # Base template
├── _includes/                # Reusable components
├── assets/
│   ├── css/
│   │   └── style.css         # Styling
│   └── images/               # Headshot (will add later)
├── _posts/                   # Blog posts (optional)
├── _config.yml               # Jekyll config
├── Gemfile                   # Dependencies
├── index.md                  # Homepage
└── README.md
```

## Key Files & What They Do
- **_config.yml** — Site settings (title, description, plugins)
- **index.md** — Homepage content (your bio, publications, etc.)
- **_layouts/default.html** — HTML wrapper for all pages
- **assets/css/style.css** — All styling (colors, fonts, spacing)
- **Gemfile** — Dependency list (ensures same gems everywhere)
- **Gemfile.lock** — Locked versions of gems (reproducibility)

## Current Status
✅ Repository created on GitHub  
✅ Jekyll scaffolded locally  
✅ Theme configured (minima)  
✅ Homepage created with placeholder content  
✅ CSS styled for academic look  
✅ Local testing working (localhost:4000)  
✅ Git initialized and pushed to GitHub  

## What's Missing (for Claude Code)
- [ ] Full publication section with citations & links
- [ ] Teaching section expansion
- [ ] Research projects showcase
- [ ] Headshot photo (will be provided later)
- [ ] Publication DOIs/URLs (from user)
- [ ] Final CSS refinements
- [ ] Mobile responsiveness testing
- [ ] GitHub Pages deployment verification

## How to Build (for Claude Code)
1. Clone: `git clone https://github.com/Haozhou98/Haozhou98.github.io.git`
2. Install: `cd Haozhou98.github.io && bundle install`
3. Test: `bundle exec jekyll serve` (visit localhost:4000)
4. Edit: Update `index.md` and `assets/css/style.css` as per requirements
5. Commit: `git add . && git commit -m "..."` 
6. Push: `git push origin main`
7. Verify: Check GitHub Pages deployment (should be auto)

## Key Deployment Notes
- **GitHub Pages enabled:** Yes (automatic for `username.github.io` repos)
- **Build command:** GitHub auto-runs `jekyll build` on push
- **Build output:** Saved to `_site/` folder (auto-generated, don't edit)
- **Live site:** https://haozhou98.github.io (updates ~1-2 min after push)

## User Preferences
- **Design:** Minimal, clean, academic (like hsword.github.io)
- **Domain:** username.github.io (no custom domain yet)
- **Content:** Publications from resume + teaching roles
- **Headshot:** Will be added later (optional for v1.0)

## Contact Info
- **User:** Hal (Haozhou Zhou)
- **Email:** zhou.haoz@northeastern.edu
- **GitHub:** Haozhou98

---

**Ready for Claude Code to take over!**
```

---

## Phase 9: Final Verification Checklist

Before telling Claude Code to start, verify everything:

In VS Code Terminal:
```bash
# 1. Check local build
bundle exec jekyll serve
# Visit http://localhost:4000 — should see homepage
# Press Ctrl+C to stop

# 2. Check Git status
git status
# Should show "On branch main, nothing to commit, working tree clean"

# 3. Check GitHub
# Go to github.com/Haozhou98/Haozhou98.github.io
# Should see all your files (index.md, _config.yml, etc.)
```

**Verification Checklist:**
- [ ] `bundle install` worked without errors
- [ ] `bundle exec jekyll serve` starts and shows "Server running..."
- [ ] http://localhost:4000 loads your homepage
- [ ] Editing `index.md` auto-reloads in browser
- [ ] `git status` shows clean working tree
- [ ] GitHub repo shows all files you created
- [ ] GitHub Pages is enabled (Settings → Pages → Live)
- [ ] `.gitignore` excludes `_site/` and `Gemfile.lock` (no wait, include Gemfile.lock!)

---

## Complete File Checklist

After Phase 9, your project should have:

```
Haozhou98.github.io/
├── .git/                        (hidden)
├── .gitignore                   ✅
├── Gemfile                      ✅
├── Gemfile.lock                 ✅ (auto-created by bundle install)
├── README.md                    ✅ (from GitHub)
├── _config.yml                  ✅
├── _layouts/
│   └── default.html             ✅
├── _includes/                   ✅ (empty for now)
├── _posts/                      ✅ (empty for now)
├── assets/
│   ├── css/
│   │   └── style.css            ✅
│   └── images/                  ✅ (will add headshot later)
├── index.md                     ✅
├── PROJECT.md                   ✅
└── WEBPAGE_DEV_REQUIREMENTS.md   (in /clawd/, not in repo)
```

---

## Next Steps: Handing Off to Claude Code

Once you've completed all phases:

1. **Tell me you're done:** "Ready for Claude Code"
2. **I'll spawn Claude Code** as a sub-agent with:
   - GitHub repo URL
   - WEBPAGE_DEV_REQUIREMENTS.md (what to build)
   - Instructions to pull your repo and build content
3. **Claude Code will:**
   - Clone your repo
   - Read requirements
   - Expand index.md with full publications, education, teaching
   - Refine CSS for polish
   - Test locally
   - Commit and push
   - Verify GitHub Pages deployment
   - Report back to you

---

## Why This Approach is Good

✅ **You learn:** Understanding each step helps you maintain the site later  
✅ **Reproducible:** Gemfile.lock ensures consistency  
✅ **Version controlled:** All changes tracked  
✅ **Scalable:** Easy for Claude Code to continue building on your foundation  
✅ **Fast feedback:** Local testing catches errors before they hit GitHub  
✅ **Professional:** Minimal, clean design that scales  

---

## Common Questions

### Q: Why Bundler instead of installing Jekyll globally?
**A:** Bundler creates an isolated environment. If you install 10 projects, each can use different Jekyll versions without conflicts. It's like virtual environments in Python.

### Q: Why not use GitHub's Jekyll template?
**A:** You can, but manual setup teaches you how everything works. Plus, you can customize more easily.

### Q: Can I edit the site directly on GitHub?
**A:** Yes, but local testing is safer. You won't catch CSS errors until it's live.

### Q: What if Claude Code breaks something?
**A:** Git tracks everything. Rollback with `git revert <commit>` or `git reset --hard <commit>`.

### Q: When do I add my headshot?
**A:** Anytime. Drop it in `assets/images/headshot.jpg`, update index.md to include it, and it deploys instantly.

### Q: How do I update the site after launch?
**A:** Edit index.md (add publications, teaching, etc.), test locally, push to GitHub. Done.

---

## You're Ready!

Your setup is **educational, professional, and production-ready**. When you've completed all phases and verified the checklist, Claude Code can take it from here.

The foundation you've built is solid. Now come the fun parts: populating content, refining design, and getting your research out to the world. 🚀

---

*Created: 2026-02-05 | VS Code Edition | Ready for Claude Code handoff*
