# Setup & Deployment Quick Start

Follow this guide to get your ML Engineer portfolio live in <30 minutes.

## 5-Minute Quick Start

### 1. Clone/Copy the Portfolio

```bash
cd ~
git clone https://github.com/your-username/ml-portfolio.git my-portfolio
cd my-portfolio
```

Or download the files and extract them.

### 2. Install Dependencies

```bash
# Install Ruby gems
bundle install
```

### 3. Run Locally

```bash
bundle exec jekyll serve
# Visit: http://localhost:4000
```

### 4. Update Your Info

Open `_config.yml` and change:
```yaml
title: Your Name | ML Engineer
author: Your Name
url: https://your-username.github.io
repository: your-username/your-username.github.io
```

### 5. Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "Initial portfolio"
git branch -M main
git remote add origin https://github.com/your-username/your-username.github.io.git
git push -u origin main
```

Enable GitHub Pages in repo Settings. Site goes live at `https://your-username.github.io`.

---

## File-by-File Customization

### Homepage (`index.md`)
- Update "2-3 line summary" with your pitch
- Replace project links with your real projects
- Update stats (years in ML, projects built, etc.)
- Update links (GitHub, resume, etc.)

### About (`about.md`)
- Replace "Your Name" with actual name
- Update background and experience
- List your actual tech stack
- Add personal interests/philosophy

### Projects (`projects.md` + `_projects/*.md`)
- Replace example projects with yours
- Each project needs: title, problem, tech, impact, demo link
- See `energy-forecasting.md` for full template

### Blog (`blog.md` + `_blog/*/*.md`)
- Update blog categories as needed
- Each post is a markdown file in the right category folder
- See example posts for structure

### Experience (`experience.md`)
- Replace companies/roles with yours
- Focus on what you built, not what you did
- Add metrics/impact where possible

### Now (`now.md`)
- Update monthly with current work
- Keep it honest and specific
- Shows people you're actively learning/building

---

## Adding New Content

### New Project

Create `_projects/your-project-slug.md`:

```yaml
---
layout: project
title: "Your Project Title"
date: 2024-04-15

tech_stack:
  - Python
  - PyTorch
  - Kubernetes

results_metrics:
  - "**Metric 1**: 20% improvement"
  - "**Metric 2**: 3x speedup"

demo_url: "https://your-demo.streamlit.app"
code_repo: "https://github.com/your-username/repo"
---

## Your project description here...
```

### New Blog Post (ML & Math)

Create `_blog/ml-math/your-post-slug.md`:

```yaml
---
layout: post
title: "Post Title"
date: 2024-04-15
category: "ML & Math"
---

Your content here...
```

Same structure for `_blog/build-logs/` and `_blog/creative/`.

### New Experience Entry

Add to `experience.md`:

```markdown
## Role Title | Company (YYYY-YYYY)

### What I Built

Specific description...

### Technologies

List here...

### Impact

Metrics or outcomes...
```

---

## Streamlit Demo Setup

For each project with a "Live Demo":

1. **Create Streamlit app** (`streamlit_app.py`):
```python
import streamlit as st
import numpy as np

st.title("Your Demo")
# Your interactive code here
```

2. **Push to GitHub**

3. **Deploy to Streamlit Cloud**:
   - Go to https://share.streamlit.io
   - Connect GitHub repository
   - Select the app file
   - Get URL like `https://your-demo.streamlit.app`

4. **Link from portfolio**:
```markdown
[Live Demo →](https://your-demo.streamlit.app)
```

---

## Theme Options

### Current Theme
- Minimal, clean, no heavy CSS
- Single custom HTML layout
- Fast, lightweight

### Alternative Themes (if you want)

```yaml
# In _config.yml, uncomment one:
theme: jekyll-theme-minimal
# OR
theme: jekyll-theme-slate
# OR
theme: jekyll-theme-cayman
# OR
theme: jekyll-theme-tactile
```

Then use their built-in styles instead of custom HTML.

---

## Custom Domain (Optional)

### Buy a Domain
- GoDaddy, Namecheap, Google Domains, etc.
- ~$12/year for `.com`

### Connect to GitHub Pages
1. Go to repo Settings → Pages
2. Enter your domain: `yourname.com`
3. Follow DNS setup instructions
4. GitHub Pages handles the rest

---

## Performance & Best Practices

### Images
```bash
# Compress before committing
# Use ImageMagick or online tools
convert large_image.png -quality 85 optimized_image.png
```

### Writing
- Keep posts 2000-3000 words max
- Break into sections with clear headings
- Link between related posts
- Update monthly with new content

### SEO
- Add `description` to `_config.yml`
- Use descriptive post titles
- Include keywords naturally
- Sitemap auto-generates at `/sitemap.xml`

### Analytics (Optional)
Add Google Analytics:

```html
<!-- In _layouts/default.html, before </head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Site not updating after push | Wait 2 minutes, check GitHub Actions for build errors |
| Local site looks different | Run `JEKYLL_ENV=production bundle exec jekyll serve` |
| Styling broken | Clear cache (Ctrl+Shift+Del), then `rm -rf .jekyll-cache` |
| 404 on posts | Verify file is in `_blog/category/` folder and named correctly |
| Demo links broken | Check URL is live and accessible |

---

## Going Further

### Advanced Customization
- Add comments with Disqus
- Add newsletter signup
- Add project filters
- Add search functionality
- Dark mode toggle

### Content Ideas
- Weekly blog posts (consistency = growth)
- Project retrospectives
- "Things I learned this month"
- Technical explanations
- Career reflections

### Promotion
- Share on Twitter/LinkedIn
- Submit posts to Hacker News
- Contribute to Medium (cross-post)
- Mention projects in interviews

---

## Maintenance

**Weekly**:
- Write one blog post

**Monthly**:
- Update "Now" page
- Review analytics
- Fix any broken links

**Quarterly**:
- Add new project page
- Refresh about/experience sections
- Update resume if needed

---

Done! Your portfolio is live.

**Next step**: Write your first blog post or add your strongest project.

Questions? See the full [README.md](README.md) or [TEMPLATES.md](TEMPLATES.md).
