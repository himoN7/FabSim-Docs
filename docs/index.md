---
layout: default
title: FabSim Documentation
description: Fluent-style documentation hub for FabSim Docs
---
<section class="page-hero">
  <p class="page-tag">Fluent Design</p>
  <h1>Welcome to FabSim Docs</h1>
  <p>Browse the documentation with a clean, modern Fluent-inspired theme built for GitHub Pages.</p>
</section>

<section class="card-grid">
{% assign docs_pages = site.pages | sort: 'path' %}
{% for doc in docs_pages %}
  {% if doc.name != 'index.md' and doc.name != 'README.md' %}
    {% assign name = doc.name | remove: '.md' | replace: '-',' ' | capitalize %}
    <article class="card">
      <h3><a href="{{ doc.url | relative_url }}">{{ doc.title | default: name }}</a></h3>
      {% if doc.excerpt %}
        <p>{{ doc.excerpt | strip_html | truncate: 120 }}</p>
      {% endif %}
    </article>
  {% endif %}
{% endfor %}
</section>
