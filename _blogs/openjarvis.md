---
title: "OpenJarvis: Personal AI, On Personal Devices"
authors:
  - key: jonsaadfalcon
    equal: true
    affiliation: Stanford
  - name: Avanika Narayan
    equal: true
    affiliation: Stanford University
  - name: John Hennessy
    affiliation: Stanford University
  - name: Christopher Ré
    affiliation: Stanford University
  - key: azaliamirhoseini
    affiliation: Stanford University
venue: none
year: 2025
date: 2025-07-01
tags:
  - machine learning
  - systems
  - personal AI
  - on-device inference
materials:
  - name: GitHub
    url: https://github.com/open-jarvis/OpenJarvis
    type: code
  - name: Docs
    url: https://open-jarvis.github.io/OpenJarvis/
    type: file-pdf
---

<div class="post-content">

<img src="/imgs/blog/openjarvis/banner.png" alt="OpenJarvis" style="width: 100%; height: auto;">
<p><em><strong>TL;DR:</strong> OpenJarvis is an open-source framework for personal AI agents that runs entirely on-device. It provides shared primitives for building on-device agents, efficiency-aware evaluations, and a learning loop that improves models using local trace data.</em></p>

<p style="text-align: center;">
💻 <a href="https://github.com/open-jarvis/OpenJarvis">GitHub</a> &nbsp;|&nbsp;
📖 <a href="https://open-jarvis.github.io/OpenJarvis/">Docs</a> &nbsp;|&nbsp;
💬 <a href="https://discord.gg/3DEGU5Ce">Discord</a>
</p>

<p style="text-align: center;"><strong>🚨 Download OpenJarvis today and top the <a href="https://open-jarvis.github.io/OpenJarvis/leaderboard/">ENERGY leaderboard</a> for a chance to win a Mac Mini! 🚨</strong></p>

<hr>

<p>In the 1970s and 80s, computing moved from mainframes to personal computers. Not because PCs were more powerful, but because they became efficient enough for what people actually needed. AI is reaching a similar moment.</p>

<p>In our recent <a href="https://arxiv.org/abs/2511.07885">Intelligence Per Watt study</a>, we found that local language models and local accelerators can accurately service 88.7% of single-turn chat and reasoning queries at interactive latencies, with intelligence efficiency improving 5.3× from 2023 to 2025.</p>

<p>At the same time, personal AI is exploding. Frameworks like OpenClaw have attracted more than 250,000 GitHub stars, inspiring a wave of agents (<a href="https://github.com/sipeed/picoclaw">PicoClaw</a>, <a href="https://github.com/HKUDS/nanobot">NanoBot</a>, <a href="https://github.com/nearai/ironclaw">IronClaw</a>, <a href="https://github.com/warengonzaga/tinyclaw">TinyClaw</a>, <a href="https://github.com/memovai/mimiclaw">MimicLaw</a>, <a href="https://github.com/zeroclaw-labs/zeroclaw">ZeroClaw</a>, etc) all built around the same idea: AI that operates over your personal context, interacting through the platforms you already use.</p>

<p>Put these together and the architecture seems obvious: personal AI should run on your personal device.</p>

<p>In nearly all of today's personal AI projects, the local component is a thin orchestration layer, while the "brain" lives in someone else's data center. Your most personal data routes through cloud APIs, with their latency, their cost, and their terms of service.</p>

<p>We built OpenJarvis to fix this.</p>

<p>OpenJarvis is an opinionated framework for personal AI running on your personal devices. It provides shared primitives for building on-device agents, evaluations that treat metrics such as energy, FLOPs, latency, and dollar cost as first-class constraints, and a learning loop that improves models using local trace data.</p>

<p>The goal is simple: make it possible to build personal AI agents that run locally by default, calling the cloud only when truly necessary. OpenJarvis aims to be both a research platform and production foundation for local AI, in the spirit of PyTorch.</p>

<hr>

<h2>🏢 OpenJarvis: The Local-First Personal AI Stack</h2>

