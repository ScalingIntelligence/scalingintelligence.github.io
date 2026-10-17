---
title: "Next-Concept Modeling: Fast, Parallel Coarse-to-Fine Text Generation"
authors:
  - key: hermannkumbong
    equal: true
    affiliation: Stanford University
  - key: ishankhare
    name: Ishan S. Khare
    equal: true
  - name: Simran Arora
    url: https://arorasimran.com/
    advisor: true
    affiliation: Stanford University
  - key: azaliamirhoseini
    advisor: true
    affiliation: Stanford University
  - name: Christopher Ré
    url: https://cs.stanford.edu/~chrismre/
    advisor: true
    affiliation: Stanford University
tags:
  - machine learning
  - natural language processing
  - generative ai
venue: none
year: 2026
date: 2026-10-12
materials:
  - name: Blog
    url: https://scalingintelligence.stanford.edu/blogs/ncm/
    type: link
  - name: Paper
    url: https://arxiv.org/abs/2610.14162
    type: file-pdf
  - name: Codebase
    url: https://github.com/HazyResearch/ncm
    type: code
---

<style>
#authors img.ncm-author-headshot {
  height: 1.5rem;
  object-fit: cover;
}

#authors .ncm-full-team-link {
  margin-top: 0.25rem;
  font-size: 0.9rem;
}

h1.title .ncm-title-logo {
  display: inline-block;
  width: auto;
  height: 1em;
  margin-right: 0.35em;
  vertical-align: -0.1em;
}
</style>

<script>
(function () {
  var title = document.querySelector('h1.title');
  if (title && !title.querySelector('.ncm-title-logo')) {
    title.insertAdjacentHTML('afterbegin', '<svg class="ncm-title-logo" viewBox="-1 -1 107 73" aria-hidden="true"><rect x="20.5" y="0" width="64" height="16" rx="2.5" fill="#E09A0E" stroke="#C48A05" stroke-width="1.4"></rect><rect x="10.25" y="20.5" width="40" height="16" rx="2.5" fill="#F5BA3E" stroke="#C48A05" stroke-width="1.4"></rect><rect x="54.75" y="20.5" width="40" height="16" rx="2.5" fill="#F5BA3E" stroke="#C48A05" stroke-width="1.4"></rect><rect x="0" y="41" width="32" height="30" rx="2.5" fill="#FCE2A6" stroke="#C48A05" stroke-width="1.4"></rect><text x="16" y="63" font-size="21" font-weight="700" text-anchor="middle" fill="#7A5200">N</text><rect x="36.5" y="41" width="32" height="30" rx="2.5" fill="#FCE2A6" stroke="#C48A05" stroke-width="1.4"></rect><text x="52.5" y="63" font-size="21" font-weight="700" text-anchor="middle" fill="#7A5200">C</text><rect x="73" y="41" width="32" height="30" rx="2.5" fill="#FCE2A6" stroke="#C48A05" stroke-width="1.4"></rect><text x="89" y="63" font-size="21" font-weight="700" text-anchor="middle" fill="#7A5200">M</text></svg>');
  }

  var headshots = {
    'Ishan S. Khare': {
      src: '/imgs/people/ishankhare.jpg',
      position: '50% 50%'
    },
    'Simran Arora': {
      src: '/imgs/people/simranarora.png',
      position: '50% 25%'
    },
    'Christopher Ré': {
      src: '/imgs/people/chrisre.jpg',
      position: '50% 28%'
    }
  };
  var equalAdvisors = ['Simran Arora', 'Azalia Mirhoseini', 'Christopher Ré'];

  Array.prototype.forEach.call(document.querySelectorAll('#authors > p'), function (author) {
    var nameElement = author.querySelector('.name');
    if (!nameElement) return;

    var name = nameElement.textContent.replace(/\*$/, '').trim();
    var headshot = headshots[name];
    if (headshot && !author.querySelector('img')) {
      var image = document.createElement('img');
      image.src = headshot.src;
      image.alt = '';
      image.className = 'ncm-author-headshot';
      image.style.objectPosition = headshot.position;

      var container = author.querySelector('a') || author;
      container.insertBefore(image, container.firstChild);
    }

    if (equalAdvisors.indexOf(name) !== -1) {
      var marker = document.createElement('sup');
      marker.textContent = '†';
      nameElement.appendChild(marker);
    }
  });

  var teamLink = document.createElement('p');
  teamLink.className = 'ncm-full-team-link pure-u-1';
  teamLink.innerHTML = '<a href="#full-author-list">Full NCM Team</a>';
  document.getElementById('authors').appendChild(teamLink);
}());
</script>

