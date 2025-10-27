---
title: 'How Do Large Language Monkeys Get Their Power (Laws)?'
authors:
  - name: Rylan Schaeffer
    affiliation: University of California, Berkeley
  - name: Joshua Kazdan
    affiliation: Stanford University
  - name: John Hughes
    affiliation: University of Cambridge
  - name: Jordan Juravsky
    affiliation: University of California, Berkeley
  - name: Sara Price
    affiliation: University of Cambridge
  - name: Aengus Lynch
    affiliation: University of Cambridge
  - name: Erik Jones
    affiliation: University of Toronto
  - name: Robert Kirk
    affiliation: University of Cambridge
  - key: azaliamirhoseini
  - name: Sanmi Koyejo
    affiliation: Stanford University
venue: icml
venue_txt: Oral
year: 2025
date: 2025-02-24
month: February
day: 24
has_pdf: true
doi: 10.48550/arXiv.2502.17578
tags:
  - machine learning
  - generative ai
slug: monkeyspower
teaser: We explain how language models exhibit power-law scaling in success rates despite per-problem exponential scaling, revealing that heavy-tailed distributions of success probabilities drive this phenomenon and enabling more efficient performance forecasting.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2502.17578
    type: file-pdf
---
Recent research across mathematical problem solving, proof assistant programming and multimodal jailbreaking documents a striking finding: when (multimodal) language model tackle a suite of tasks with multiple attempts per task -- succeeding if any attempt is correct -- then the negative log of the average success rate scales a power law in the number of attempts. In this work, we identify an apparent puzzle: a simple mathematical calculation predicts that on each problem, the failure rate should fall exponentially with the number of attempts. We confirm this prediction empirically, raising a question: from where does aggregate polynomial scaling emerge? We then answer this question by demonstrating per-problem exponential scaling can be made consistent with aggregate polynomial scaling if the distribution of single-attempt success probabilities is heavy tailed such that a small fraction of tasks with extremely low success probabilities collectively warp the aggregate success trend into a power law - even as each problem scales exponentially on its own. We further demonstrate that this distributional perspective explains previously observed deviations from power law scaling, and provides a simple method for forecasting the power law exponent with an order of magnitude lower relative error, or equivalently, ~2-4 orders of magnitude less inference compute. Overall, our work contributes to a better understanding of how neural language model performance improves with scaling inference compute and the development of scaling-predictable evaluations of (multimodal) language models.