<p>OpenJarvis emerged from a simple question: what's standing in the way of personal AI running locally today?  We believe the answer comes down to three missing pieces in today's local AI systems:</p>

<ul>
<li><strong>Shared abstractions.</strong> Teams assemble bespoke stacks, choosing independently among model servers, orchestration frameworks, memory stores, tool interfaces, and adaptation pipelines. The result is duplicated effort and brittle, non-interoperable systems. There is no agreed-upon "local AI software stack" the way there is for web development or mobile apps.</li>

<li><strong>Efficiency-aware evaluations</strong>: Systems are tuned for task quality alone, even though on-device deployments must jointly satisfy constraints on latency, energy, memory footprint, and dollar cost. Efficiency isn't a nice-to-have on a laptop running on battery; it's a hard requirement.</li>

<li><strong>Closed-loop optimization</strong> Because most AI systems run in the cloud, the pieces needed for local improvement don't exist: trace data isn't available, model weights are closed, and the runtime isn't tunable. This makes it nearly impossible to study or build personal AI agents that improve over time.</li>
</ul>

<p>To close these gaps, we built OpenJarvis. OpenJarvis is the open-source stack for <strong>personal AI agents that runs entirely on-device.</strong> Designed to serve as both a research platform and deployment-ready infrastructure, OpenJarvis does three things:</p>

<ul>
<li>Defines a <strong>set of composable primitives</strong> that replace ad hoc integration with an opinionated framework of five primitives — Intelligence, Engine, Agents, Tools & Memory, and Learning — providing the shared abstractions the ecosystem currently lacks. These primitives can be studied individually or as an integrated whole.</li>

<li>Makes <strong>efficiency a first-class evaluation target</strong> by tracking energy, dollar cost, FLOPs, latency, and related system metrics alongside accuracy. These measurements are essential for optimizing edge deployments, where resource constraints are fundamental.</li>

<li><strong>Provides an optimization harness for deploying optimization strategies</strong> across the complete local AI stack: 1) model weights, 2) LM prompts, 3) agentic logic, and 4) inference engine. By learning from local trace data, the harness applies the best optimization strategies to-date while giving researchers a testbed to explore new approaches tailored to the trace signatures that distinguish personal AI (i.e., long-horizon sessions, persistent cross-session context, non-stationary user preferences).</li>
</ul>

<hr>

<h2>🧱 Primitives for On-Device AI</h2>

<p>OpenJarvis is structured around five composable primitives. Each primitive can be benchmarked, substituted, and optimized independently, or analyzed within the context of the full system. Collectively, these primitives define modular, hardware-aware abstractions that support both standalone use and system-level composition.</p>

<img src="/imgs/blog/openjarvis/fig_1.png" alt="OpenJarvis Architecture Diagram" style="width: 100%; height: auto;">
<p><em>Figure 1: The five primitives of OpenJarvis (Intelligence, Engine, Agents, Tools & Memory, and Learning) form a composable, hardware-aware stack for on-device personal AI.</em></p>

<h3>🧠 Intelligence: On-Device Language Models</h3>

<p>The <strong>Intelligence</strong> primitive is the model layer: the on-device language models that provide reasoning, generation, and understanding. Recent progress in open models has made personal AI on consumer hardware newly practical. Families like <a href="https://qwen.ai/blog?id=qwen3.5">Qwen</a>, <a href="https://openai.com/index/introducing-gpt-oss/">GPT-OSS</a>, <a href="https://ai.google.dev/gemma/docs/gemma-3n">Gemma</a>, <a href="https://github.com/ibm-granite/granite-4.0-language-models">Granite</a>, <a href="https://github.com/THUDM/GLM-4">GLM</a>, <a href="https://kimi.ai/">Kimi</a>, and others now span a wide range of sizes, context lengths, and efficiency profiles, making it possible to match meaningful capability to local hardware.</p>

