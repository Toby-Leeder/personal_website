---
layout: default
title: "Toby Leeder"
excerpt: "Computer Science and Applied Math at UC Berkeley. Full-stack projects, writing, and résumé."
---

{% assign featured = site.projects | where: "featured", true | sort: "weight" %}

<div class="home">

  <section class="home__hero">
    <div>
      <p class="home__eyebrow">CS + Applied Math · UC Berkeley · Open to SWE internships</p>
      <h1 class="home__title">I build <u>software</u><br>end to end.</h1>
      <p class="home__lead">Full-stack and mobile apps, internal tools, and the occasional game. I like owning a feature from the schema to the UI and shipping it.</p>
      <div class="home__actions">
        <a class="btn btn--primary" href="/projects/">View projects</a>
        <a class="btn btn--inverse" href="/assets/resume.pdf">Résumé (PDF)</a>
        <span class="home__quick"><a href="https://github.com/Toby-Leeder">github</a> · <a href="https://www.linkedin.com/in/toby-leeder/">linkedin</a> · <a href="mailto:tobyleeder@berkeley.edu">email</a></span>
      </div>
    </div>
    <div class="home__photo">
      <img src="/assets/images/headshot.jpg" alt="Toby Leeder">
    </div>
  </section>

  <ul class="home__stack">
    <li class="label">WORKS WITH</li>
    {% for t in site.data.stack %}<li>{{ t }}</li>{% endfor %}
  </ul>

  <section class="home__section">
    <div class="home__section-head"><h2>Selected projects</h2><a href="/projects/">all projects →</a></div>
    <div class="cards">
      {% for p in featured %}
      <a class="card" href="{{ p.url | relative_url }}">
        <img src="{{ p.teaser | relative_url }}" alt="">
        <div>
          <h3>{{ p.title }}</h3>
          <p>{{ p.summary }}</p>
          {% if p.tech %}<ul class="tech">{% for t in p.tech limit: 4 %}<li>{{ t }}</li>{% endfor %}</ul>{% endif %}
        </div>
      </a>
      {% endfor %}
    </div>
  </section>

  <section class="home__section">
    <div class="home__two">
      <div>
        <div class="home__section-head"><h2>Writing</h2><a href="/blog/">blog →</a></div>
        <ul class="home__list">
          {% for post in site.posts limit: 4 %}
          <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m" }}</time></li>
          {% endfor %}
        </ul>
      </div>
      <div>
        <div class="home__section-head"><h2>Off the clock</h2><a href="/about/">about →</a></div>
        <div class="home__aside"><b>Tutoring CS 61A at UC Berkeley.</b> Directing a short film with the Business and Film Association, performed in The Prom in summer 2025, and I've been to 15 countries. The child-actor story is on the <a href="/about/">About page</a>.</div>
      </div>
    </div>
  </section>

</div>