# TL;DR

Can we generate quality text in just a handful of parallel steps? We introduce **Next-Concept Modeling (NCM)**, a new language modeling framework that generates text from coarse concepts to fine details. Unlike autoregressive models that generate one token at a time, or diffusion models that repeatedly refine the entire sequence, NCM predicts increasingly detailed representations of text in parallel. Across a wide range of tasks (unconditional generation, translation, summarization, and reasoning), NCM achieves competitive or better quality than strong diffusion baselines while generating text **10–31× faster** and seeing **4–5× fewer training tokens**.

<div style="text-align: center; padding-top: 0;">
  <img src="/imgs/blog/ncm/hero.png" alt="Next-Concept Modeling: from coarse ideas to coherent text. Four levels, from global ideas to details, lead to a final coherent paragraph." style="width: 100%; height: auto;">
</div>

---

## The Case for Non-Autoregressive Language Models

Language model inference has become a dominant computational workload, driving interest in faster alternatives to autoregressive generation, especially as models produce increasingly long outputs. **Diffusion language models (DLMs)** are a leading alternative: they refine multiple tokens in parallel, potentially accelerating generation compared with token-by-token decoding. This iterative refinement may also benefit tasks that require **global planning**. However, DLMs still require **many sampling steps**, process the **full sequence at each step**, and often need many **passes over the training data**.

We introduce **Next-Concept Modeling (NCM)**, a new point on the non-autoregressive language modeling frontier built around coarse-to-fine generation. NCM learns to represent text as a hierarchy of discrete codes, then generates one level at a time, predicting all codes within each level in parallel. Generation takes only a few steps, and shorter sequences make early steps especially inexpensive. Across our benchmarks, NCM achieves better or comparable quality to strong diffusion baselines while generating substantially faster and using fewer training tokens.

---

## NCM: Next-Concept Modeling at a Glance

The key idea behind NCM is simple: NCM first represents text as a hierarchy that grows from coarse to fine. Early levels of the hierarchy are short, holding just a few discrete codes that capture the broad structure of the text, while later ones are longer and add finer details. Below we show a sample generation trajectory with NCM. The model starts out with some high-level idea of what it wants to generate and then progressively adds detail to it.

<style>
.ncm-trajectory {
  color: #1f2430;
  border: 1px solid #d5d9df;
  padding: 14px 16px;
  margin: 1.25em 0;
  text-align: left;
}

.ncm-trajectory__context {
  color: #6b7280;
  font-size: 13px;
  line-height: 1.5;
}

.ncm-trajectory__context strong,
.ncm-trajectory__status strong {
  color: #1f2430;
}

.ncm-trajectory__levels {
  display: flex;
  gap: 4px;
  margin: 14px 0 10px;
}

.ncm-trajectory__level {
  flex: 1;
  height: 8px;
  min-width: 0;
  padding: 0;
  border: 0;
  border-radius: 0;
  background: #eceef1;
  cursor: pointer;
  transition: background 200ms ease;
}

.ncm-trajectory__level.is-complete {
  background: #eda100;
}

.ncm-trajectory__level:focus-visible,
.ncm-trajectory__button:focus-visible {
  outline: 2px solid #9a6700;
  outline-offset: 2px;
}

.ncm-trajectory__status {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 12px;
  font-size: 13px;
}

.ncm-trajectory__muted {
  color: #6b7280;
  font-weight: 400;
}

.ncm-trajectory__score {
  color: #9a6700;
  font-weight: 700;
}

.ncm-trajectory__meter {
  height: 4px;
  margin: 6px 0 12px;
  background: #eceef1;
}

.ncm-trajectory__meter-fill {
  height: 100%;
  width: 0;
  background: #eda100;
  transition: width 400ms ease;
}