<p>OpenJarvis sits above that rapidly changing landscape with a unified model catalog. Rather than forcing users to track model releases, parameter counts, or memory tradeoffs, OpenJarvis lets them specify the capability they need and then determines what their hardware can realistically support. The goal is not just model access, but a stable interface over a fast-moving ecosystem — one that makes it possible to study how model choice, independent of agent logic or inference backend, affects task quality, efficiency, and personalization over time.</p>

<h3>⚙️ Engine: Hardware-Aware Inference</h3>

<p>The <strong>Engine</strong> primitive is the execution layer: the inference backend that determines how models actually run on a device. Local inference today is powerful but fragmented, with backends such as <a href="https://ollama.com/">Ollama</a>, <a href="https://github.com/vllm-project/vllm">vLLM</a>, <a href="https://github.com/sgl-project/sglang">SGLang</a>, <a href="https://github.com/ggml-org/llama.cpp">llama.cpp</a>, <a href="https://github.com/apple/python-apple-fm-sdk">Apple Foundation Models</a>, <a href="https://github.com/exo-explore/exo">Exo</a>, <a href="https://github.com/NexaAI/nexa-sdk">Nexa</a>, and <a href="https://github.com/trymirai/uzu">Mirai Uzu</a> each offering different strengths depending on platform, memory, and performance constraints.</p>

<p>OpenJarvis provides a hardware-aware interface over that fragmentation. With commands like <code>jarvis init</code>, it detects the user's system and recommends an engine and model configuration suited to the available hardware; with <code>jarvis doctor</code>, it helps keep that setup healthy over time.</p>

<h3>🤖 Agents: Composable Reasoning</h3>

<p>The <strong>Agents</strong> primitive is the behavior layer: the reasoning patterns that turn raw model capability into structured action. Existing approaches such as <a href="https://arxiv.org/abs/2210.03629">ReAct</a> (Yao et al., 2023) and <a href="https://arxiv.org/abs/2407.16741">OpenHands</a> (Wang et al., 2024) show how models can plan, call tools, and iterate on tasks, but many agent frameworks assume abundant compute and memory. On-device systems require something more disciplined: agents that can reason effectively within bounded context windows, limited working memory, and strict efficiency constraints.</p>

<p>OpenJarvis provides a composable set of agent roles designed for those constraints. It supports established reasoning patterns while introducing roles such as the <strong>Orchestrator</strong>, which breaks complex tasks into subtasks and delegates them, and the <strong>Operative</strong>, a lightweight executor for recurring personal AI workflows. Rather than relying on a single general-purpose agent, OpenJarvis lets developers combine specialized agents for planning, delegation, and execution on local hardware.</p>

<h3>🔧 Tools & Memory: Grounding Intelligence in the Real World</h3>

<p>The <strong>Tools & Memory</strong> primitive is the grounding layer: the mechanisms that connect intelligence to the outside world and to persistent personal context. Models become far more useful when they can retrieve documents, call tools, communicate with other agents, and operate across the channels where users already live. The challenge is that these integrations are often inconsistent, cloud-dependent, or difficult to compose.</p>

<p>OpenJarvis provides a local-first interaction pattern over both tools and memory. It includes native support for <a href="https://modelcontextprotocol.io/">MCP</a> (Model Context Protocol) for standardized tool use, <a href="https://github.com/google/A2A">Google A2A</a> (Agent-to-Agent) for inter-agent communication, and semantic indexing for local retrieval over papers, notes, and documents. It also connects to a broad set of messaging platforms, webchat, and webhooks. The result is a system whose intelligence is grounded in the user's actual environment, while keeping storage and control local by default.</p>

<h3>📚 Learning: Self-Improving Systems</h3>

<p>The <strong>Learning</strong> primitive is the adaptation layer: the mechanisms that help the system get better over time. Techniques such as <strong>supervised fine-tuning</strong>, <strong>LoRA</strong>, <strong>GRPO</strong>, and <strong>bandit-based routing</strong> have made model and agent improvement more accessible, but in most systems these remain separate from everyday use. Personal AI should not just run locally; it should learn locally from accumulated interaction and feedback.</p>

