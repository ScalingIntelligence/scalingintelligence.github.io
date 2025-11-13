---
title: 'Intelligence Per Watt: A Study of Local Intelligence Efficiency'
authors:
  - key: jonsaadfalcon
    equal: true
    affiliation: Stanford
  - name: Avanika Narayan
    equal: true
    affiliation: Stanford University
  - name: Hakki Orhun Akengin
    affiliation: Stanford University
  - name: J. Wes Griffin
    affiliation: Stanford University
  - name: Herumb Shandilya
    affiliation: Stanford University
  - key: adriangamarralafuente
  - name: Medhya Goel
    affiliation: Stanford University
  - name: Rebecca Joseph
    affiliation: Stanford University
  - key: shloknatarajan
  - name: Etash Kumar Guha
    affiliation: Stanford University
  - name: Shang Zhu
    affiliation: Together AI
  - name: Ben Athiwaratkun
    affiliation: Together AI
  - name: John Hennessy
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
  - name: Christopher Ré
    affiliation: Stanford University
venue: preprint
year: 2025
date: 2025-11-11
has_pdf: true
doi: 10.48550/arXiv.2511.07885
tags:
  - machine learning
  - systems
  - hardware efficiency
teaser: We introduce intelligence-per-watt (IPW) to measure how efficiently inference systems convert energy into useful computation. Local LMs accurately respond to 88.7% of single-turn chat and reasoning queries, with local intelligence efficiency improving 5.3x from 2023-2025.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2511.07885
    type: file-pdf
  - name: Codebase
    url: https://github.com/HazyResearch/intelligence-per-watt
    type: code
  - name: Blog post
    url: https://hazyresearch.stanford.edu/blog/2025-11-11-ipw
    type: link
---
Large language model (LLM) queries are predominantly processed by frontier models in centralized cloud infrastructure. Rapidly growing demand strains this paradigm, and cloud providers struggle to scale infrastructure at pace. Two advances enable us to rethink this paradigm: small LMs (<=20B active parameters) now achieve competitive performance to frontier models on many tasks, and local accelerators (e.g., Apple M4 Max) run these models at interactive latencies. This raises the question: can local inference viably redistribute demand from centralized infrastructure? Answering this requires measuring whether local LMs can accurately answer real-world queries and whether they can do so efficiently enough to be practical on power-constrained devices (i.e., laptops). We propose intelligence per watt (IPW), task accuracy divided by unit of power, as a metric for assessing capability and efficiency of local inference across model-accelerator pairs. We conduct a large-scale empirical study across 20+ state-of-the-art local LMs, 8 accelerators, and a representative subset of LLM traffic: 1M real-world single-turn chat and reasoning queries. For each query, we measure accuracy, energy, latency, and power. Our analysis reveals 3 findings. First, local LMs can accurately answer 88.7% of single-turn chat and reasoning queries with accuracy varying by domain. Second, from 2023-2025, IPW improved 5.3x and local query coverage rose from 23.2% to 71.3%. Third, local accelerators achieve at least 1.4x lower IPW than cloud accelerators running identical models, revealing significant headroom for optimization. These findings demonstrate that local inference can meaningfully redistribute demand from centralized infrastructure, with IPW serving as the critical metric for tracking this transition. We release our IPW profiling harness for systematic intelligence-per-watt benchmarking.

