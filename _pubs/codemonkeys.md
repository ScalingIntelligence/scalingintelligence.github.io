---
title: 'CodeMonkeys: Scaling Test-Time Compute for Software Engineering'
authors:
  - key: ryanehrlich
    equal: true
  - key: bradleybrown
    affiliation: University of Oxford
    equal: true
  - key: jordanjuravsky
    equal: true
  - name: Ronald Clark
    affiliation: University of Oxford
  - name: Christopher Ré
    affiliation: Stanford
  - key: azaliamirhoseini
venue: preprint
year: 2025
day: 23
has_pdf: true
doi: 
tags:
  - machine learning
  - generative AI
teaser: In this work, we present CodeMonkeys, a system designed to solve software engineering problems by scaling test time compute. CodeMonkeys resolves 57.4% of issues in SWE-bench Verified. When ensembling with edits from existing top SWE-bench submissions, we obtains a score of 66.2% outperforming the best member of the ensemble on its own.
materials:
  - name: Paper
    url: https://arxiv.org/abs/2501.14723
    type: file-pdf
  - name: CodeMonkeys Codebase
    url: https://github.com/ScalingIntelligence/codemonkeys
    type: code
  - name: Trajectories
    url: https://github.com/swe-bench/experiments/pull/171
    type: database
  - name: Codebase Content Dataset
    url: https://huggingface.co/datasets/ScalingIntelligence/swe-bench-verified-codebase-content
    type: database
---
Scaling test-time compute is a promising axis for improving LLM capabilities.
However, test-time compute can be scaled in a variety of ways, and effectively combining different approaches remains an active area of research. 
Here, we explore this problem in the context of solving real-world GitHub issues from the SWE-bench dataset. 
Our system (CodeMonkeys) allows models to iteratively edit a codebase by jointly developing and running a testing script alongside their draft edit. 
We sample many of these multi-turn trajectories for every issue to generate a collection of candidate edits. 
This approach lets us scale "serial" test-time compute by increasing the number of iterations per trajectory and "parallel" test-time compute by increasing the number of trajectories per problem. 
With parallel scaling, we can amortize up-front costs across multiple downstream samples, allowing us to identify relevant codebase context using the simple method of letting an LLM read every file.
In order to select between candidate edits, we combine voting with model-generated tests with a final multi-turn trajectory dedicated to selection.
Overall, CodeMonkeys resolves 57.7% of issues from SWE-bench Verified using a budget of approximately 2300 USD.
Our selection method can also be used to combine candidates from different sources. Selecting over an ensemble of edits from existing top SWE-bench submissions obtains a score of 66.2% and outperforms the best member of the ensemble on its own.
