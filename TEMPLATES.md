# Portfolio Templates & Best Practices

Quick reference for creating content in this portfolio.

---

## Blog Post Template (ML & Math)

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
category: "ML & Math"
---

## Intro Paragraph

Start with the big question or problem.

## Core Concept

Break it down into digestible sections.

### Header 3 for subsections

Use code blocks for examples:

\`\`\`python
# Code example
import numpy as np
\`\`\`

### Mathematics

Use inline math: $E = mc^2$

Or block math:

$$\frac{\partial L}{\partial w} = -\frac{1}{m} \sum_{i=1}^{m} (y^{(i)} - \hat{y}^{(i)}) x^{(i)}$$

## Implementation

Show practical code or examples.

## Key Takeaways

- Bullet point 1
- Bullet point 2
- Bullet point 3

## Further Reading

- [Link to resource](url)
- [Another resource](url)
```

---

## Blog Post Template (Build Logs)

```markdown
---
layout: post
title: "Project Title: Lesson Learned"
date: YYYY-MM-DD
category: "Build Logs"
---

## The Challenge

What problem needed solving?

## What We Did

Your approach and solution.

## Lessons Learned

- What worked
- What failed
- How we fixed it
- Why it matters for production

## Code/Architecture Example

Show diagrams or code:

\`\`\`
┌─────────┐
│ Component 1 │
└─────────┘
    │
    ▼
┌─────────┐
│ Component 2 │
└─────────┘
\`\`\`

## Impact

Metrics, outcomes, real-world results.

## What's Next

Where this leads or future improvements.
```

---

## Project Template

```markdown
---
layout: project
title: "Project Title"
date: YYYY-MM-DD

problem_statement: |
  What was the problem?
  Why did it matter?

tech_stack:
  - Technology 1
  - Technology 2
  - Technology 3

results_metrics:
  - "**Metric 1**: X% improvement"
  - "**Metric 2**: Y reduction"
  - "**Metric 3**: Z impact"

demo_url: "https://link-to-streamlit-app.streamlit.app"
demo_description: "What users can do with the demo"

code_repo: "https://github.com/your-username/repo"

resources:
  - title: "Resource Title"
    url: "https://resource-link.com"
  - title: "Blog Post Related"
    url: "/blog/ml/post-slug"

lessons_list:
  - "Lesson 1: Explanation"
  - "Lesson 2: Explanation"
  - "Lesson 3: Explanation"
---

## Your Approach

Detailed explanation of how you solved it.

### Subsection 1

Details...

### Subsection 2

Details...

## Results

Specific metrics and outcomes.

## Lessons Learned

Deeper explanation of learnings.

---
```

---

## Writing Tips for ML Portfolio

### 1. Case Study Format > Resume Bullets

❌ Bad:
```
Built recommendation system. Increased engagement by 20%.
Used collaborative filtering and matrix factorization.
```

✅ Good:
```
## Recommendation System

**Problem**: Personalization at scale. 5M+ users, billions of items.

**Approach**: Hybrid system combining collaborative filtering (~70% weight)
for scalability + content-based filtering (~30%) for diversity.

**Results**: 22% engagement increase, 15% click-through rate boost.

**Key insight**: The ensemble approach outperformed either alone.
Diversity is underrated in recommendations.
```

### 2. Show, Don't Tell

❌ Bad:
```
We optimized the model significantly.
```

✅ Good:
```
Reduced inference latency from 320ms to 128ms (60% faster).
Model size shrunk from 440MB to 268MB (40% smaller).
```

### 3. Include Failure Stories

People respect honesty. Failures are learning opportunities.

✅ Good:
```
## What Didn't Work

Initially, we tried using attention mechanisms for sequence modeling.
The added complexity didn't improve accuracy over LSTM + XGBoost
and made debugging harder. Lesson: simpler models win when
accuracy is equal.
```

### 4. Explain the "Why"

Don't just describe what you did. Explain your reasoning.

❌ Bad:
```
We used DistilBERT instead of BERT.
```

✅ Good:
```
We chose DistilBERT because:
1. **40% smaller** - Cheaper to serve at scale
2. **60% faster** - Meets our <200ms latency SLA
3. **Only 1% accuracy loss** - Tradeoff is worth it

The lesson: simpler, optimized models often beat heavy architectures
in production if accuracy difference is small.
```

### 5. Use Visuals (Sparingly)

Include:
- Diagrams of architecture
- ASCII diagrams of data flow
- Screenshot of live demo
- GIF of app in action

Avoid: Heavy animations, auto-playing videos, complex infographics

### 6. Connect Projects & Posts

Link related content:
- Project → Blog post explaining algorithm
- Blog post → Related project code
- Experience → Projects that resulted

```markdown
**See also**: [Understanding Attention](/blog/ml/attention) for the
mathematical foundations of this approach.
```

---

## Project Metrics to Include

### ML-Specific
- Accuracy, precision, recall, F1, AUROC
- MAE, RMSE, MAPE (regression)
- Speed improvements (latency, throughput)
- Cost reductions (infrastructure, compute)

### Business Impact
- Engagement/conversion improvements
- Cost saved
- Users impacted
- Scale (requests/sec, data volume)

### System Metrics
- Uptime/reliability
- Query latency (p50, p99)
- Deployment time
- Team velocity improvement

---

## SEO for Blog Posts

Each post should have:
1. **Keyword-rich title**: "Understanding Attention Mechanisms: From Theory to Production"
2. **Meta description**: "A deep dive into how attention works, with math and code examples"
3. **Good heading structure**: h1 → h2 → h3 (proper hierarchy)
4. **Links**: Internal (to other posts/projects) + External (to resources)
5. **Keywords naturally in**: Title, first paragraph, headings, closing

---

## Things to Avoid

- Jargon without explanation
- Massive walls of text (break into sections)
- Code without context (explain what/why)
- Outdated tool versions (cite date)
- Self-congratulatory tone (focus on lessons)
- "Coming soon" pages (complete articles only)

---

## Maintenance Checklist

- ✅ Update "Now" page monthly
- ✅ Review blog posts quarterly (fix broken links)
- ✅ Update resume when roles change
- ✅ Add new project every 3 months
- ✅ Check Google Analytics for popular content
- ✅ Fix broken external links (quarterly)

---

Done! Now go write something impressive.
