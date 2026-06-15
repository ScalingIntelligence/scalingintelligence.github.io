---
title: 'Decentralized Multi-Agent Systems with Shared Context'
authors:
  - key: yuzhenmao
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
teaser: DeLM replaces centralized orchestration with a shared, verified context and a task queue, letting parallel agents asynchronously accumulate reusable progress for scalable, reliable, and cost-efficient test-time reasoning.
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
Multi-agent systems (MAS) can scale large language model reasoning at test time by decomposing complex problems into parallel subtasks. However, most existing MAS rely on centralized orchestration, where a main agent assigns work, collects outputs, and merges results. As the number of subtasks grows, this controller becomes a communication and integration bottleneck. We propose Decentralized Language Models (DeLM), a MAS framework that decentralizes coordination through parallel agents, a shared verified context, and a task queue. Agents asynchronously claim subtasks, read accumulated progress, perform local reasoning, and write back compact verified updates. The shared context acts as a common communication substrate, enabling agents to build on one another's verified progress without routing every update through a central controller. Empirically, DeLM improves both software-engineering test-time scaling and long-context reasoning. On SWE-bench Verified, DeLM achieves the best performance across Avg.@1, Pass@2, and Pass@4, with gains of up to 10.5 percentage points over the strongest baseline, while reducing cost per task by roughly 50%. On LongBench-v2 Multi-Doc QA, DeLM achieves the highest average accuracy across four frontier model families, improving over the strongest baseline by up to 5.7 percentage points. The code is available on our [project website](https://yuzhenmao.github.io/DeLM/).