<p>OpenJarvis turns that idea into an operational loop. Its learning layer uses personal traces to synthesize training data, refine agent behavior, and improve model selection over time, with commands like <code>jarvis optimize</code> packaging that process into a usable workflow.</p>

<hr>

<h2>⚡ Efficiency as a First-Class Metric</h2>

<p>Most AI frameworks treat efficiency as an afterthought. OpenJarvis inverts this: <strong>energy and dollar cost are first-class design constraints alongside accuracy from the start.</strong></p>

<p>OpenJarvis includes a hardware-agnostic telemetry system that profiles energy consumption across NVIDIA GPUs (via NVML), AMD GPUs, and Apple Silicon (via <code>powermetrics</code>), sampling at 50ms intervals. The <code>jarvis bench</code> command provides standardized benchmarking of latency, throughput, and energy per query. A built-in dashboard visualizes cost savings, model comparisons, and efficiency metrics in real time.</p>

<img src="/imgs/blog/openjarvis/fig_2.png" alt="OpenJarvis benchmarking CLI showing latency, throughput, and energy metrics" style="width: 100%; height: auto;">
<p><em>Figure 2: The OpenJarvis dashboard provides real-time visibility into inference latency, energy consumption, cost savings, and model performance across local and cloud configurations.</em></p>

<hr>

<h2>🔒 Learning from Local Traces</h2>

<p>Because execution is local and interaction traces remain on-device, OpenJarvis captures rich, structured trace data across every layer of the stack — from raw inference telemetry and prompt–completion pairs to agent decision trajectories and tool call sequences. This local trace infrastructure enables closed-loop optimization across four layers of the stack:</p>

<ul>
<li><strong>Model weights</strong> — gradient-based updates including SFT, GRPO, DPO, and other reinforcement learning from human feedback methods that fine-tune local model parameters directly.</li>

<li><strong>LM prompts</strong> — prompt optimization strategies such as DSPy that automatically refine instructions and few-shot examples to improve task performance without modifying model weights.</li>

<li><strong>Agentic logic</strong> — agent-level optimization approaches like GEPA that improve how agents decompose tasks, select tools, and coordinate sub-agents.</li>

<li><strong>Inference engine</strong> — engine-level tuning including quantization selection, batch scheduling, and hardware-specific kernel configuration.</li>
</ul>

<hr>

<h2>🚀 What Can You Do With OpenJarvis?</h2>

<p>OpenJarvis can be used from the <strong>command line</strong> (<code>jarvis ask</code>, <code>jarvis chat</code>), through a <strong>browser-based dashboard</strong> with built-in webchat, or via a <strong>Tauri-based desktop app</strong> on macOS, Linux, and Windows.</p>

<img src="/imgs/blog/openjarvis/fig_3.png" alt="OpenJarvis browser dashboard with webchat, energy metrics, and cost comparison" style="width: 100%; height: auto;">
<p><em>Figure 3: OpenJarvis supports interaction via CLI, browser dashboard, desktop app, and 26+ messaging channels.</em></p>

<p>Here are some ways to use it!</p>

<ul>
<li><strong>Personal AI tasks.</strong> Email triage, morning briefings, daily digests, and scheduled summaries that run on a cron schedule without cloud dependencies. Point OpenJarvis at a folder of papers or notes and it builds a local knowledge base for question answering and research. Connect it to your messaging platforms and interact through iMessage, Telegram, WhatsApp, or whatever you already use. Critically, all of your personal details, documents, messages, and preferences stay on your device, unlike cloud-based systems that send everything to external servers.</li>

<li><strong>Traditional LM workloads.</strong> Open-ended chat, mathematical and scientific reasoning, code generation, knowledge-intensive question answering, and structured output generation, all running locally with energy and cost tracked per query.</li>

<li><strong>Agentic and long-horizon tasks.</strong> The scheduler enables cron-based automation ("every morning at 7am, pull my calendar, check my email, and prepare a briefing") while the agent framework supports multi-step workflows like code review, web research, and document processing pipelines.</li>
</ul>

