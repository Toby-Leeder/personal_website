---
title: "BinaryGames"
date: 2025-09-19
summary: "A paired frontend + backend project demonstrating full‑stack architecture, auth, and deployment workflow."
tech: [JavaScript, React, Node.js, Express, REST, JSON, GitHub Actions]
role: "Full‑stack developer"
layout: single
permalink: /projects/binarygames/
links:
  - label: "Frontend Code"
    url: "https://github.com/Toby-Leeder/binarygames-frontend"   # ← update if different
  - label: "Backend Code"
    url: "https://github.com/Toby-Leeder/binarygames-backend"    # ← update if different
  # - label: "Live Demo"
  #   url: "https://example.com"                                  # optional
header:
  overlay_color: "#000"
  overlay_filter: 0.3
  overlay_image: /assets/images/binarygames-hero.gif             # ← add a GIF/screenshot
---

## Problem
Briefly describe the user problem or motivation. For example: a learning tool with interactive challenges that visualize binary/bitwise operations.

## Approach
- **Frontend:** React SPA, router, state management, API client, reusable components.
- **Backend:** Node/Express REST API, routes, data validation, error handling.
- **Integration:** Auth/session (if applicable), request/response contracts, CORS, and CI.

## Architecture (high‑level)
[React UI] ⇄ [API Client] ⇄ (HTTPS) ⇄ [Express Routes] → [Controller] → [Model/Storage]


## Results
- Key outcomes (latency targets, features delivered, tests covered).
- Screenshots or a short GIF of core flows (challenge view, scoreboard, etc.).

## Responsibilities
- Implemented X feature end‑to‑end (component ↔ API).
- Wrote unit/integration tests for Y modules.
- Set up CI/CD (GitHub Actions) and deployment scripts (if applicable).

## Next Steps
Add persistence (Postgres/Mongo), OAuth login, or real‑time updates with websockets.

> **Tip:** Drop images at `/assets/images/` and update the `overlay_image` above.
