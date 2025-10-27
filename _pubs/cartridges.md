---
title: 'Cartridges: Lightweight and general-purpose long context representations via self-study'
authors:
  - name: Sabri Eyuboglu
    equal: true
    affiliation: Stanford
  - key: ryanehrlich
    equal: true
    affiliation: Stanford
  - name: Simran Arora
    equal: true
    affiliation: Stanford, Caltech
  - name: Neel Guha
    affiliation: Stanford
  - name: Dylan Zinsley
    affiliation: University of Buffalo
  - name: Emily Liu
    affiliation: Stanford
  - name: Will Tennien
    affiliation: Stanford
  - name: Atri Rudra
    affiliation: University of Buffalo
  - name: James Zou
    affiliation: Stanford
  - key: azaliamirhoseini
    affiliation: Stanford
  - name: Christopher Ré
    affiliation: Stanford
venue: preprint
venue_txt: ES-FoMo (Oral) III @ICML 2025
year: 2025
date: 2025-06-06
has_pdf: true
doi: 10.48550/arXiv.2506.06266
tags:
  - machine learning
  - generative ai
teaser: With Cartridges, scaling test time compute can >25x decode throughput
materials:
  - name: Paper
    url: https://arxiv.org/abs/2506.06266
    type: file-pdf
  - name: Codebase
    url: https://github.com/HazyResearch/cartridges
    type: code
  - name: Blog post
    url: https://hazyresearch.stanford.edu/blog/2025-06-08-cartridges
    type: link
---
Large language models are often used to answer queries grounded in large text corpora (e.g. codebases, legal documents, or chat histories) by placing the entire corpus in the context window and leveraging in-context learning (ICL).
Although current models support contexts of 100K-1M tokens, this setup is costly to serve because the memory consumption of the KV cache scales with input length.
We explore an alternative: training a smaller KV cache offline on each corpus.
At inference time, we load this trained KV cache, which we call a Cartridge, and decode a response.
Critically, the cost of training a Cartridge can be amortized across all the queries referencing the same corpus. However, we find that the naive approach of training the Cartridge with next-token prediction on the corpus is not competitive with ICL. Instead, we propose self-study, a training recipe in which we generate synthetic conversations about the corpus and train the Cartridge with a context-distillation objective. We find that Cartridges trained with self-study replicate the functionality of ICL, while being significantly cheaper to serve. On challenging long-context benchmarks, Cartridges trained with self-study match ICL performance while using 38.6x less memory and enabling 26.4x higher throughput. Self-study also extends the model's effective context length (e.g. from 128k to 484k tokens on MTOB) and surprisingly, leads to Cartridges that can be composed at inference time without retraining.
