---
layout: default
title: Welcome to FabSim Docs
description: Browse the documentation with a clean, modern Fluent-inspired theme built for GitHub Pages.
permalink: /
---
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
