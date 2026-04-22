---
title: 'Scaling Verification Can Be More Effective than Scaling Policy Learning for Vision-Language-Action Alignment'
authors:
  - key: jackykwok
    equal: true
  - name: Xilun Zhang
    equal: true
    affiliation: Stanford University
  - name: Mengdi Xu
    affiliation: Stanford University
  - name: Yuejiang Liu
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
  - key: chelseafinn
    affiliation: Stanford University
  - key: marcopavone
    affiliation: Stanford, NVIDIA
venue: preprint
year: 2026
date: 2026-02-12
has_pdf: true
doi: 10.48550/arXiv.2602.12281
tags:
  - machine learning
  - robotics
teaser: CoVer-VLA introduces a contrastive verifier for vision-language-action alignment, demonstrating that scaling test-time verification yields larger gains than scaling policy pre-training, with up to 45% improvement in real-world robotic experiments.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2602.12281
    type: file-pdf
  - name: Codebase
    url: https://github.com/cover-vla/cover-vla
    type: code
  - name: Models
    url: https://huggingface.co/cover-vla
    type: database
  - name: Demos
    url: https://cover-vla.github.io/
    type: link
---
The long-standing vision of general-purpose robots hinges on their ability to understand and act upon natural language instructions. Vision-Language-Action (VLA) models have made remarkable progress toward this goal, yet their generated actions can still misalign with the given instructions. In this paper, we investigate test-time verification as a means to shrink the "intention-action gap." We first characterize the test-time scaling laws for embodied instruction following and demonstrate that jointly scaling the number of rephrased instructions and generated actions greatly increases test-time sample diversity, often recovering correct actions more efficiently than scaling each dimension independently. To capitalize on these scaling laws, we present CoVer, a contrastive verifier for vision-language-action alignment, and show that our architecture scales gracefully with additional computational resources and data. We then introduce CoVer-VLA, a hierarchical test-time verification pipeline using the trained verifier. At deployment, our framework precomputes a diverse set of rephrased instructions from a Vision-Language-Model (VLM), repeatedly generates action candidates for each instruction, and then uses the verifier to select the optimal high-level prompt and low-level action chunks. Compared to scaling policy pre-training on the same data, our verification approach yields 22% gains in-distribution and 13% out-of-distribution on the SIMPLER benchmark, with a further 45% improvement in real-world experiments. On the PolaRiS benchmark, CoVer-VLA achieves 14% gains in task progress and 9% in success rate.
