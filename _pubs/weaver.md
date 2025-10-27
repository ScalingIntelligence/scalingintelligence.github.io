---
title: 'Weaver: Shrinking the Generation-Verification Gap with Weak Verifiers'
authors:
  - key: jonsaadfalcon
    equal: true
    affiliation: Stanford
  - name: E. Kelly Buchanan
    equal: true
    affiliation: Stanford University
  - name: Mayee F. Chen
    equal: true
    affiliation: Stanford University
  - name: Tzu-Heng Huang
    affiliation: University of Wisconsin-Madison
  - key: brendanmclaughlin
  - key: tanvirbhathal
  - name: Shang Zhu
    affiliation: Together AI
  - name: Ben Athiwaratkun
    affiliation: Together AI
  - name: Frederic Sala
    affiliation: University of Wisconsin-Madison
  - name: Scott Linderman
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
  - name: Christopher Ré
    affiliation: Stanford University
venue: neurips
venue_txt: ES-FoMo III @ICML 2025
year: 2025
date: 2025-06-24
has_pdf: true
doi: 10.48550/arXiv.2506.18203
tags:
  - machine learning
  - generative ai
teaser: Weaver boosts language model performance by intelligently combining weak verifiers using weak supervision, achieving near-oracle accuracy with drastically reduced compute.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2506.18203
    type: file-pdf
  - name: Codebase
    url: https://github.com/HazyResearch/scaling-verification
    type: code
  - name: Blog post
    url: https://hazyresearch.stanford.edu/blog/2025-06-18-weaver
    type: link
  - name: Datasets and Models
    url: https://huggingface.co/collections/hazyresearch/weaver-683798010b39c9653ddb9bd8
    type: database
---
Verifiers can improve language model capabilities by scoring and ranking responses from generated candidates. Currently, high-quality verifiers are either unscalable (e.g., humans) or limited in utility (e.g., tools like Lean). While LM judges and reward models have become broadly useful as general-purpose verifiers, a significant performance gap remains between them and oracle verifiers (verifiers with perfect accuracy). To help close this gap, we introduce Weaver, a framework for designing a strong verifier by combining multiple weak, imperfect verifiers. We find weighted ensembles of verifiers, which typically require learning from labeled data, significantly outperform unweighted combinations due to differences in verifier accuracies. To reduce dependency on labeled data, Weaver leverages weak supervision to estimate each verifier's accuracy and combines outputs into a unified score that better reflects true response quality. However, directly applying weak supervision algorithms poses challenges, including inconsistent verifier output formats and handling low-quality verifiers. Weaver addresses these using dataset statistics to normalize outputs and filter specific verifiers. We study Weaver's effectiveness in test-time repeated sampling, where a model generates multiple candidate responses and selects one. Our evaluations show Weaver significantly improves over Pass@1-performance when selecting the first candidate-across reasoning and math tasks, achieving o3-mini-level accuracy with Llama 3.3 70B Instruct as generator, and an ensemble of 70B or smaller judge and reward models as verifiers (87.7% average). This gain mirrors the jump between GPT-4o and o3-mini (69.0% vs. 86.7%), which required extensive finetuning and post-training. To reduce computational costs of verifier ensembles, we train a 400M cross-encoder using Weaver's combined output scores.
