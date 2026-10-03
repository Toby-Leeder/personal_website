---
layout: single
title: "Projects"
permalink: /projects/
author_profile: false
classes: wide
excerpt: "Things I've built, with code and write-ups."
---

{% assign projects = site.projects | sort: "weight" %}

<div class="cards">
{% for p in projects %}
  <a class="card" href="{{ p.url | relative_url }}">
    {% if p.teaser %}<img src="{{ p.teaser | relative_url }}" alt="">{% else %}<div class="card__placeholder">{{ p.date | date: "%Y" }}</div>{% endif %}
    <div>
      <h3>{{ p.title }}</h3>
      <p>{{ p.summary }}</p>
      {% if p.tech %}<ul class="tech">{% for t in p.tech limit: 5 %}<li>{{ t }}</li>{% endfor %}</ul>{% endif %}
    </div>
  </a>
{% endfor %}
</div>
