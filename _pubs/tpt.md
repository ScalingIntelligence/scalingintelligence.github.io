---
title: 'Think, Prune, Train, Improve: Scaling Reasoning Without Scaling Models'
authors:
  - key: caiacostello
  - key: simonguo
  - name: Anna Goldie
    affiliation: Stanford University
  - key: azaliamirhoseini
venue: lad
venue_txt: SSI-FM @ICLR 2025
year: 2025
date: 2025-04-25
month: April
day: 25
has_pdf: true
doi: 10.48550/arXiv.2504.18116
tags:
  - machine learning
  - model
  - generative ai
teaser: Think, Prune, Train (TPT) is a scalable framework that enables smaller language models to achieve performance rivaling larger ones through iterative self-improvement on their own reasoning traces, with experimental results showing models like Gemma-2B and LLaMA-70B-Instruct surpassing GPT-4o on reasoning tasks.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2504.18116
    type: file-pdf
---
Large language models (LLMs) have demonstrated strong capabilities in programming and mathematical reasoning tasks, but are constrained by limited high-quality training data. Synthetic data can be leveraged to enhance fine-tuning outcomes, but several factors influence this process, including model size, synthetic data volume, pruning strategy, and number of fine-tuning rounds. We explore these axes and investigate which conditions enable model self-improvement. We introduce the Think, Prune, Train process, a scalable framework that iteratively fine-tunes models on their own reasoning traces, using ground-truth pruning to ensure high-quality training data. This approach yields improved performance: on GSM8K, Gemma2-2B achieves a Pass@1 of 57.6% (from 41.9%), Gemma2-9B reaches 82%, matching LLaMA-3.1-70B, and LLaMA-3.1-70B attains 91%, even surpassing GPT-4o, demonstrating the effectiveness of self-generated reasoning and systematic data selection for improving LLM capabilities.
