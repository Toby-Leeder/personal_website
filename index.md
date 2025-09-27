---
layout: splash
title: "Hi, I'm Toby"
excerpt: "Software engineering projects, notes, and résumé"
header:
  overlay_color: "#000"
  overlay_filter: 0.4
  overlay_image: /assets/images/project-demo.gif
  actions:
    - label: "View Projects"
      url: "/projects/"
    - label: "Download Résumé"
      url: "/assets/resume.pdf"
intro:
  - excerpt: "I build things across data, backend, and creative tech. Here are a few highlights."
feature_row:
  - image_path: /assets/images/cyph-hero.png
    alt: "CYPH"
    title: "Can You Picture Heaven"
    excerpt: "Creative game development."
    url: "/projects/cyph/"
    btn_label: "Read more"
    btn_class: "btn--primary"
  - image_path: /assets/images/leedingtomorrow-front.png
    alt: "LeedingTomorrow"
    title: "LeedingTomorrow"
    excerpt: "Professional website development for educator."
    url: "/projects/leedingtomorrow/"
    link: https://www.leedingtomorrow.com/
    btn_label: "Read more"
    btn_class: "btn--primary"
  - image_path: /assets/images/classroomjukebox.png
    alt: "Classroom Jukebox"
    title: "Classroom Jukebox"
    excerpt: "Collaborative queue for classroom music."
    url: "/projects/classroomjukebox/"
    btn_label: "Read more"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}
{% include feature_row %}
