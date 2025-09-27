---
title: "Classroom Jukebox"
date: 2025-09-19
summary: "Collaborative music queue for classrooms — combined frontend and backend contributions."
tech: [JavaScript, React, Node.js, Express, REST, OAuth, GitHub]
role: "Contributor (frontend + backend)"
layout: single
links:
  - label: "Frontend Repository"
    url: "https://github.com/aidenhuynh/cj_frontend"
  - label: "Backend Repository"
    url: "https://github.com/aidenhuynh/cj_backend"

  # - label: "Live Demo"
  #   url: "https://example.com"   # optional if there’s a hosted instance
header:
  overlay_color: "#000"
  overlay_filter: 0.3
  overlay_image: /assets/images/classroomjukebox.png   # ← add a GIF/screenshot
---

## Problem
Teachers often play music in class, but it's hard for students to provide input on what songs to play! 

## Approach
- **Auth/integration:** OAuth / API hooks for direct Spotify Connections
- **Frontend:** React components for queue view, searching, moderation panel.
- **Backend:** REST endpoints for track search, enqueue, dequeue, and admin actions.

## Highlights
- Implemented/modified components for song controls (play, pause, skip) and queue updates.
- Added/updated API routes and request validation.
- Wrote docs and usage notes so anyone can run locally.

## Results
- Demonstrates collaborative product design and FE/BE coordination.
- Shows practical handling of rate limits, API errors, and UX for contention.
