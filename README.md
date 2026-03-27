# AI Trust Evaluation - Claude Code Skill

An **interactive diagnostic skill** for evaluating and designing trust/accuracy frameworks for AI systems that extract, synthesize, or generate intelligence from uncontrolled sources (open web, public data, multi-source RAG).

Unlike a static reference doc, this skill **actively diagnoses your specific situation** — it scans your codebase for trust-relevant signals, asks targeted questions about your product and users, then produces a tailored action plan with only the strategies that apply to you.

## How It Works

```
Phase 0: Auto-Detect     → Scans your project for trust-relevant code, existing
                            mechanisms, and product signals (web extraction, RAG,
                            LLM generation, dashboard UI, etc.)

Phase 1: Understand       → Asks 3-5 targeted questions (skipping what Phase 0
                            already answered) about your product, users, and
                            current trust mechanisms

Phase 2: Diagnose         → Maps your situation against a 39-issue registry and
                            17-strategy taxonomy. Identifies YOUR specific failure
                            modes, not generic ones.

Phase 3: Recommend        → Produces a phased roadmap with quick wins, what to
                            build, what NOT to build, and which research papers
                            apply to your architecture

Phase 4: Deep Dive        → Optional: implementation details for any strategy,
                            or stress-test via agent-review-panel
```

## When to Use

- Users don't trust AI-generated outputs and citations alone aren't sufficient
- Classic RAG grounding techniques don't apply (corpus is the open web)
- Designing confidence scoring, verification, or trust UX for an AI product
- Evaluating whether a proposed trust framework will actually work
- Someone asks "how do we make users trust our AI outputs?"
- Building evaluation pipelines for web-extracted intelligence

## What It Covers

### Core Framework
- **The Three Laws of Trust** — accountability, track record, simplicity (trust is relational, not just informational)
- **The Inversion Principle** — detect when wrong (tractable) vs prove when right (unbounded)
- **17-Strategy Taxonomy** across 3 layers: backend intelligence, trust UX, continuous calibration
- **39-Issue Registry** of common pitfalls including source independence, circular sourcing, cognitive overload
- **Failure Mode Analysis** — the most dangerous failure is when false claims get the highest confidence scores
- **Persona-Based Design** — executive (one score), consultant (exportable proof), analyst (full transparency)

### Research-Backed Implementation Pipeline (New in v2.0)
A 7-step pipeline grounded in 25+ academic papers:

