# Changelog

All notable changes to this project will be documented in this file.

## [5.0.0] - 2026-03-25

- **Progressive disclosure architecture** — SKILL.md is now a lean 216-line process document; knowledge base extracted to `references/` files loaded on-demand per phase
- **Improved trigger accuracy** — precision 81.8%, recall 90.0% (up from 73.9%/85.0%) with stronger negative boundaries and natural-language trigger phrases
- **Error handling & handoff points** — explicit behavior for minimal input, out-of-scope requests, and handoffs to agent-review-panel and deep-research
- **Schliff-benchmarked** — composite score 80.4/100 across 7 dimensions. Improved with [Schliff](https://github.com/Zandereins/schliff), the disciplined skill improvement framework

## [4.0.0]

- **Tiered Evaluation Pipeline** — 4-tier cost/throughness routing: HHEM fast-pass, DeepEval/RAGAS scored, RefChecker granular, ChainPoll/Semantic Entropy high-confidence
- **DeepEval integration** — 30+ metrics, Pytest-native CI/CD, debuggable LLM judge reasoning
- **Vectara Hallucination Leaderboard** — model selection based on hallucination rates across 100+ models
- **Lynx** — first open-source model beating GPT-4 on hallucination detection (HaluBench)
- **RefChecker** — knowledge-triplet verification for pinpointing specific hallucinated claims
- **ChainPoll** — multi-sample CoT voting for high-confidence detection
- **HELM benchmarks** — holistic model evaluation for trust-sensitive applications
- **RAGAS v0.4+** — updated coverage including multimodal, agent, and multi-turn evaluation

## [3.0.0]

- **Research-backed 7-step pipeline** grounded in 25+ academic papers
- **Adaptive decomposition** and **molecular facts** for claim-level verification

## [2.0.0]

- **Research-backed implementation pipeline** with semantic entropy and adversarial robustness
- Deep research across 25+ academic papers and competitive analysis of 15 existing tools

## [1.0.0]

- Initial release: trust taxonomy, 17 strategies, 39-issue registry
