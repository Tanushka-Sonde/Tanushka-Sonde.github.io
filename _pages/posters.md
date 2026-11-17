---
layout: page
title: Posters
permalink: /posters/
description: Posters and presentation slides from my projects.
nav: true
nav_order: 3
---

{% assign shown = 0 %}

<div class="row row-cols-1 row-cols-md-2">
{% for p in site.data.posters %}
  {% assign pdf_path = "/" | append: p.pdf %}
  {% assign pdf_file = site.static_files | where: "path", pdf_path | first %}
  {% if pdf_file %}
    {% assign shown = shown | plus: 1 %}
    {% assign thumb_path = "/" | append: p.thumbnail %}
    {% assign thumb_file = site.static_files | where: "path", thumb_path | first %}
    <div class="col mb-4">
      <div class="card h-100 hoverable">
        {% if thumb_file %}
          <a href="{{ p.pdf | relative_url }}" target="_blank" rel="noopener">
            <img src="{{ p.thumbnail | relative_url }}" class="card-img-top" alt="{{ p.title }}" style="object-fit: cover; max-height: 320px;">
          </a>
        {% endif %}
        <div class="card-body">
          <p class="post-meta mb-1">{{ p.kind | default: "Poster" }}{% if p.date %} &middot; {{ p.date }}{% endif %}</p>
          <h3 class="card-title" style="font-size: 1.15rem;">{{ p.title }}</h3>
          {% if p.venue %}<p class="card-text">{{ p.venue }}</p>{% endif %}
          <a class="btn btn-sm z-depth-0" role="button" href="{{ p.pdf | relative_url }}" target="_blank" rel="noopener"><i class="fa-solid fa-file-pdf"></i> PDF</a>
          {% if p.project %}
            <a class="btn btn-sm z-depth-0" role="button" href="{{ '/projects/' | append: p.project | append: '/' | relative_url }}"><i class="fa-solid fa-diagram-project"></i> Project page</a>
          {% endif %}
          {% if p.blog %}
            <a class="btn btn-sm z-depth-0" role="button" href="{{ p.blog | relative_url }}"><i class="fa-solid fa-pen"></i> Blog post</a>
          {% endif %}
        </div>
      </div>
    </div>
  {% endif %}
{% endfor %}
</div>

{% if shown == 0 %}

<p>Posters and slides will appear here soon.</p>
{% endif %}
