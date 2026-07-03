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
{% assign pages = site.pages | sort: 'path' %}
{% for doc in pages %}
  {% assign name = doc.name | remove: '.md' | replace: '-',' ' | capitalize %}
  {% if doc.path != 'index.md' and doc.path != '_config.yml' and doc.path != '_layouts/default.html' and doc.name != 'README.md' %}
    <article class="card">
      <h3><a href="{{ doc.url | relative_url }}">{{ doc.title | default: name }}</a></h3>
      {% if doc.excerpt %}
        <p>{{ doc.excerpt | strip_html | truncate: 120 }}</p>
      {% endif %}
    </article>
  {% endif %}
{% endfor %}
</section>
