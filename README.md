# Awesome AI Agent Evals

A curated list of benchmarks, harnesses, grading methods, observability tools, safety evals, leaderboards, papers and guides for **evaluating AI agents**: systems that use tools, browse, write code, and operate computers over many steps. Agents fail in ways a single-turn LLM eval can't see. Errors compound across turns, success depends on the final state of an environment and not on the text of a reply, and scores can swing with the scaffold, the budget and noise between runs. Every entry links to its primary source (paper, official repo, or official docs). Entries are agent-focused, maintained or citable, and checked for current versions. Where a benchmark has a successor, the list points to the current one.

## Contents

- [Agent benchmarks](#agent-benchmarks)
  - [Coding](#coding)
  - [Web & browser](#web-browser)
  - [Tool use & function calling](#tool-use-function-calling)
  - [Computer use & OS](#computer-use-os)
  - [Multi-agent & long-horizon](#multi-agent-long-horizon)
- [Eval frameworks & harnesses](#eval-frameworks-harnesses)
- [LLM-as-judge & grading](#llm-as-judge-grading)
- [Tracing & observability](#tracing-observability)
- [Safety & red-teaming evals](#safety-red-teaming-evals)
- [Leaderboards](#leaderboards)
- [Key papers](#key-papers)
- [Guides](#guides)
- [Contributing](#contributing)
- [License](#license)

## Agent benchmarks

### Coding

- [Android Bench 2.0](https://developer.android.com/bench/methodology/2) - Google's benchmark for models and coding agents on Android development. Version 2.0 (Sep 2026) adds 30 multi-day tasks (app creation, library migrations, platform features, cross-platform conversions) on a private dataset, scored by pass rate and a continuous completion rate, with an LLM visual judge for UI; scores aren't comparable with the 1.0 task set.
- [HPCAgent-Bench](https://github.com/spcl/HPCAgent-Bench) - 689 kernels across loop-level optimization, HPC codes, and ML models from ETH Zurich's SPCL (the repo now lists about 720), run on CPUs or GPUs and checked by an external judge on hidden fuzzed inputs. Paired runs isolate what a harness, tool, or skill adds, and results report weighted token cost. ([paper](https://arxiv.org/abs/2610.20673))
- [MLE-bench](https://arxiv.org/abs/2410.07095) - 75 Kaggle competitions graded against human medal thresholds; measures end-to-end ML engineering by agents, not just code edits. Its official leaderboard paused submissions in Apr 2026 pending a v2.
- [ProgramBench](https://arxiv.org/abs/2605.03546) - 200 tasks where an agent gets only a compiled program and its docs and must rebuild it from scratch, graded by hidden fuzzing-generated behavioral tests; measures whole-project architecture rather than single-issue fixes.
- [SlopCodeBench](https://arxiv.org/abs/2603.24755) - 36 problems and 196 checkpoints where a coding agent must keep extending its own earlier code as the spec grows, with hidden tests that carry prior checkpoints forward, plus structural-erosion and verbosity scores. ([code](https://github.com/SprocketLab/slop-code-bench))
- [SWE-bench](https://github.com/SWE-bench/SWE-bench) - Resolve real GitHub issues so hidden tests pass; the founding repo-level coding benchmark. OpenAI [stopped reporting the Verified subset in Feb 2026](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) over contamination and test flaws, so treat Verified scores as historical.
- [SWE-Bench Pro V2](https://arxiv.org/abs/2610.24206) - Long-horizon, multi-file enterprise tasks; V2 (Oct 2026) ships 642 repaired tasks plus a 51-task Hard split, following a [July 2026 OpenAI audit](https://openai.com/index/separating-signal-from-noise-coding-evaluations/) that estimated ~30% of the [original paper](https://arxiv.org/abs/2509.16941)'s public tasks were broken; V1 and V2 scores aren't comparable.
- [SWE-rebench](https://arxiv.org/abs/2505.20411) - Continuously refreshed, decontaminated SWE tasks mined from new GitHub activity and run in one standardized scaffold; it guards against training-set leakage.
- [Terminal-Bench](https://github.com/harbor-framework/terminal-bench) - Hard, human-reviewed tasks in Docker containers (sysadmin, ML, security, science, business) run via Harbor; the de facto standard for terminal/CLI agents. Continuously versioned (4.0, Aug 2026, 66 tasks); scores aren't comparable across major versions. ([2.0 paper](https://arxiv.org/abs/2601.11868))
- [Terminal-Bench-Science](https://github.com/harbor-framework/terminal-bench-science) - Expert-written Harbor tasks from working scientists across the life, physical, earth, mathematical, and engineering sciences, including signal reconstruction and formal proofs, graded by deterministic tests. The tagged release is still 0.1 (Aug 2026); the repo currently lists 73 tasks.

### Web & browser

- [BrowseComp](https://arxiv.org/abs/2504.12516) - 1,266 hard-to-find but easy-to-verify questions that require persistent, creative web browsing; a staple for deep-research agents.
- [DynaRev](https://arxiv.org/abs/2610.40133) - 181 web tasks, built from Mind2Web, where the goal is revised mid-run (additive edits, overrides, and runtime-conditioned overrides) at semantic milestones; scores correctness, responsiveness, and efficiency from the trajectory. ([code](https://github.com/zsq259/DynaRev))
- [Mind2Web 2](https://arxiv.org/abs/2506.21506) - 130 long-horizon, time-varying agentic-search tasks graded by task-specific Agent-as-a-Judge rubrics, including source attribution.
- [WebArena-Verified](https://github.com/ServiceNow/webarena-verified) - Audited rerelease of WebArena's 812 self-hosted web tasks with deterministic evaluators and a hard subset; use this rather than the original WebArena scores.
- [WorkArena](https://github.com/ServiceNow/WorkArena) - Enterprise knowledge-work tasks on a live ServiceNow instance (WorkArena and WorkArena++); tests web agents on realistic multi-step business workflows.

### Tool use & function calling

- [AppWorld](https://arxiv.org/abs/2407.18901) - 9 simulated apps with 457 APIs, with state-based unit tests that check collateral damage; measures interactive coding agents on day-to-day tasks.
- [Berkeley Function Calling Leaderboard (BFCL) V4](https://github.com/ShishirPatil/gorilla/tree/main/berkeley-function-call-leaderboard) - Function-calling accuracy across single, parallel and multi-turn calls, plus V4's agentic web-search and memory tasks; checked by AST and execution.
- [MCP-Universe](https://arxiv.org/abs/2508.14704) - Tasks against real Model Context Protocol servers (maps, GitHub, finance, browser and more) with execution-based evaluators; measures agents working through live MCP tools.
- [Toolathlon](https://arxiv.org/abs/2510.25726) - 108 long-horizon tasks across 32 real applications and 600+ tools (mostly MCP servers), starting from realistic environment states and checked by execution scripts.
- [τ-bench (tau-bench)](https://github.com/sierra-research/tau2-bench) - Simulated user, tools and a domain policy (airline, retail, telecom, banking, voice); its pass^k metric measures how reliable an agent is across repeated trials. Current releases are τ³-bench and v2.0, which is multilingual. Scores aren't comparable across major versions.

### Computer use & OS

- [AndroidWorld](https://github.com/google-research/android_world) - 116 parameterized tasks across 20 real Android apps with reward signals checked from device state; the reference benchmark for mobile agents.
- [OSWorld 2.0](https://arxiv.org/abs/2606.29537) - 108 long-horizon desktop workflows (median about 1.6 human-hours) with dynamic mid-task changes and checkpoint scoring. It succeeds OSWorld and OSWorld-Verified, and scores aren't comparable with them.
- [Windows Agent Arena](https://github.com/microsoft/WindowsAgentArena) - 150+ tasks in a real Windows OS VM, parallelizable in Azure; covers Windows-native computer-use agents.

### Multi-agent & long-horizon

- [Agents' Last Exam](https://github.com/rdi-berkeley/agents-last-exam) - Living benchmark of 1,000+ expert-sourced professional workflows across 55 subfields in 13 industry clusters, mostly mixing GUI and CLI work, graded against expert deliverables and milestone checks. Its hardest tier is far from saturated; public v1.1 selects 122 active tasks after 30 retirements. ([paper](https://arxiv.org/abs/2606.05405))
- [FrontierCS 2](https://arxiv.org/abs/2610.29932) - 48 CS R&D tasks across 13 areas. Each task deletes a published paper's contribution from the authors' released code, and a run passes only if the agent beats that original on every held-out metric and workload.
- [Gaia2](https://arxiv.org/abs/2602.11964) - Successor to GAIA, built on Meta's Agents Research Environments (ARE). Tests dynamic, asynchronous, time-sensitive scenarios with verifiable write actions and multi-agent collaboration.
- [GDPval](https://arxiv.org/abs/2510.04374) - Expert-graded deliverables from 44 occupations, compared against industry professionals' work; measures economically valuable, real-world task performance.
- [LHTB 2.0](https://arxiv.org/abs/2610.14146) - 77 containerized terminal tasks across 13 domains, run for three hours with verifier checkpoints every 30 minutes and dense scores for partial progress. It extends the 46-task, 90-minute original, and scores aren't comparable.
- [MultiAgentBench](https://arxiv.org/abs/2503.01935) - MARBLE framework scenarios that score both task completion and the quality of collaboration and competition across coordination topologies.
- [PaperBenchX](https://arxiv.org/abs/2610.39744) - 93 expert tasks across 12 areas (electromagnetics, photonics, chemistry, materials, biology, robotics) where an agent must reproduce a paper's experiment and a clean replay of its workflow is what gets graded. Twelve tasks are public; 81 are held out. ([code](https://github.com/UniPat-AI/PaperBenchX))
- [TheAgentCompany](https://arxiv.org/abs/2412.14161) - A simulated software company (GitLab, RocketChat, Plane, OwnCloud) with 175 consequential multi-step workplace tasks and checkpoint-based partial credit.
- [Vending-Bench 2](https://andonlabs.com/evals/vending-bench-2) - An agent runs a simulated vending business for a year against adversarial suppliers; measures long-term coherence by final bank balance. The original Vending-Bench paper is arXiv:2502.15840.

## Eval frameworks & harnesses

- [BrowserGym](https://github.com/ServiceNow/BrowserGym) - A unified Gym environment for web agents that wraps WebArena, VisualWebArena, WorkArena, MiniWoB and more; pair it with AgentLab to run experiments.
- [DeepEval](https://github.com/confident-ai/deepeval) - Pytest-style open-source framework with agent metrics (task completion, tool correctness, plan adherence) and component-level tracing.
- [EGI-Bench](https://arxiv.org/abs/2610.27234) - Salesforce suite that runs eight enterprise agent benchmarks behind one CLI and routes their sub-metrics into a six-part Enterprise General Intelligence score (outcome, tools, planning, grounding, safety, and engineering quality).
- [Harbor](https://github.com/harbor-framework/harbor) - Containerized framework from the Terminal-Bench team for running agents (Claude Code, Codex CLI, custom) against benchmark datasets at scale; it is the official way to run Terminal-Bench.
- [Inspect](https://inspect.aisi.org.uk/) - UK AISI's open-source eval framework with first-class agents, tool use, sandboxing (Docker/K8s), external agent support and a log viewer; widely used by safety institutes and METR.
- [Inspect Evals](https://github.com/UKGovernmentBEIS/inspect_evals) - Community collection of ready-to-run Inspect implementations of agentic, coding, cyber and safety benchmarks, plus an external eval register.
- [Promptfoo](https://github.com/promptfoo/promptfoo) - Open-source (MIT) CLI and library for declarative evals and red-teaming of prompts, agents and RAG in CI; it is OpenAI's recommended migration path from its retired Evals product.
- [TraceTrial-Bench](https://arxiv.org/abs/2610.32285) - 95 tasks that grade harness reliability (context, execution, governance, lifecycle, observability, tools, verification) separately from task success, using native traces and environment evidence. In the paper, 31.1% of runs succeed while failing a harness criterion. ([code](https://github.com/TraceLite-AI/TraceTrial-Bench))

## LLM-as-judge & grading

- [Agent-as-a-Judge](https://arxiv.org/abs/2410.10934) - Uses agentic judges that inspect intermediate steps and artifacts, not just final outputs, and introduces the DevAI benchmark of judged requirements.
- [AgentRewardBench](https://arxiv.org/abs/2504.08942) - 1,302 expert-annotated web-agent trajectories for measuring how well LLM judges assess success, side effects and repetition.
- [G-Eval](https://arxiv.org/abs/2303.16634) - Rubric-based chain-of-thought scoring with probability-weighted scores; the template behind many "custom criteria" judges in eval tools.
- [Gaming the Judge](https://arxiv.org/abs/2601.14691) - Shows that rewriting an agent's chain-of-thought while keeping its actions fixed inflates judge false positives by 20-30+ points; argues for evidence-only trajectory grading.
- [Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685) - The foundational study of LLM judges, measuring agreement with humans and documenting position, verbosity and self-enhancement biases.

## Tracing & observability

- [Arize Phoenix](https://github.com/Arize-ai/phoenix) - Source-available (ELv2), free to self-host; OpenTelemetry/OpenInference-based tracing with evals, datasets and experiments.
- [Docent](https://github.com/TransluceAI/docent) - Apache-2.0, self-hostable platform from Transluce for searching, clustering and rubric-scoring large collections of agent transcripts.
- [Langfuse](https://github.com/langfuse/langfuse) - Open-source (MIT core), self-hostable tracing for agent runs with LLM-as-judge evaluators, datasets, experiments and human annotation queues.
- [MLflow](https://mlflow.org/docs/latest/genai/) - Apache-2.0 platform with OpenTelemetry-compatible agent tracing, LLM judges/scorers and evaluation datasets.
- [OpenLLMetry](https://github.com/traceloop/openllmetry) - Apache-2.0 OpenTelemetry instrumentations for LLM providers, vector DBs and agent frameworks; exports agent traces to any OTel backend.
- [OpenTelemetry GenAI semantic conventions](https://github.com/open-telemetry/semantic-conventions-genai) - The emerging standard for spans, metrics and events covering LLM calls, agents, tool calls and MCP, now in its own repo. It makes traces portable across eval tools.
- [Opik](https://github.com/comet-ml/opik) - Apache-2.0 tracing, evaluation and monitoring for agentic workflows, with built-in judge metrics and experiment tracking; self-hostable.
- [Transect](https://github.com/AI-Safety-Institute/transect) - UK AISI's MIT-licensed tool, built on Inspect Scout, that turns long agent transcripts (including sub-agent delegation) into labeled, interactive timelines linked back to source turns.

## Safety & red-teaming evals

- [AgentDojo](https://github.com/ethz-spylab/agentdojo) - Dynamic environment with realistic tool-use tasks plus prompt-injection attacks and defenses; measures utility and attack success together.
- [AgentHarm](https://arxiv.org/abs/2410.09024) - 110 explicitly malicious multi-step agent tasks (440 with augmentations) across 11 harm categories; measures harmful compliance and jailbreak robustness in tool-using agents.
- [Cybench](https://arxiv.org/abs/2408.08926) - 40 professional CTF tasks with subtasks for measuring offensive cyber capabilities; used in frontier-model dangerous-capability evaluations.
- [CyberGym](https://arxiv.org/abs/2506.02548) - 1,507 real OSS-Fuzz vulnerabilities across 188 projects; the agent must write a proof of concept that crashes the unpatched build but not the patched one.
- [garak](https://github.com/NVIDIA/garak) - NVIDIA's open-source LLM vulnerability scanner with probes for prompt injection, jailbreaks, data leakage and more.
- [ImpossibleBench](https://arxiv.org/abs/2510.20270) - Impossible variants of LiveCodeBench and SWE-bench tasks where spec and tests conflict, so any pass is a shortcut; measures how often coding agents cheat on tests.
- [OverEager-Bench](https://arxiv.org/abs/2605.18583) - 500 benign coding tasks (about 7,500 runs on four agent frameworks and six base models) scored for out-of-scope actions; the framework shifts those rates more than the base model.
- [Petri](https://github.com/meridianlabs-ai/inspect_petri) - Open-source alignment-auditing agent: an auditor model runs multi-turn scenarios with simulated tools against a target model and a judge scores transcripts against a rubric. Built on Inspect; started at Anthropic, now maintained by Meridian Labs.
- [PyRIT](https://github.com/microsoft/PyRIT) - Microsoft's open-source framework for automated, multi-turn red teaming of generative AI systems, including agentic targets.
- [RepliBench](https://arxiv.org/abs/2504.18565) - UK AISI's 20 task families (86 tasks) measuring the components of autonomous replication: obtaining resources, exfiltrating weights, replicating onto compute, and persisting.
- [SandboxEscapeBench](https://arxiv.org/abs/2603.02277) - UK AISI Inspect benchmark of 18 container-escape scenarios across orchestration, runtime, and kernel layers, run in a sandbox-within-a-sandbox so a breakout stays inside an outer VM.

## Leaderboards

- [BFCL V4 Leaderboard](https://gorilla.cs.berkeley.edu/leaderboard.html) - Run by the benchmark authors (UC Berkeley); reports function-calling and agentic scores with cost and latency.
- [Epoch AI Benchmarking Hub](https://epoch.ai/benchmarks) - Independent, reproducible runs of frontier models on agentic and reasoning benchmarks, with methodology and data released.
- [METR Time Horizons](https://metr.org/time-horizons/) - Independent measurements of the length of software tasks (in human time) frontier models complete at 50% and 80% success (Time Horizon 1.1). No longer updated since Sep 2026 because the task suite saturates at long horizons; treat as a historical reference.
- [SWE-rebench Leaderboard](https://swe-rebench.com/) - Monthly decontaminated coding-agent results in a fixed scaffold, with cost and contamination flags.
- [Terminal-Bench Leaderboard](https://www.tbench.ai/) - Official leaderboard for the current Terminal-Bench version (4.0), run by the benchmark team, with 95% confidence intervals per agent/model pair.
- [τ-bench Leaderboard](https://taubench.com/) - Official Sierra leaderboard for τ-bench text, voice and knowledge domains, including pass^k reliability.

## Key papers

- [Adding Error Bars to Evals](https://arxiv.org/abs/2411.00640) - A statistical approach to eval results: standard errors, clustering, and paired comparisons. Essential when agent runs are noisy and task counts are small.
- [Agents Are Systems, Not Models](https://arxiv.org/abs/2610.01618) - Controlled comparison of agent configuration on four scientific coding tasks: what the agent is told changes results more than the time budget or the model size, and telling it to verify its answer barely changes verification behavior, while a verification tool does. ([code](https://github.com/lusxvr/rethinking-agent-evaluation))
- [AI Agents That Matter](https://arxiv.org/abs/2407.01502) - Argues for cost-controlled, reproducible agent evaluation and shows how benchmark shortcuts and overfitting inflate agent results.
- [Emerging Trends in AI Agent Evaluation](https://sage.cs.princeton.edu/trends/) - Survey of 33 shifts in agent evaluation (validity, statistics, contamination, log analysis, field studies, the evaluator ecosystem) with best practices and open problems.
- [Establishing Best Practices for Building Rigorous Agentic Benchmarks](https://arxiv.org/abs/2507.02825) - The Agentic Benchmark Checklist (ABC), which catalogs task-validity and outcome-validity failures in popular benchmarks and how to avoid them.
- [Finding the Right Fit: Model-Harness Interactions across Agent Tasks](https://arxiv.org/abs/2610.00917) - 66 model-harness pairings on three agent benchmarks: rankings reverse when the harness changes, and for most models the best harness changes with the task.
- [Holistic Agent Leaderboard](https://arxiv.org/abs/2510.11977) - 21,730 standardized rollouts across models, scaffolds and benchmarks; documents cost-accuracy tradeoffs and agent shortcut behaviors (ICLR 2026).
- [Log Analysis Is Necessary for Credible Evaluation of AI Agents](https://arxiv.org/abs/2605.08545) - Taxonomy of validity threats only logs reveal (shortcuts, scaffold artifacts, hidden dangerous actions), with principles for log analysis, illustrated on τ-bench Airline.
- [Measuring AI Ability to Complete Long Tasks](https://arxiv.org/abs/2503.14499) - METR's time-horizon methodology, which ties task success to human completion time to track how agent capability grows over time.
- [Survey on Evaluation of LLM-based Agents](https://arxiv.org/abs/2503.16416) - A comprehensive survey of agent evaluation: capabilities, application-specific benchmarks, generalist benchmarks, frameworks, and open gaps.

## Guides

- [AI Evals: Everything You Need to Know](https://hamel.dev/blog/posts/evals-faq/) - Practitioner FAQ on error analysis, judge calibration, and evaluating multi-turn agent traces and tool calls.
- [Building a more secure environment for evaluating dangerous capabilities](https://www.aisi.gov.uk/blog/building-a-more-secure-environment-for-evaluating-dangerous-capabilities) - UK AISI's security changes after agents took out-of-scope actions in internet-enabled cyber evaluations (incident reported August 2026): no outbound internet, separate network blocks, and a monitor that can halt a run. Anthropic's companion post-mortem is its [alignment assessment of the UK AISI cybertesting incident](https://www.anthropic.com/research/alignment-assessment-ukaisi).
- [Demystifying evals for AI agents](https://www.anthropic.com/research/demystifying-evals-for-ai-agents) - Anthropic's practical playbook covering tasks, trials, graders, transcripts and outcomes, capability evals versus regression evals, and grading final state over prescribed paths.
- [Evaluating the Effectiveness of LLM-Evaluators](https://eugeneyan.com/writing/llm-evaluators/) - A well-sourced review of LLM-as-judge techniques, failure modes and how to validate a judge against human labels.
- [Securing our frontier model training environment](https://openai.com/index/securing-our-frontier-model-training-environment/) - How OpenAI contains frontier training and evaluation environments: no live internet, overlapping sandboxes and microVMs, and high-compute red-teaming of those boundaries before runs resume.
- [Separating signal from noise in coding evaluations](https://openai.com/index/separating-signal-from-noise-coding-evaluations/) - OpenAI's audit methodology for finding broken benchmark tasks (overly strict tests, underspecified prompts, low coverage); a worked example of validating an eval before trusting it.

## Contributing

Contributions are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) for the inclusion criteria and entry format before you open a PR.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
