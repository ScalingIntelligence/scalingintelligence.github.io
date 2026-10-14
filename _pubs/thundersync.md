---
title: 'ThunderSync: Lossless Acceleration of Agentic Reinforcement Learning'
authors:
  - key: seilkang
    equal: true
    affiliation: Stanford University
  - key: hangookang
    equal: true
    affiliation: Stanford University
  - key: tarunsuresh
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
venue: preprint
year: 2026
date: 2026-10-05
has_pdf: true
doi: 10.48550/arXiv.2610.05935
bibtex_raw: |
  @misc{kang2026thundersyncrllosslessaccelerationagentic,
        title={ThunderSyncRL: Lossless Acceleration of Agentic Reinforcement Learning},
        author={Seil Kang and Hangoo Kang and Tarun Suresh and Youngeun Kim and Shreyas Pimpalgaonkar and Seong Jae Hwang and Azalia Mirhoseini},
        year={2026},
        eprint={2610.05935},
        archivePrefix={arXiv},
        primaryClass={cs.LG},
        url={https://arxiv.org/abs/2610.05935},
  }
tags:
  - machine learning
  - ml systems
  - generative ai
teaser: ThunderSync streams gradient computation as soon as each trajectory's inputs are fixed, training agentic models up to 1.9x faster than synchronous RL with identical GRPO and OPD updates and zero policy staleness.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2610.05935
    type: file-pdf
  - name: Blog
    url: https://seilk.github.io/thundersync/
    type: link
  - name: Codebase
    url: https://github.com/seilk/thundersync
    type: code
---
Language models are moving beyond generating answers to pursuing long-horizon goals in interactive environments. Post-training these agents requires long, heterogeneous trajectories, and synchronous systems leave learner engines idle until rollout and verification finish. To squeeze out these pipeline bubbles, asynchronous training overlaps rollout and learning across updates, but comes at the cost of policy staleness. We introduce ThunderSync, which starts gradient computation as soon as all required inputs are fixed, without policy staleness. For group relative policy optimization (GRPO), ThunderSync computes each trajectory's score gradient as soon as the reward for that trajectory arrives, without waiting for the group. For on-policy distillation (OPD), it computes gradients for each completed agentic turn's teacher-scored actions while tool calls run in the sandbox. We prove that gradient streaming produces the same GRPO and OPD updates as batch-synchronous training, without changing either objective. On SWE-bench Verified and Terminal Bench 4.0, we train models to the same performance up to 1.9x faster than synchronous training. With zero policy staleness, ThunderSync also outperforms asynchronous training at a fixed budget by up to 2.47 percentage points.
