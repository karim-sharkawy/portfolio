---
layout: default
title: Blog
---

# Blog

I write about quality over frequency, focusing on systems thinking, honesty in sharing failures, and clear communication for interested readers. Here are my thoughts on ML systems, production lessons, and creative explorations.

## Featured Writing

- **[Current ML Learning Journey]({{ '/blog/ml/current-ml-learning/' | relative_url }})** – My ongoing exploration of model compression, causal inference, and LLM fine-tuning
- **[Weapons of Math Destruction]({{ '/blog/ml/weapons-of-math-destruction/' | relative_url }})** – Cathy O'Neil's critical examination of algorithmic bias and inequality

## Categories

<details open>
  <summary><strong>Machine Learning & Systems ({{ site.categories.ML | size }})</strong></summary>
  {% assign posts = site.categories.ML | sort: 'date' | reverse %}
  {% if posts.size > 0 %}
  <ul style="list-style: none; padding-left: 0;">
    {% for post in posts %}
    <li style="margin-bottom: 1.1rem;">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a><br/>
      <small>{{ post.excerpt | strip_html | truncate: 120 }}</small>
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p><em>No posts yet.</em></p>
  {% endif %}
</details>

<details>
  <summary><strong>Mathematical Thinking ({{ site.categories.Math | size }})</strong></summary>
  {% assign posts = site.categories.Math | sort: 'date' | reverse %}
  {% if posts.size > 0 %}
  <ul style="list-style: none; padding-left: 0;">
    {% for post in posts %}
    <li style="margin-bottom: 1.1rem;">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a><br/>
      <small>{{ post.excerpt | strip_html | truncate: 120 }}</small>
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p><em>No posts yet.</em></p>
  {% endif %}
</details>

<details>
  <summary><strong>Writing & Creative Work ({{ site.categories.Creative | size }})</strong></summary>
  {% assign posts = site.categories.Creative | sort: 'date' | reverse %}
  {% if posts.size > 0 %}
  <ul style="list-style: none; padding-left: 0;">
    {% for post in posts %}
    <li style="margin-bottom: 1.1rem;">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a><br/>
      <small>{{ post.excerpt | strip_html | truncate: 120 }}</small>
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p><em>No posts yet.</em></p>
  {% endif %}
</details>