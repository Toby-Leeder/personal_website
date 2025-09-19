---
title: "Classroom Jukebox"
date: 2025-09-19
summary: "Collaborative music queue for classrooms — combined frontend and backend contributions."
tech: [JavaScript, React, Node.js, Express, REST, OAuth, GitHub]
role: "Contributor (frontend + backend)"
links:
  - label: "Repository"
    url: "https://github.com/aidenhuynh/classroom-jukebox"
  # - label: "Live Demo"
  #   url: "https://example.com"   # optional if there’s a hosted instance
header:
  overlay_color: "#000"
  overlay_filter: 0.3
  overlay_image: /assets/images/classroom-jukebox-hero.gif   # ← add a GIF/screenshot
---

## Problem
Students want a fair, moderated way to suggest and vote on songs in shared spaces without chaos or monopolization.

## Approach
- **Queue logic:** submit → dedupe → vote → play; teacher‑moderated controls.
- **Auth/integration:** OAuth / API hooks (e.g., Spotify/YouTube if used).
- **Frontend:** React components for queue view, voting, moderation panel.
- **Backend:** REST endpoints for track search, enqueue, vote, dequeue, and admin actions.

## Highlights
- Implemented/modified components for vote interactions and queue updates.
- Added/updated API routes and request validation.
- Wrote docs and usage notes so anyone can run locally.

## Results
- Demonstrates collaborative product design and FE/BE coordination.
- Shows practical handling of rate limits, API errors, and UX for contention.

## Local Setup
```bash
# repo: https://github.com/aidenhuynh/classroom-jukebox
# (sample commands—adjust to actual README)
npm install
npm run dev
# backend:
cd server && npm install && npm run dev
