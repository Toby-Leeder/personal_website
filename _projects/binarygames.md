---
featured: false
weight: 4
title: "Binary Games"
date: 2024-05-01
summary: "A virtual escape room of computer science puzzles with accounts and a speedrun leaderboard, built for AP Computer Science Principles."
excerpt: "A virtual escape room of computer science puzzles with accounts and a speedrun leaderboard, built for AP Computer Science Principles."
teaser: /assets/images/binarygames.jpg
tech: [JavaScript, HTML, CSS, Python, Flask, Docker]
role: "Full-stack developer"
layout: single
permalink: /projects/binarygames/
links:
  - label: "Live Site"
    url: "https://toby-leeder.github.io/binarygames-frontend"
  - label: "Frontend Code"
    url: "https://github.com/Toby-Leeder/binarygames-frontend"
  - label: "Backend Code"
    url: "https://github.com/Toby-Leeder/binarygames-backend"
header:
  overlay_color: "#000"
  overlay_filter: 0.5
  overlay_image: /assets/images/binarygames.jpg
---

## What it is

Binary Games is a virtual escape room built by Team JCK for AP Computer Science Principles. Players create an account, then work through a series of computer science themed puzzles to escape the room. A leaderboard tracks speedrun times, and each game can also be played on its own.

## The games

- **Binary Racer:** a reaction game around reading binary quickly.
- **Pipes:** route the flow by picking the right operators.
- **Logic and Logic Gates:** evaluate boolean expressions and gate diagrams.
- **RGB Guesser:** match a color to its hex and RGB values.
- **Bomb Defusal:** decode Base64 before the timer runs out.

## How it's built

- **Frontend:** static HTML, CSS, and JavaScript, hosted on GitHub Pages.
- **Backend:** a Python Flask API with user accounts and leaderboard storage, containerized with Docker.
