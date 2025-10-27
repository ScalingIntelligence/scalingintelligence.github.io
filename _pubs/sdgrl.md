---
title: 'Synthetic Data Generation & Multi-Step RL for Reasoning & Tool Use'
authors:
  - name: Anna Goldie
    affiliation: Stanford University
    equal: true
  - key: azaliamirhoseini
    equal: true
  - name: Hao Zhou
    affiliation: Google DeepMind
  - name: Irene Cai
    affiliation: Google DeepMind
  - name: Christopher D. Manning
    affiliation: Stanford University
venue: colm
venue_txt:
year: 2025
date: 2025-04-28
month: April
day: 28
has_pdf: true
doi: 10.48550/arXiv.2504.04736
tags:
  - model
  - generative ai
teaser: SWiRL is a framework that improves language model reasoning through synthetic data generation and step-wise reinforcement learning, enabling models to outperform larger proprietary models across diverse reasoning tasks while demonstrating strong generalization capabilities.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2504.04736
    type: file-pdf
---
Reinforcement learning has been shown to improve the performance of large language models. However, traditional approaches like RLHF or RLAIF treat the problem as single-step. As focus shifts toward more complex reasoning and agentic tasks, language models must take multiple steps of text generation, reasoning and environment interaction before generating a solution. We propose a synthetic data generation and RL methodology targeting multi-step optimization scenarios. This approach, called Step-Wise Reinforcement Learning (SWiRL), iteratively generates multi-step reasoning and tool use data, and then learns from that data. It employs a simple step-wise decomposition that breaks each multi-step trajectory into multiple sub-trajectories corresponding to each action by the original model. It then applies synthetic data filtering and RL optimization on these sub-trajectories. We evaluated SWiRL on a number of multi-step tool use, question answering, and mathematical reasoning tasks. Our experiments show that SWiRL outperforms baseline approaches by 21.5%, 12.3%, 14.8%, 11.1%, and 15.3% in relative accuracy on GSM8K, HotPotQA, CofCA, MuSiQue, and BeerQA, respectively. Excitingly, the approach exhibits generalization across tasks: for example, training only on HotPotQA (text question-answering) improves zero-shot performance on GSM8K (a math dataset) by a relative 16.9%.