<hr>

<h2>🤝 Get Involved</h2>

<p>OpenJarvis is a first step toward establishing a core framework for personal AI on personal devices. As local models and consumer hardware become more performant, we need better infrastructure for running AI agents on-device, reducing reliance on cloud APIs while keeping your most personal data exactly where it belongs. OpenJarvis is open-source under Apache 2.0 because the tools for studying and building local-first AI should be available to everyone, and because these problems are too large for any single lab to solve alone.</p>

<p>If you are a researcher, developer, or user, we would love to get you involved.</p>

<ul>
<li><strong>Researchers:</strong> We see major opportunities for further research, including advances in local language models, efficient agent architectures, memory management for on-device systems, and learning approaches that improve with use while preserving privacy. OpenJarvis provides an evaluation harness spanning 30+ benchmarks, and we would love for you to use it to measure and push progress.</li>

<li><strong>Developers:</strong> We encourage you to build on top of OpenJarvis and help us understand where the real bottlenecks are. Which use cases matter most? Where does performance fall short? Please visit our <a href="https://github.com/open-jarvis/OpenJarvis">GitHub</a> to get started. We welcome PRs that expand the ecosystem.</li>

<li><strong>Users:</strong> Try it out and tell us what you think. Point it at your files, connect it to your messaging platforms, and let us know what works and what does not. The fastest way to get started is to run <code>pip install openjarvis</code>, then <code>jarvis init</code>. Join us on <a href="https://discord.gg/3DEGU5Ce">discord</a> and share your thoughts!</li>
</ul>

<p>The fastest way to get started:</p>

<pre style="background-color: #f5f5f5; padding: 16px; border-radius: 8px; overflow-x: auto;"><code># Install
git clone https://github.com/open-jarvis/OpenJarvis.git
cd OpenJarvis
uv sync                           # core framework
uv sync --extra server             # + FastAPI server

# Let it Rip!
jarvis init          # auto-detect hardware, recommend engine
jarvis doctor        # verify setup
jarvis ask "What is the capital of France?"
</code></pre>

<p>
💻 <a href="https://github.com/open-jarvis/OpenJarvis">GitHub</a> &nbsp;|&nbsp;
📖 <a href="https://open-jarvis.github.io/OpenJarvis/">Docs</a> &nbsp;|&nbsp;
💬 <a href="https://discord.gg/3DEGU5Ce">Discord</a>
</p>

<hr>

<h2>🙏 Acknowledgements</h2>

<p>We are grateful to <a href="https://ollama.com/">Ollama</a>, <a href="https://research.ibm.com/">IBM Research</a>, <a href="https://laudeinstitute.com/">Laude Institute</a>, <a href="https://marlowe.stanford.edu/">Stanford Marlowe</a>, <a href="https://hai.stanford.edu/">Stanford HAI</a>, <a href="https://cloud.google.com/">Google Cloud Platform</a>, <a href="https://lambdalabs.com/">Lambda Labs</a>, <a href="https://nlp.stanford.edu/">Stanford NLP</a>, and <a href="https://ai.stanford.edu/">Stanford AI Lab</a> for their generous support.</p>

<p>OpenJarvis is part of <a href="https://hazyresearch.stanford.edu/blog/2025-11-11-ipw">Intelligence Per Watt</a>, a research initiative studying the efficiency of on-device AI systems. The project is developed at <a href="https://hazyresearch.stanford.edu/">Hazy Research</a> and the <a href="https://scalingintelligence.stanford.edu/">Scaling Intelligence Lab</a> at Stanford SAIL.</p>

<h3>Full Author List</h3>

<p>Jon Saad-Falcon*, Avanika Narayan*, Hakki Orhun Akengin, Herumb Shandilya, Robby Manihani, Gabriel Bo, John Hennessy, Christopher Ré, Azalia Mirhoseini</p>

</div>
