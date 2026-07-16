---
title: 'Distill to Detect: Exposing Stealth Biases in LLMs through Cartridge Distillation'
authors:
  - key: shayantalaei
    equal: true
    affiliation: Stanford University
  - name: Abhinav Chinta
    equal: true
    affiliation: Stanford University
  - name: Devvrit Khatri
    affiliation: University of Texas at Austin
  - name: Amin Karbasi
    affiliation: Stanford University, Cisco Systems
  - key: azaliamirhoseini
    affiliation: Stanford University
  - name: Amin Saberi
    affiliation: Stanford University
venue: colm
venue_txt: Mech Interp, TAIGR, CoLoRAI, AI4GOOD @ ICML 2026
year: 2026
date: 2026-07-01
has_pdf: true
doi: 10.48550/arXiv.2607.01208
bibtex_raw: |
  @misc{talaei2026distilldetectexposingstealth,
        title={Distill to Detect: Exposing Stealth Biases in LLMs through Cartridge Distillation},
        author={Shayan Talaei and Abhinav Chinta and Devvrit Khatri and Amin Karbasi and Azalia Mirhoseini and Amin Saberi},
        year={2026},
        eprint={2607.01208},
        archivePrefix={arXiv},
        primaryClass={cs.CL},
        url={https://arxiv.org/abs/2607.01208},
  }
tags:
  - machine learning
  - generative ai
teaser: D2D distills the distributional shift between a suspected model and its base into a KV-cache prefix cartridge, amplifying hidden preferential biases into generated text so they can be reliably detected.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2607.01208
    type: file-pdf
  - name: Website
    url: https://distill2detect.github.io/
    type: link
  - name: Codebase
    url: https://github.com/abhinav-chinta/Distill2Detect
    type: code
---
Language models deployed in high-stakes roles can potentially favor certain entities, brands, or viewpoints, steering user decisions at scale. Such preferential biases can be introduced by any actor in the model's supply chain and are most dangerous when the model reveals its preference only on the relevant topic while behaving identically to its unmodified base on all other inputs. Recent work has shown that these biases can transfer through context distillation on semantically unrelated data, with the signal residing entirely in the soft logit distribution and remaining invisible to text-based inspection. However, the defender faces a fundamental asymmetry: without knowing the bias topic, no detection method can reliably surface a stealth preferential bias, regardless of whether it examines generated text, internal representations, or model weights. Here we introduce Distill to Detect (D2D), a method that surfaces hidden biases by distilling the distributional shift between a suspected model and its base into a cartridge (a KV-cache prefix adapter), concentrating the dominant divergence and amplifying the bias signal into generated text. We show that D2D successfully amplifies the hidden biases of stealth models to the extent that they can be reliably detected across multiple bias types. We also propose a theoretical framework that explains the efficacy of D2D through the lens of Fisher-weighted projection of the logit distribution shift, supported by empirical observations. By turning the capacity bottleneck of prefix-tuning adapters into a detection tool, D2D provides a practical building block for auditing hidden behaviors in deployed language models.
