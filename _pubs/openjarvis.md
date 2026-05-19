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
has_pdf: true
doi: 10.48550/arXiv.2605.17172
tags:
  - machine learning
  - systems
  - personal AI
  - on-device inference
teaser: OpenJarvis decomposes the personal AI stack into five typed primitives (Intelligence, Engine, Agents, Tools & Memory, Learning) and uses LLM-guided spec search to close the local-cloud accuracy gap to within 3.2 pp on average while reducing marginal API cost ~800x and end-to-end latency 4x — running entirely on-device at inference time.
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
Personal AI stacks, like OpenClaw and Hermes Agent, are becoming central to daily work, yet they route nearly every query (often over sensitive local data) to cloud-hosted frontier models. Replacing frontier models with local models inside existing stacks does not work: swapping Claude Opus 4.6 for Qwen3.5-9B drops accuracy by 25-39 pp across personal AI tasks like PinchBench and GAIA. Existing stacks bundle agentic prompts, tool descriptions, memory configuration, and runtime settings around a specific cloud model. Only the prompts can be tuned, and state-of-the-art prompt optimizers close just 5 pp of the local-cloud gap on their own. This motivates a decomposed personal AI stack: one that exposes individual primitives which can be optimized individually or jointly to close the local-cloud gap. We present OpenJarvis, an architecture that represents a personal AI system as a typed spec over five primitives: Intelligence, Engine, Agents, Tools & Memory, and Learning. Each primitive is an independently editable field, making the stack end-to-end optimizable and measurable against accuracy, cost, and latency. Towards closing the local-cloud gap without surrendering local-model properties, OpenJarvis introduces LLM-guided spec search, a local-cloud collaboration in which frontier cloud models propose edits across the spec at search time, only non-regressing edits are accepted, and the resulting spec runs entirely on-device at inference time. With LLM-guided spec search, on-device specs match or exceed cloud accuracy on 4 of 8 benchmarks and land within 3.2 pp of the best cloud baseline on average. They also reduce marginal API cost by ~800x and end-to-end latency by 4x.
