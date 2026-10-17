---
title: 'Next-Concept Modeling: Fast, Parallel Coarse-to-Fine Text Generation'
authors:
  - key: hermannkumbong
    equal: true
    affiliation: Stanford University
  - key: ishankhare
    name: Ishan S. Khare
    equal: true
  - name: Iddah Mlauzi
    url: https://www.linkedin.com/in/iddah-mlauzi/
    affiliation: Stanford University
  - name: Andrew Woen
    url: https://www.linkedin.com/in/andrew-woen-539822195/
    affiliation: Stanford University
  - name: Charles Ding
    url: https://www.linkedin.com/in/charles-dingg/
    affiliation: Stanford University
  - name: E. Kelly Buchanan
    url: https://ekbuchanan.com/
    affiliation: Stanford University
  - name: Simran Arora
    url: https://arorasimran.com/
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
  - name: Christopher Ré
    url: https://cs.stanford.edu/~chrismre/
    affiliation: Stanford University
venue: preprint
year: 2026
date: 2026-10-12
has_pdf: false
doi: 10.48550/arXiv.2610.14162
tags:
  - machine learning
  - natural language processing
  - generative ai
materials:
  - name: Paper
    url: https://arxiv.org/abs/2610.14162
    type: file-pdf
  - name: Codebase
    url: https://github.com/HazyResearch/ncm
    type: code
  - name: Blog post
    url: /blogs/ncm/
    type: link
---
Next-Concept Modeling (NCM) is a language modeling framework that generates text from coarse concepts to fine details. Unlike autoregressive models that generate one token at a time, or diffusion models that repeatedly refine the entire sequence, NCM predicts increasingly detailed representations of text in parallel. Across unconditional generation, translation, summarization, and reasoning tasks, NCM achieves competitive or better quality than strong diffusion baselines while generating text 10–31x faster and using 4–5x fewer training tokens.
