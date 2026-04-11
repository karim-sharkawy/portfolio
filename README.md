# Portfolio Site Structure

This is a complete ML Engineer portfolio built with Jekyll + GitHub Pages.

## Directory Layout

```
.
├── _config.yml              # Jekyll configuration
├── _layouts/                # HTML layouts
│   ├── default.html        # Main layout (header, nav, footer)
│   ├── post.html           # Blog post layout
│   └── project.html        # Project page layout
├── _projects/              # Project case studies (collections)
│   ├── energy-forecasting.md
│   └── text-classification.md
├── _blog/                  # Blog posts (organized by category)
│   ├── ml-math/
│   │   └── attention-explained.md
│   ├── build-logs/
│   │   └── model-registry.md
│   └── creative/
│       └── symmetry.md
├── assets/
│   └── images/projects/    # Project screenshots/gifs
├── index.md                # Homepage
├── about.md                # About page
├── projects.md             # Projects index
├── blog.md                 # Blog index
├── experience.md           # Experience section
├── now.md                  # Now page
├── Gemfile                 # Ruby dependencies
├── .gitignore
└── README.md               # Setup & deployment instructions
```

## Quick Setup

### 1. Prerequisites

- Ruby 3.1+
- Bundler

### 2. Local Development

```bash
# Clone or navigate to your portfolio
cd _portfolio

# Install gems
bundle install

# Start Jekyll server
bundle exec jekyll serve

# Visit http://localhost:4000
```

### 3. Create New Content

**New Project:**
```bash
# Create file: _projects/your-project.md
---
layout: project
title: "Project Title"
date: 2024-04-01

tech_stack:
  - Python
  - PyTorch
  - FastAPI

results_metrics:
  - "Metric 1: X%"
  - "Metric 2: Y"

demo_url: "https://your-demo.streamlit.app"
code_repo: "https://github.com/your-username/repo"
---

## Your content here...
```

**New Blog Post (ML & Math):**
```bash
# Create file: _blog/ml-math/your-post.md
---
layout: post
title: "Post Title"
date: 2024-04-01
category: "ML & Math"
---

Your markdown here...
```

**New Blog Post (Build Logs or Creative):**
```bash
# Same structure, different folder:
# _blog/build-logs/your-post.md
# _blog/creative/your-post.md
```

### 4. Add Images

```bash
# Place project images in: assets/images/projects/
# Reference in markdown as: ![alt](/assets/images/projects/image.png)
```

---

## Deployment to GitHub Pages

### Method 1: Using GitHub Actions (Recommended)

**Step 1: Create Repository**
```
Repository name: your-username.github.io
(Replace your-username with your actual GitHub username)
Public repo
```

**Step 2: Push Code**
```bash
cd _portfolio
git init
git add .
git commit -m "Initial commit: ML portfolio"
git branch -M main
git remote add origin https://github.com/your-username/your-username.github.io.git
git push -u origin main
```

**Step 3: Enable GitHub Pages**
- Go to repository Settings → Pages
- Select "Deploy from a branch"
- Choose branch: `main`, folder: `/ (root)`
- Save

**Step 4: GitHub Actions Automatically Builds Jekyll**
- GitHub Pages automatically runs Jekyll on push
- Site appears at: `https://your-username.github.io`

### Method 2: Manual Build & Deploy

If you prefer to build locally:

```bash
# Build site locally
bundle exec jekyll build

# This creates _site/ folder
# Commit _site/ to repository
git add _site/
git commit -m "Build site"
git push
```

Then point GitHub Pages to the main branch.

---

## Customization

### Update Personal Info

Edit `_config.yml`:
```yaml
title: Your Name | ML Engineer
description: Your tagline
author: Your Name
url: https://your-username.github.io
repository: your-username/your-username.github.io
```

### Change Navigation Links

In `_layouts/default.html`, update the `<nav>` section with your links.

### Add Resume

```bash
# Place resume at: assets/resume.pdf
# Link from anywhere: [Resume](/assets/resume.pdf)
```

### Add Social Links

Update footer and links in:
- `index.md` (GitHub link)
- `about.md` (LinkedIn link)
- `_layouts/default.html` (footer)

### Customize Colors

Edit inline CSS in `_layouts/default.html`:
```css
a {
  color: #0066cc;  /* Change link color */
}

.demo-button {
  background-color: #0066cc;  /* Change button color */
}
```

---

## Streamlit Demo Integration

Each project links to a Streamlit app. Here's how to set up:

**1. Create Streamlit App**
```bash
# Create: streamlit_app.py
import streamlit as st
import pandas as pd

st.title("Your Demo")
# Your demo code here
```

**2. Deploy to Streamlit Cloud**
- Push to GitHub
- Go to [share.streamlit.io](https://share.streamlit.io)
- Deploy from GitHub repo
- Get URL like: `https://your-username-demo.streamlit.app`

**3. Link from Portfolio**
```markdown
[Live Demo →](https://your-username-demo.streamlit.app)
```

---

## Blog Examples & Ideas

### ML & Math Posts
- How transformers work (step by step)
- Bayesian thinking for ML engineers
- Why batch normalization matters
- The bias-variance tradeoff
- Feature scaling and why it matters
- Loss functions and when to use each

### Build Logs
- "We scaled to 1M predictions/day"
- "The debugging nightmare that taught me..."
- "Why we switched from [X] to [Y]"
- "Post-mortem: The outage and what we learned"
- "Infrastructure lessons from first deployment"

### Creative
- Poems inspired by algorithms
- Essays on technology and humanity
- Reflections on learning and growth
- Metaphors in machine learning

---

## SEO Best Practices

1. **Add meta descriptions** to each page:
```yaml
---
description: "Clear, concise description for search engines"
---
```

2. **Use heading hierarchy**: h1 → h2 → h3 (don't skip levels)

3. **Include keywords naturally**: In titles, headings, first paragraph

4. **Link between posts**: Cross-references improve SEO

5. **Sitemap auto-generates**: `jekyll-sitemap` gem creates `sitemap.xml`

---

## Performance Tips

- Keep pages under 3000 words (readability + performance)
- Compress images: `image.png` → `image_compressed.png`
- Use external CDN for large assets
- Avoid JavaScript where possible (static site = fast)
- Test with Google PageSpeed Insights

---

## Troubleshooting

**Site not updating after push?**
- Wait 30 seconds to 2 minutes for rebuild
- Check GitHub Actions tab for build errors
- Verify `_config.yml` has no YAML syntax errors

**Local site looks different from live?**
```bash
# Ensure you're testing production build
JEKYLL_ENV=production bundle exec jekyll serve
```

**Styling looks broken?**
- Clear browser cache: Ctrl+Shift+Delete
- Clear Jekyll cache: `rm -rf .jekyll-cache/`

**Want custom domain?**
- Buy domain (Namecheap, GoDaddy, etc.)
- Go to repo Settings → Pages
- Enter custom domain
- Follow DNS setup instructions

---

## Next Steps

1. **Fill in your info**: Update names, links, social profiles
2. **Add 1-2 real projects**: Your strongest work as case studies
3. **Write 3 blog posts**: Start building reputation
4. **Deploy**: Push to GitHub, enable Pages
5. **Iterate**: Add content and refine over time

---

## Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Guide](https://pages.github.com/)
- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [YAML Front Matter Reference](https://jekyllrb.com/docs/front-matter/)

---

**Remember**: Done is better than perfect. Start with this template, deploy, then refine based on feedback.