1. **Claim Decomposition** — break outputs into atomic, verifiable claims ([FActScore](https://arxiv.org/abs/2305.14251), [Molecular Facts](https://arxiv.org/abs/2406.20317), [Adaptive Decomposition](https://arxiv.org/abs/2501.00085))
2. **Per-Claim Verification** — search-augmented fact-checking for each claim ([SAFE pipeline](https://arxiv.org/abs/2403.18802) from Google DeepMind)
2.5. **Fast-Pass Hallucination Detection** — lightweight first-pass filtering using [HHEM](https://huggingface.co/vectara/hallucination_evaluation_model) (Vectara) or [Lynx](https://arxiv.org/abs/2407.08488) (Patronus AI, beats GPT-4 on HaluBench). Use [DeepEval](https://github.com/confident-ai/deepeval) (14.2k ⭐) or [RAGAS v0.4+](https://github.com/explodinggradients/ragas) (13.1k ⭐) for scored evaluation with reasoning.
3. **Source Independence Detection** — near-duplicate clustering + citation graph analysis to avoid counting syndicated content as corroboration ([MAFC](https://arxiv.org/abs/2305.13281))
4. **Corroboration Scoring** — credibility-weighted aggregation across truly independent sources
5. **Uncertainty Quantification** — semantic entropy to detect hallucinations without ground truth ([Semantic Entropy](https://www.nature.com/articles/s41586-024-07421-0), Nature 2024)
6. **Behavioral Consistency** — reference-free trust signal via multi-run stability ([TrustBC](https://arxiv.org/abs/2404.13782))
7. **Adversarial Robustness** — defend against the [Fact-Saboteurs](https://www.usenix.org/conference/usenixsecurity23/presentation/bao) taxonomy of attacks (USENIX Security 2023)

### New in v5.0.0

- **Progressive disclosure architecture** — SKILL.md is now a lean 216-line process document; knowledge base extracted to `references/` files loaded on-demand per phase
- **Improved trigger accuracy** — precision 81.8%, recall 90.0% (up from 73.9%/85.0%) with stronger negative boundaries and natural-language trigger phrases
- **Error handling & handoff points** — explicit behavior for minimal input, out-of-scope requests, and handoffs to agent-review-panel and deep-research
- **Schliff-benchmarked** — composite score 80.4/100 across 7 dimensions, 34/38 eval assertions passing. Improved with [Schliff](https://github.com/Zandereins/schliff), the disciplined skill improvement framework

### v4.0.0

- **Tiered Evaluation Pipeline** — 4-tier cost/thoroughness routing: HHEM fast-pass → DeepEval/RAGAS scored → RefChecker granular → ChainPoll/Semantic Entropy high-confidence
- **DeepEval integration** (14.2k ⭐) — 30+ metrics, Pytest-native CI/CD, debuggable LLM judge reasoning
- **Vectara Hallucination Leaderboard** — model selection based on hallucination rates across 100+ models
- **Lynx** — first open-source model beating GPT-4 on hallucination detection (HaluBench)
- **RefChecker** — knowledge-triplet verification for pinpointing specific hallucinated claims
- **ChainPoll** — multi-sample CoT voting for high-confidence detection
- **HELM benchmarks** — holistic model evaluation for trust-sensitive applications
- **RAGAS v0.4+** — updated coverage including multimodal, agent, and multi-turn evaluation

### Competitive Landscape
No existing Claude Code skill or tool provides a general-purpose AI trust evaluation framework. This skill uniquely combines claim-level verification, trust UX design, and continuous calibration into a single reusable framework.

### Open-Source Tools Referenced
[FActScore](https://github.com/shmsw25/FActScore) | [SAFE](https://github.com/google-deepmind/long-form-factuality) | [RAGAS v0.4+](https://github.com/explodinggradients/ragas) (13.1k ⭐) | [DeepEval](https://github.com/confident-ai/deepeval) (14.2k ⭐) | [Lynx](https://huggingface.co/PatronusAI/Llama-3-Patronus-Lynx-70B-Instruct) | [RefChecker](https://github.com/amazon-science/RefChecker) | [HHEM](https://huggingface.co/vectara/hallucination_evaluation_model) | [HELM](https://github.com/stanford-crfm/helm) (2.7k ⭐) | [Vectara Leaderboard](https://github.com/vectara/hallucination-leaderboard) (3.2k ⭐)

## Example: What a Session Looks Like

Here's a real interaction showing how the skill adapts to a specific product (a web intelligence tool for consultants):

<details>
<summary><b>Phase 0: Auto-Detect</b> — scans your project before asking anything</summary>

> I scanned your project and found:
> - **Web extraction signals**: `httpx`, `beautifulsoup`, `extract`, `crawl` detected
> - **LLM generation**: `anthropic`, `claude`, `prompt` in pipeline code
> - **Dashboard UI**: React components in `src/dashboard/`
> - **Existing trust mechanisms**: `source_url` field in API responses, citation links in UI
>
> It looks like you're extracting from the web via LLM, with citations already in place.
> Source independence and syndication detection are likely your biggest gaps. Let me
> confirm a few things...
</details>

<details>
<summary><b>Phase 1: Targeted Questions</b> — only asks what it couldn't figure out</summary>

> **What output types do users consume?**
> - (a) Hard facts (revenue, HQ, founding year)
> - (b) Synthesized insights (competitive positioning, market trends)
> - (c) Recommendations or predictions
> - (d) Mix — roughly what %?

*User: "Mix — 30% hard facts, 60% synthesized, 10% predictions"*

> **Who are the primary users?**

*User: "Consultants who present findings to their clients"*
</details>

<details>
<summary><b>Phase 2: Tailored Diagnosis</b> — maps YOUR situation to the issue registry</summary>

> ### Your Top 5 Failure Modes (from the 39-issue registry)
>
> 1. **Syndication inflation** (Critical) — Your web extraction pipeline pulls from
>    news sites that syndicate press releases. "Confirmed by 5 sources" may really
>    be 1 press release reprinted 5 times.
>
> 2. **Streetlight effect** (High) — Authoritative DB cross-ref only covers your 30%
>    hard facts. The 60% synthesized insights — where consultants need trust most —
>    have no verification pathway.
>
> 3. **Export gap** (High) — Consultants need trust metadata in PowerPoint/PDF for
>    client presentations. Dashboard-only trust signals disappear when exported.
>
> 4. **[2 more specific to their architecture...]**
>
> ### Strategy Fit for Your Product
>
> | Strategy | Fit | Why |
> |----------|-----|-----|
> | "What We Don't Know" section | **High** | Consultants presenting to clients NEED to show gaps |
> | Source independence detection | **High** | Web extraction = syndication problem |
> | Semantic entropy | **High** | Your LLM synthesis step is where hallucinations enter |
> | Provenance graph | **Low** | Consultants won't click through — they need exportable proof |
> | Confidence decay | **Low** | Reports are point-in-time deliverables, not live dashboards |
</details>

<details>
<summary><b>Phase 3: Tailored Roadmap</b> — what to build, what to skip</summary>

> ### Quick Wins (ship this sprint)
> 1. **"What We Don't Know" section** — surface gaps per report. Consultants will
>    love this: "We could not find revenue data from the last 12 months" builds
>    credibility with their clients.
> 2. **Temporal freshness badges** — tag every claim with source date. Near-free.
>
> ### Phase 1: Consultant Trust Anchor
> - Verified badges for hard facts (SEC/Crunchbase cross-ref)
> - Export-friendly trust metadata (badges, freshness, gaps survive PDF export)
> - Composite trust score per insight section
>
> ### Phase 2: Synthesized Insight Verification
> - Source independence detection (embedding clustering before corroboration)
> - SAFE pipeline for per-claim verification of synthesized insights
> - Semantic entropy on LLM synthesis step as "likely wrong" detector
>
> ### What NOT to Build
> - **Provenance graph** — low ROI for consultant workflow
> - **Confidence decay** — irrelevant for point-in-time reports
> - **User correction loop** — <2% adoption without incentives; defer
</details>

## Key Insights

> "The framework's most dangerous failure mode is success." — A false claim from syndicated sources receives the highest confidence score, causing users to trust wrong outputs *more* than without the framework.

> "Instead of proving claims correct (unbounded), detect when the system is likely wrong (tractable)." — The Inversion Principle

> "Trust is built through accountability, track record, and simplicity — not through 17 layers of epistemological metadata."

## Installation

### Claude Code

**Option 1: Plugin install (recommended)**
```bash
/plugin marketplace add wan-huiyan/ai-trust-evaluation
/plugin install ai-trust-evaluation@wan-huiyan-ai-trust-evaluation
```

**Option 2: Git clone**
```bash
git clone https://github.com/wan-huiyan/ai-trust-evaluation.git ~/.claude/skills/ai-trust-evaluation
```

### Cursor

Cursor supports skills via `~/.cursor/skills/` (Cursor 2.4+), though global discovery can be flaky. Options from most to least reliable:

**Option 1: Per-project rule (most reliable)**
```bash
mkdir -p .cursor/rules
# Create .cursor/rules/ai-trust-evaluation.mdc with the content of SKILL.md
# Add frontmatter: alwaysApply: true
```

**Option 2: npx skills CLI**
```bash
npx skills add wan-huiyan/ai-trust-evaluation --global
```

**Option 3: Manual global install**
```bash
git clone https://github.com/wan-huiyan/ai-trust-evaluation.git ~/.cursor/skills/ai-trust-evaluation
```

## Origin

This skill was extracted from a structured brainstorming session + 5-agent adversarial review panel using the [Agent Review Panel](https://github.com/wan-huiyan/agent-review-panel) skill (Feasibility Analyst, Stakeholder Advocate, Risk Assessor, Statistical Rigor Reviewer, Devil's Advocate) with completeness audit and supreme judge arbitration. The review methodology draws on [ChatEval](https://arxiv.org/abs/2308.07201) (ICLR 2024), [AutoGen](https://github.com/microsoft/autogen), [Du et al.](https://arxiv.org/abs/2305.14325) (ICML 2024), [MachineSoM](https://arxiv.org/abs/2305.11846) (ACL 2024), and [DebateLLM](https://arxiv.org/abs/2311.09763).

v2.0 was enhanced through deep research across 25+ academic papers and competitive analysis of 15 existing tools:

| Paper | Venue | Contribution |
|-------|-------|-------------|
| [FActScore](https://arxiv.org/abs/2305.14251) | EMNLP 2023 | Atomic fact evaluation framework |
| [SAFE](https://arxiv.org/abs/2403.18802) | NeurIPS 2024 | Search-augmented factual evaluation (Google DeepMind) |
| [Semantic Entropy](https://www.nature.com/articles/s41586-024-07421-0) | Nature 2024 | Uncertainty quantification without ground truth |
| [Molecular Facts](https://arxiv.org/abs/2406.20317) | 2024 | Decontextualized claim decomposition |
| [MAFC](https://arxiv.org/abs/2305.13281) | 2026 | Multi-agent fact-checking with credibility weighting |
| [TrustBC](https://arxiv.org/abs/2404.13782) | 2024 | Behavioral consistency as reference-free trust signal |
| [Fact-Saboteurs](https://www.usenix.org/conference/usenixsecurity23/presentation/bao) | USENIX Security 2023 | Adversarial attack taxonomy for fact-checking |
| [ELM Trust UX](https://doi.org/10.1108/INTR-2024-ELM) | Emerald 2025 | Persona-based trust interface design |
| [DeepEval](https://github.com/confident-ai/deepeval) | 2024 | 30+ LLM evaluation metrics with Pytest-native CI/CD |
| [Lynx](https://arxiv.org/abs/2407.08488) | 2024 | Open-source hallucination detector beating GPT-4 |
| [RefChecker](https://arxiv.org/abs/2405.14486) | 2024 | Knowledge-triplet verification (Amazon Science) |
| [ChainPoll](https://arxiv.org/abs/2310.18344) | 2023 | Multi-sample CoT hallucination detection |
| [HELM](https://github.com/stanford-crfm/helm) | 2022+ | Holistic model evaluation (Stanford CRFM) |
| [Vectara HHEM](https://github.com/vectara/hallucination-leaderboard) | 2024+ | Hallucination leaderboard: 100+ models, 7.7k articles |

## Version History

| Version | Changes |
|---------|---------|
| 5.0.0 | Progressive disclosure (SKILL.md 533→216 lines), `references/` extraction, improved triggers (precision 82%, recall 90%), error handling, [Schliff](https://github.com/Zandereins/schliff)-benchmarked at 80.4/100 |
| 4.0.0 | Tiered evaluation pipeline, DeepEval, HHEM/Vectara leaderboard, Lynx, RefChecker, ChainPoll, HELM, RAGAS v0.4+ |
| 3.0.0 | Research-backed 7-step pipeline, 25+ papers, adaptive decomposition, molecular facts |
| 2.0.0 | Research-backed implementation pipeline, semantic entropy, adversarial robustness |
| 1.0.0 | Initial release: trust taxonomy, 17 strategies, 39-issue registry |

## License

MIT
