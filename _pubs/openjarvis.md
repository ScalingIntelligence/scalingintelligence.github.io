---
title: "OpenJarvis: Personal AI, On Personal Devices"
authors:
  - key: jonsaadfalcon
    equal: true
    affiliation: Stanford
  - name: Avanika Narayan
    equal: true
    affiliation: Stanford University
  - name: John Hennessy
    affiliation: Stanford University
  - name: Christopher Ré
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
venue: preprint
year: 2026
date: 2026-03-12
has_pdf: false
doi: 10.48550/arXiv.2605.17172
tags:
  - machine learning
  - systems
  - personal AI
  - on-device inference
teaser: OpenJarvis is an open-source framework for personal AI agents that runs entirely on-device, providing shared primitives for on-device agents, efficiency-aware evaluations, and a learning loop that improves models using local trace data.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2605.17172
    type: file-pdf
  - name: Codebase
    url: https://github.com/open-jarvis/OpenJarvis
    type: code
  - name: Docs
    url: https://open-jarvis.github.io/OpenJarvis/
    type: link
  - name: Blog post
    url: https://scalingintelligence.stanford.edu/blogs/openjarvis/
    type: link
---
OpenJarvis is an opinionated, open-source framework for personal AI agents that runs entirely on-device. It provides shared primitives for building on-device agents, efficiency-aware evaluations that treat latency, energy, memory footprint, and dollar cost as first-class constraints, and a closed-loop optimization pipeline that improves models using local trace data. The goal is to make it possible to build personal AI agents that run locally by default, calling the cloud only when truly necessary — serving as both a research platform and production foundation for local AI.
