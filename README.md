# AI Trust Evaluation

[![GitHub release](https://img.shields.io/github/v/release/wan-huiyan/ai-trust-evaluation)](https://github.com/wan-huiyan/ai-trust-evaluation/releases) [![Claude Code](https://img.shields.io/badge/Claude_Code-skill-orange)](https://claude.com/claude-code) [![license](https://img.shields.io/github/license/wan-huiyan/ai-trust-evaluation)](LICENSE) [![last commit](https://img.shields.io/github/last-commit/wan-huiyan/ai-trust-evaluation)](https://github.com/wan-huiyan/ai-trust-evaluation/commits)

Point this skill at your AI product and get a tailored trust roadmap — what to build, what to skip, and which failure modes will bite you.

Unlike a static checklist, this skill **actively diagnoses your situation** — it scans your codebase for trust-relevant signals, asks targeted questions, then produces a phased action plan with only the strategies that apply to you. Works for any system that pulls from the open web, RAG pipelines, or LLM-generated content.

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

## What It Covers

### Core Framework
- **The Three Laws of Trust** — accountability, track record, simplicity (trust is relational, not just informational)
- **The Inversion Principle** — detect when wrong (tractable) vs prove when right (unbounded)
- **17-Strategy Taxonomy** across 3 layers: backend intelligence, trust UX, continuous calibration
- **39-Issue Registry** of common pitfalls including source independence, circular sourcing, cognitive overload
- **Failure Mode Analysis** — the most dangerous failure is when false claims get the highest confidence scores
- **Persona-Based Design** — executive (one score), consultant (exportable proof), analyst (full transparency)

### Research-Backed Verification Pipeline

An 8-step pipeline grounded in 25+ academic papers:

1. **Claim Decomposition** — break outputs into atomic, verifiable claims ([FActScore](https://arxiv.org/abs/2305.14251), [Molecular Facts](https://arxiv.org/abs/2406.20317), [Adaptive Decomposition](https://arxiv.org/abs/2501.00085))
2. **Per-Claim Verification** — search-augmented fact-checking for each claim ([SAFE pipeline](https://arxiv.org/abs/2403.18802) from Google DeepMind)
3. **Fast-Pass Hallucination Detection** — lightweight first-pass filtering using [HHEM](https://huggingface.co/vectara/hallucination_evaluation_model) (Vectara) or [Lynx](https://arxiv.org/abs/2407.08488) (Patronus AI). Use [DeepEval](https://github.com/confident-ai/deepeval) or [RAGAS](https://github.com/explodinggradients/ragas) for scored evaluation with reasoning.
4. **Source Independence Detection** — near-duplicate clustering + citation graph analysis to avoid counting syndicated content as corroboration ([MAFC](https://arxiv.org/abs/2305.13281))
5. **Corroboration Scoring** — credibility-weighted aggregation across truly independent sources
6. **Uncertainty Quantification** — semantic entropy to detect hallucinations without ground truth ([Semantic Entropy](https://www.nature.com/articles/s41586-024-07421-0), Nature 2024)
7. **Behavioral Consistency** — reference-free trust signal via multi-run stability ([TrustBC](https://arxiv.org/abs/2404.13782))
8. **Adversarial Robustness** — defend against the [Fact-Saboteurs](https://www.usenix.org/conference/usenixsecurity23/presentation/bao) taxonomy of attacks (USENIX Security 2023)

### Open-Source Tools Referenced

[FActScore](https://github.com/shmsw25/FActScore) | [SAFE](https://github.com/google-deepmind/long-form-factuality) | [RAGAS](https://github.com/explodinggradients/ragas) | [DeepEval](https://github.com/confident-ai/deepeval) | [Lynx](https://huggingface.co/PatronusAI/Llama-3-Patronus-Lynx-70B-Instruct) | [RefChecker](https://github.com/amazon-science/RefChecker) | [HHEM](https://huggingface.co/vectara/hallucination_evaluation_model) | [HELM](https://github.com/stanford-crfm/helm) | [Vectara Leaderboard](https://github.com/vectara/hallucination-leaderboard)

## Design Principles

- **The framework's most dangerous failure mode is success.** A false claim from syndicated sources receives the highest confidence score, causing users to trust wrong outputs *more* than without the framework.
- **Detect when wrong, not prove when right.** Proving correctness is unbounded; detecting likely errors is tractable. This is the Inversion Principle.
- **Trust comes from accountability, track record, and simplicity** — not from 17 layers of epistemological metadata.

## Installation

### Prerequisites

- [Claude Code](https://claude.com/claude-code) (latest version recommended)
- Works best with Claude Opus or Sonnet models

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

**Verify installation:** Ask Claude Code a trust-related question like *"how do we make users trust our AI outputs?"* — the skill should activate automatically and begin with Phase 0 auto-detection.

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

## Limitations

- Does not perform dynamic security testing or runtime analysis
- Trust assessment quality depends on the accuracy of information you provide
- Not a substitute for a formal security audit or penetration test

## Contributing

Contributions welcome, especially around keeping research references current. Please open an issue before submitting a PR for non-trivial changes.

## Origin

This skill was built through a structured brainstorming session + 5-agent adversarial review panel using the [Agent Review Panel](https://github.com/wan-huiyan/agent-review-panel) skill. Enhanced through deep research across 25+ academic papers and competitive analysis of 15 existing tools. See the [full research bibliography](skills/ai-trust-evaluation/references/tools-and-papers.md) for citations.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

Current version: check [releases](https://github.com/wan-huiyan/ai-trust-evaluation/releases).

## License

MIT
