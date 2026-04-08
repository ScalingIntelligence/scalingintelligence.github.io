---
title: 'TRACE: Capability-Targeted Agentic Training'
authors:
  - key: hangookang
    equal: true
  - name: Tarun Suresh
    equal: true
    affiliation: Stanford University
  - key: jonsaadfalcon
  - key: azaliamirhoseini
venue: preprint
year: 2026
date: 2026-04-07
has_pdf: true
doi: 10.48550/arXiv.2604.05336
tags:
  - Agents
  - Reinforcement Learning
  - Self-improving AI
teaser: TRACE is an end-to-end system for environment-specific agent self-improvement that automatically identifies the specific capabilities the agent lacks in the environment and synthesizes targeted training environments to teach each missing capability.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2604.05336
    type: file-pdf
  - name: Codebase
    url: https://github.com/ScalingIntelligence/TRACE
    type: code
---
Large Language Models (LLMs) deployed in agentic environments must exercise multiple capabilities across different task instances, where a capability is performing one or more actions in a trajectory that are necessary for successfully solving a subset of tasks in the environment. Many existing approaches either rely on synthetic training data that is not targeted to the model's actual capability deficits in the target environment or train directly on the target environment, where the model needs to implicitly learn the capabilities across tasks. We introduce TRACE (Turning Recurrent Agent failures into Capability-targeted training Environments), an end-to-end system for environment-specific agent self-improvement. TRACE contrasts successful and failed trajectories to automatically identify lacking capabilities, synthesizes a targeted training environment for each that rewards whether the capability was exercised, and trains a LoRA adapter via RL on each synthetic environment, routing to the relevant adapter at inference. Empirically, TRACE generalizes across different environments, improving over the base agent by **+14.1 points** on &tau;<sup>2</sup>-Bench (customer service) and **+7 perfect scores** on ToolSandBox (tool use), outperforming the strongest baseline by **+7.4 points** and **+4 perfect scores**, respectively. Given the same number of rollouts, TRACE scales more efficiently than baselines, outperforming GRPO and GEPA by **+9.2** and **+7.4** points on &tau;<sup>2</sup>-Bench.
