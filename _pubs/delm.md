---
title: 'Decentralized Multi-Agent Systems with Shared Context'
authors:
  - key: yuzhenmao
    affiliation: Stanford University
  - name: Jerry Gu
    affiliation: Stanford University
  - name: Aadi Chauhan
    affiliation: Stanford University
  - name: Qizheng Zhang
    affiliation: Stanford University
  - key: hangookang
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
venue: preprint
year: 2026
date: 2026-06-09
has_pdf: true
doi: 10.48550/arXiv.2606.10662
tags:
  - Agents
  - generative ai
teaser: DeLM replaces the main agent with a shared context and a task queue, letting parallel agents asynchronously claim tasks, publish findings, and build on one another's progress, making long-horizon agents both more accurate and faster.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2606.10662
    type: file-pdf
  - name: Code
    url: https://github.com/yuzhenmao/DeLM
    type: code
  - name: Project Page
    url: https://yuzhenmao.github.io/DeLM/
    type: link
---
Multi-agent systems (MAS) can scale large language model agents on long-horizon tasks by running them in parallel, yet existing designs waste much of this parallelism in *bubbles*: agent time spent waiting on others or redoing a peer's work. These bubbles stem from how agents communicate. Independent agents share nothing and rediscover what their peers have already found; peer-communicating agents wait at synchronous rounds; and under centralized orchestration, the main agent blocks on its sub-agents while progress is relayed. We propose Decentralized Language Models (DeLM), a MAS framework on top of existing agent harnesses that squeezes out these bubbles by replacing the main agent with a shared context and a task queue. Agents asynchronously claim tasks, publish findings as soon as they are available, and build on or correct one another's progress, with every peer's status visible to all. On long-horizon tasks from Terminal-Bench 4.0 and DeepSWE v1.1, and on SWE-bench Verified, DeLM is both more accurate and faster than Codex, Claude Code, their native subagents, and AOrchestra in every setting, improving accuracy by up to 17.5 points over the strongest baseline and running up to 2.49x faster than the harness it builds on. On ProgramBench, where agents rebuild programs from scratch, DeLM makes faster progress than Claude Code and finishes a 120-minute budget up to 19.9 points higher in test pass rate. The code is available on our [project website](https://yuzhenmao.github.io/DeLM/).
