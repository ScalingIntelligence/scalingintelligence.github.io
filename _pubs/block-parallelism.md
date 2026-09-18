---
title: 'Block Parallelism For Efficient Distributed Long-Context Diffusion Language Model Training'
authors:
  - name: Tarun Suresh
    equal: true
    affiliation: Stanford University
  - name: Pranshu Chaturvedi
    equal: true
    affiliation: Stanford University
  - key: hangookang
    equal: true
    affiliation: Stanford University
  - name: Parth Shroff
    affiliation: Stanford University
  - name: Ishan S. Khare
    affiliation: Stanford University
  - key: hermannkumbong
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
venue: preprint
year: 2026
date: 2026-09-16
has_pdf: false
doi: 10.48550/arXiv.2609.19242
bibtex_raw: |
  @misc{suresh2026blockparallelismefficientdistributed,
        title={Block Parallelism For Efficient Distributed Long-Context Diffusion Language Model Training},
        author={Tarun Suresh and Pranshu Chaturvedi and Hangoo Kang and Parth Shroff and Ishan S. Khare and Hermann Kumbong and Azalia Mirhoseini},
        year={2026},
        eprint={2609.19242},
        archivePrefix={arXiv},
        primaryClass={cs.LG},
        url={https://arxiv.org/abs/2609.19242},
  }
tags:
  - machine learning
  - ml systems
  - generative ai
teaser: Context-Sharded Block Parallelism (CSBP) accelerates long-context training for block diffusion language models and diffusion-based speculative decoding while preserving the training objective and gradients.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2609.19242
    type: file-pdf
  - name: Website
    url: https://scalingintelligence.stanford.edu/Turbo-dLLM/
    type: link
  - name: Codebase
    url: https://github.com/ScalingIntelligence/Turbo-dLLM
    type: code
---
Block diffusion language models (BDLMs) combine autoregressive dependencies across blocks with parallel denoising within blocks, but long-context training is constrained by distributed attention communication and activation memory. Conventional context parallelism (CP) shards the combined clean-plus-corrupted sequence by position, communicating shared clean K/V together with block-specific corrupted K/V and their gradients. We observe that the BDLM objective separates over target blocks. We introduce block parallelism (BP), a new distributed parallelism dimension that assigns each corrupted-block computation to one rank. To scale BP to long contexts, we introduce context-sharded block parallelism (CSBP), which also shards the shared clean sequence across those ranks. CSBP keeps corrupted K/V and gradients local, avoids replicated clean prefixes, and preserves BDLM training semantics. On 16 H200 GPUs at 256K context, CSBP improves throughput over the best baseline by 1.18-1.45x for supervised fine-tuning and 1.27-1.33x for conversion of autoregressive models to BDLMs, while matching or reducing peak HBM. Full-model speedup reaches 1.61x at 512K. On eight H100 GPUs, CSBP accelerates DFlash2 speculative-decoder training by 2.48x at 512K and 7.59x at 1M. In matched 12-hour DiffusionGemma 26B-A4B SFT runs, CSBP achieves higher pass rates at every trained checkpoint on SWE-bench Verified and Terminal-Bench Lite.
