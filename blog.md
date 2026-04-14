---
layout: default
title: Blog
---

# Blog

I write about quality over frequency, focusing on systems thinking, honesty in sharing failures, and clear communication for interested readers. Here are my thoughts on ML systems, production lessons, and creative explorations.

## Featured Writing

{% assign featured_ml = site.ml | where: "title", "Current ML Learning Journey" | first %}
{% assign featured_wmd = site.ml | where: "title", "Weapons of Math Destruction — Cathy O'Neil" | first %}

{% if featured_ml %}- **[{{ featured_ml.title }}]({{ featured_ml.url | relative_url }})** – My ongoing exploration of model compression, causal inference, and LLM fine-tuning
{% endif %}
{% if featured_wmd %}- **[{{ featured_wmd.title }}]({{ featured_wmd.url | relative_url }})** – Cathy O'Neil's critical examination of algorithmic bias and inequality
{% endif %}

## Categories

<details open>
  <summary><strong>Machine Learning & Systems ({{ site.ml | size }})</strong></summary>
  {% assign posts = site.ml | sort: 'date' | reverse %}
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
  <summary><strong>Mathematical Thinking ({{ site.math | size }})</strong></summary>
  {% assign posts = site.math | sort: 'date' | reverse %}
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
  <summary><strong>Writing & Creative Work ({{ site.creative | size }})</strong></summary>
  {% assign posts = site.creative | sort: 'date' | reverse %}
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