.ncm-trajectory__output {
  min-height: 3.8em;
  font-size: 17px;
  line-height: 1.9;
}

.ncm-trajectory__word {
  display: inline-block;
  padding: 0 3px;
  margin-right: 3px;
}

.ncm-trajectory__word.is-changed {
  animation: ncm-word-fade 350ms ease-out;
}

.ncm-trajectory__controls {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  align-items: center;
  margin-top: 10px;
}

.ncm-trajectory__button {
  padding: 4px 10px;
  border: 1px solid #d5d9df;
  border-radius: 0;
  background: #fff;
  color: #1f2430;
  font: inherit;
  font-size: 13px;
  cursor: pointer;
}

.ncm-trajectory__button:disabled {
  cursor: not-allowed;
  opacity: 0.45;
}

@keyframes ncm-word-fade {
  from { opacity: 0.15; transform: translateY(2px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (prefers-reduced-motion: reduce) {
  .ncm-trajectory__level,
  .ncm-trajectory__meter-fill,
  .ncm-trajectory__word.is-changed {
    animation: none;
    transition: none;
  }
}
</style>

<div class="ncm-trajectory" id="ncm-generation-trajectory">
  <div class="ncm-trajectory__context">
    <div><strong>German:</strong> Es besteht allerdings die Sorge, dass kleine und mittlere Unternehmen angreifbar gegenüber Hacking und Überwachung bleiben.</div>
    <div><strong>Reference:</strong> However, there is concern that small and medium-sized companies remain vulnerable to hacking and surveillance.</div>
  </div>

  <div class="ncm-trajectory__levels" role="tablist" aria-label="NCM generation levels">
    <button class="ncm-trajectory__level" type="button" data-level="0" aria-label="Show level 1"></button>
    <button class="ncm-trajectory__level" type="button" data-level="1" aria-label="Show level 2"></button>
    <button class="ncm-trajectory__level" type="button" data-level="2" aria-label="Show level 3"></button>
    <button class="ncm-trajectory__level" type="button" data-level="3" aria-label="Show level 4"></button>
    <button class="ncm-trajectory__level" type="button" data-level="4" aria-label="Show level 5"></button>
    <button class="ncm-trajectory__level" type="button" data-level="5" aria-label="Show level 6"></button>
    <button class="ncm-trajectory__level" type="button" data-level="6" aria-label="Show level 7"></button>
    <button class="ncm-trajectory__level" type="button" data-level="7" aria-label="Show level 8"></button>
    <button class="ncm-trajectory__level" type="button" data-level="8" aria-label="Show level 9"></button>
    <button class="ncm-trajectory__level" type="button" data-level="9" aria-label="Show level 10"></button>
    <button class="ncm-trajectory__level" type="button" data-level="10" aria-label="Show level 11"></button>
  </div>

  <div class="ncm-trajectory__status">
    <strong>Level <span data-current-level>11</span> of 11 <span class="ncm-trajectory__muted">· decoded output after this level</span></strong>
    <span><span class="ncm-trajectory__muted">BLEU </span><span class="ncm-trajectory__score" data-bleu>77.8</span></span>
  </div>

  <div class="ncm-trajectory__meter" role="progressbar" aria-label="BLEU score" aria-valuemin="0" aria-valuemax="100" aria-valuenow="77.8">
    <div class="ncm-trajectory__meter-fill" data-meter></div>
  </div>

  <div class="ncm-trajectory__output" data-output aria-live="polite">However, there is concern that small and medium-sized enterprises will remain vulnerable to hacking and surveillance.</div>

  <div class="ncm-trajectory__controls">
    <button class="ncm-trajectory__button" type="button" data-play>Play</button>
    <button class="ncm-trajectory__button" type="button" data-prev>‹ Prev</button>
    <button class="ncm-trajectory__button" type="button" data-next disabled>Next ›</button>
  </div>
</div>

<script>
(function () {
  var root = document.getElementById('ncm-generation-trajectory');
  if (!root) return;

  var generations = [
    { text: 'However, it is a important to the the the and the.', bleu: 6.0 },
    { text: 'However, it is a small to to small and and small, and small and and.', bleu: 7.2 },
    { text: 'However, it is a concern for small small small medium-sized and-sized-sized and and and...', bleu: 6.3 },
    { text: 'However, there is a medium of medium-sized enterprises remain remain remain remain to to to to.', bleu: 16.0 },
    { text: 'However, there is a concern of medium medium-sized-sized enterprises remain remain remain and and orientation.', bleu: 17.5 },
    { text: 'However, there is a concern that medium medium-sized enterprises will remain remain remain surveillance surveillance surveillance capabilities', bleu: 17.7 },
    { text: 'However, there is that that and and medium-sized enterprises will remain remain vulnerable surveillance and surveillance.', bleu: 24.2 },
    { text: 'However, there is that concern and and medium-sized enterprises will remain vulnerable to hacking and surveillance.', bleu: 51.7 },
    { text: 'However, there is that that small and medium-sized enterprises will remain vulnerable to to and surveillance.', bleu: 38.9 },
    { text: 'However, there is concern that small and medium-sized enterprises will remain vulnerable to to surveillance surveillance.', bleu: 56.3 },
    { text: 'However, there is concern that small and medium-sized enterprises will remain vulnerable to hacking and surveillance.', bleu: 77.8 }
  ];

  var levelButtons = Array.prototype.slice.call(root.querySelectorAll('[data-level]'));
  var currentLevel = root.querySelector('[data-current-level]');
  var bleu = root.querySelector('[data-bleu]');
  var meter = root.querySelector('[data-meter]');
  var meterTrack = meter.parentElement;
  var output = root.querySelector('[data-output]');
  var playButton = root.querySelector('[data-play]');
  var prevButton = root.querySelector('[data-prev]');
  var nextButton = root.querySelector('[data-next]');
  var reducedMotion = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  var index = generations.length - 1;
  var playing = false;
  var timer = null;

  function normalize(word) {
    return word.toLowerCase().replace(/[^a-z0-9-]/g, '');
  }

  function render(nextIndex, animate) {
    index = nextIndex;
    var generation = generations[index];
    var previousWords = index > 0 ? generations[index - 1].text.split(/\s+/).map(normalize) : [];

    output.replaceChildren();
    generation.text.split(/\s+/).forEach(function (word, wordIndex) {
      var span = document.createElement('span');
      span.className = 'ncm-trajectory__word';
      if (animate && previousWords[wordIndex] !== normalize(word)) {
        span.classList.add('is-changed');
      }
      span.textContent = word;
      output.appendChild(span);
    });

    currentLevel.textContent = String(index + 1);
    bleu.textContent = generation.bleu.toFixed(1);
    meter.style.width = generation.bleu + '%';
    meterTrack.setAttribute('aria-valuenow', String(generation.bleu));

    levelButtons.forEach(function (button, buttonIndex) {
      button.classList.toggle('is-complete', buttonIndex <= index);
      button.setAttribute('aria-selected', buttonIndex === index ? 'true' : 'false');
    });

    prevButton.disabled = index === 0;
    nextButton.disabled = index === generations.length - 1;
  }

  function schedule() {
    window.clearTimeout(timer);
    if (!playing) return;
    timer = window.setTimeout(function () {
      render(index >= generations.length - 1 ? 0 : index + 1, true);
      schedule();
    }, index >= generations.length - 1 ? 1600 : 650);
  }

  function setPlaying(value) {
    playing = value;
    playButton.textContent = playing ? 'Pause' : 'Play';
    playButton.setAttribute('aria-pressed', playing ? 'true' : 'false');
    schedule();
  }

  levelButtons.forEach(function (button) {
    button.addEventListener('click', function () {
      setPlaying(false);
      render(Number(button.getAttribute('data-level')), true);
    });
  });

  playButton.addEventListener('click', function () {
    setPlaying(!playing);
  });

  prevButton.addEventListener('click', function () {
    setPlaying(false);
    render(Math.max(0, index - 1), true);
  });

  nextButton.addEventListener('click', function () {
    setPlaying(false);
    render(Math.min(generations.length - 1, index + 1), true);
  });

  render(index, false);
  if (!reducedMotion) {
    timer = window.setTimeout(function () {
      render(0, true);
      setPlaying(true);
    }, 800);
  }
}());
</script>

<p style="text-align: center;"><em>A single NCM generation on German-to-English translation, decoded level by level. BLEU is computed per level against the reference.</em></p>

In **Stage 1**, we train a multiscale tokenizer to represent text as a coarse-to-fine hierarchy of discrete codes. Building on [VAR's approach to images](https://arxiv.org/abs/2404.02905), we make several changes for text: each level has its own codebook, nested dropout encourages earlier levels to capture meaningful information without relying on later levels, and a token-level reconstruction objective promotes accurate text recovery.

<div style="text-align: center;">
  <img src="/imgs/blog/ncm/stage-1.svg" alt="Stage 1: learning a multiscale tokenizer that converts text into coarse-to-fine residual codes." style="width: 100%; height: auto;">
</div>

In **Stage 2**, we freeze the tokenizer and train a transformer to predict each level from the cumulative reconstruction of earlier levels. We train on all levels in **a single forward pass** and corrupt a fraction of conditioning codes to improve robustness to generation errors. At inference, the model generates one level at a time, predicting **all codes within each level in parallel**. The tokenizer's decoder then converts the completed hierarchy into text. We provide additional details in [our paper](https://arxiv.org/abs/2610.14162).

<div style="text-align: center;">
  <img src="/imgs/blog/ncm/stage-2.svg" alt="Stage 2: learning to generate each concept level from cumulative reconstructions of earlier levels." style="width: 100%; height: auto;">
</div>

---

## Evaluating NCM

We evaluate NCM on language modeling, machine translation, extreme summarization, reasoning (Sudoku), scaling (training and test time), inference efficiency, and training efficiency.

### Language Modeling

Following standard practice in the diffusion language modeling literature, we evaluate NCM on three datasets, [TinyStories](https://arxiv.org/abs/2305.07759), [One Billion Word (LM1B)](https://arxiv.org/abs/1312.3005), and [OpenWebText](https://skylion007.github.io/OpenWebTextCorpus/), using generative perplexity, and report the mean per-sample unigram entropy. NCM achieves similar or better quality compared to the strongest baselines while using fewer function evaluations (NFE).

<div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: flex-start; gap: 8px;">
  <img src="/imgs/blog/ncm/lm-tinystories-v4.png" alt="Generative perplexity against NFE on TinyStories. NCM matches the baselines with 11× fewer function evaluations." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/lm-lm1b-v4.png" alt="Generative perplexity against NFE on LM1B. NCM reaches better perplexity with 11× fewer function evaluations." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/lm-openwebtext-v4.png" alt="Generative perplexity against seconds per sequence on OpenWebText. NCM is 10× faster at comparable perplexity." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
</div>
<p style="text-align: center;"><em>Generative perplexity on TinyStories, LM1B, and OpenWebText. NCM reaches comparable or better quality with 11× fewer function evaluations.</em></p>

### Inference Efficiency

Efficiency in the diffusion language modeling literature is often reported through the number of function evaluations (NFE), which overlooks potentially **substantial differences in the cost per evaluation**. We therefore benchmark wall-clock generation time on a single NVIDIA H100 SXM 80GB GPU. For unconditional generation, NCM runs **21× faster on LM1B** and **10× faster on OpenWebText** than the strongest diffusion baselines, with better or comparable generative perplexity. For conditional generation, it matches the strongest diffusion baseline's ROUGE-L on XSum while running **31× faster**.

<div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: flex-start; gap: 8px;">
  <img src="/imgs/blog/ncm/latency-lm1b-v4.png" alt="Generative perplexity against wall-clock time on LM1B. NCM is 21× faster than the strongest diffusion baseline." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/latency-xsum-v4.png" alt="ROUGE-L against wall-clock time on XSum. NCM matches the strongest diffusion baseline while running 31× faster." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/latency-sudoku-v4.png" alt="Hard Sudoku solve rate by method with NFE counts. NCM solves the most boards at 12 NFE." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
</div>
<p style="text-align: center;"><em>Wall-clock generation time on an H100. Unconditional generation on LM1B, conditional generation on XSum, and reasoning on hard Sudoku.</em></p>

### Conditional Generation

The growing consensus is that generative perplexity [does not fully capture generative quality](https://arxiv.org/abs/2606.08417). We further evaluate NCM on machine translation (WMT14 De-En) and extreme summarization (XSum). On both tasks, NCM achieves similar or better performance than the strongest diffusion baselines [while being up to 31× faster](#inference-efficiency).

### Reasoning

We hypothesize that coarse-to-fine generation supports global planning by establishing an overall structure before resolving finer details. We evaluate this intuition on Sudoku, where solutions must satisfy multiple constraints. NCM outperforms both autoregressive and diffusion baselines across all difficulty levels while requiring fewer function evaluations.

<div style="text-align: center;">
  <img src="/imgs/blog/ncm/sudoku.svg" alt="Completely solved Sudoku boards by difficulty. NCM achieves the highest solve rate and degrades the least as puzzles get harder." style="width: 100%; height: auto;">
</div>
<p style="text-align: center;"><em>Completely solved Sudoku boards by difficulty. NCM degrades the least as puzzles get harder. The full comparison is in the paper.</em></p>

### Scaling

We show preliminary scaling results for NCM: scaling the model parameter count consistently improves generative perplexity and unigram entropy. We also use NCM's fast sampling for test-time scaling. Minimum Bayes risk (MBR) decoding over multiple candidates improves BLEU on WMT14 and ROUGE on XSum. Although other methods can also benefit from MBR, NCM generates more candidates within the same sampling-time budget, enabling it to outperform diffusion baselines.

<div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: flex-start; gap: 8px;">
  <img src="/imgs/blog/ncm/scaling-model-v4.png" alt="Generative perplexity against training tokens for NCM at 97M, 317M, and 725M parameters on LM1B." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/scaling-tts-wmt-v4.png" alt="Test-time scaling on WMT14: BLEU against wall-clock time for NCM with K MBR candidates against diffusion baselines." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
  <img src="/imgs/blog/ncm/scaling-tts-xsum-v4.png" alt="Test-time scaling on XSum: ROUGE-L against wall-clock time for NCM with K MBR candidates against diffusion baselines." style="flex: 1 1 200px; min-width: 0; max-width: 100%; height: auto;">
</div>
<p style="text-align: center;"><em>Model scaling on LM1B, and test-time scaling with MBR decoding on WMT14 and XSum.</em></p>

### Lower Training Budget

NCM achieves these quality and inference gains with substantially fewer training tokens. On LM1B and OpenWebText, it uses approximately **4.9×** and **4×** fewer tokens than strong diffusion baselines, respectively, even when accounting for both tokenizer and generator training.

---

## What's Next

We are excited that such a different way of generating text works at all, and produces strong early results. But we are even more excited about what comes next:

- **Further scaling.** We showed initial scaling results, but how does NCM scale with even more parameters, FLOPs, and tokens?
- **Learning representations jointly with generation.** Training the tokenizer and generator together could produce representations better suited to the generation task.
- **Adapting computation to difficulty.** Instead of using a fixed hierarchy for every example, NCM could learn when to stop or allocate additional computation, potentially improving both speed and quality.

Feedback is always welcome! Feel free to reach out at [kumboh@cs.stanford.edu](mailto:kumboh@cs.stanford.edu).

---

## Full Author List

[Hermann Kumbong](https://x.com/KumbongHermann)<sup>*</sup>, [Ishan S. Khare](https://iskhare.github.io/)<sup>*</sup>, [Iddah Mlauzi](https://www.linkedin.com/in/iddah-mlauzi/), [Andrew Woen](https://www.linkedin.com/in/andrew-woen-539822195/), [Charles Ding](https://www.linkedin.com/in/charles-dingg/), [E. Kelly Buchanan](https://ekbuchanan.com/), [Simran Arora](https://arorasimran.com/)<sup>†</sup>, [Azalia Mirhoseini](https://www.azaliamirhoseini.com/)<sup>†</sup>, and [Christopher Ré](https://cs.stanford.edu/~chrismre/)<sup>†</sup>

<sup>*</sup> Equal contribution. <sup>†</sup> Equal advising.
