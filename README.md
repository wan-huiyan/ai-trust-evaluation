# AI Trust Evaluation - Claude Code Skill

A Claude Code skill for evaluating and designing trust/accuracy frameworks for AI systems that extract, synthesize, or generate intelligence from uncontrolled sources (open web, public data, multi-source RAG).

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
3. **Source Independence Detection** — near-duplicate clustering + citation graph analysis to avoid counting syndicated content as corroboration ([MAFC](https://arxiv.org/abs/2305.13281))
4. **Corroboration Scoring** — credibility-weighted aggregation across truly independent sources
5. **Uncertainty Quantification** — semantic entropy to detect hallucinations without ground truth ([Semantic Entropy](https://www.nature.com/articles/s41586-024-07421-0), Nature 2024)
6. **Behavioral Consistency** — reference-free trust signal via multi-run stability ([TrustBC](https://arxiv.org/abs/2404.13782))
7. **Adversarial Robustness** — defend against the [Fact-Saboteurs](https://www.usenix.org/conference/usenixsecurity23/presentation/bao) taxonomy of attacks (USENIX Security 2023)

### Competitive Landscape
No existing Claude Code skill or tool provides a general-purpose AI trust evaluation framework. This skill uniquely combines claim-level verification, trust UX design, and continuous calibration into a single reusable framework.

### Open-Source Tools Referenced
[FActScore](https://github.com/shmsw25/FActScore) | [SAFE](https://github.com/google-deepmind/long-form-factuality) | [MiniCheck](https://github.com/Liyan06/MiniCheck) | [RAGAS](https://github.com/explodinggradients/ragas) | [LangSmith](https://github.com/langchain-ai/langsmith-sdk)

## Example Output

When you invoke this skill (e.g., "how do we make users trust our AI-extracted insights?"), it produces structured analysis like:

<details>
<summary><b>Strategy Taxonomy (17 strategies across 3 layers)</b></summary>

```
Layer 1: Claim-Level Intelligence (Backend)
├── 1. Claim Decomposition + Atomic Verification
├── 2. Multi-Source Corroboration Scoring
├── 3. Source Authority Weighting
├── 4. Internal Consistency Checks
├── 5. Temporal Freshness Signals
├── 6. Stratify by Fact Type
└── 7. Authoritative Database Cross-Referencing

Layer 2: Trust UX (Frontend Dashboard)
├── 8.  Epistemic Status Labels ("Well-established fact", "Emerging signal", ...)
├── 9.  Provenance Graph
├── 10. Three-Tier Visual Hierarchy
├── 11. "What We Don't Know" Section
├── 12. Run-over-Run Stability Signals
├── 13. Confidence Decay
└── 14. Adversarial Counter-Evidence

Layer 3: Continuous Calibration (Feedback Loop)
├── 15. Synthetic Ground Truth Testing
├── 16. User Correction Feedback Loop
└── 17. Sampling Audits
```
</details>

<details>
<summary><b>39-Issue Registry (sample)</b></summary>

| # | Issue | Category | Severity |
|---|-------|----------|----------|
| 1 | Source independence unaddressed — syndicated content inflates corroboration | Backend | Critical |
| 2 | No scoring functions or calibration protocols defined | Backend | Critical |
| 3 | Cognitive overload — 17 simultaneous trust signals paralyze users | UX | High |
| 4 | No adversarial threat model for SEO manipulation or planted misinfo | Security | High |
| 5 | "What We Don't Know" — most novel idea, but undefined implementation | UX | Medium |
| ... | *34 more issues across all layers* | | |
</details>

<details>
<summary><b>Implementation Roadmap</b></summary>

```
Phase 0: Prerequisites (before building anything)
  → User research: what trust signals do YOUR users actually want?
  → Dependency graph: which strategies depend on which?
  → Source independence detection (unsolved prerequisite for corroboration)

Phase 1: Ship 4 high-ROI items
  → Verified badges (authoritative DB cross-ref) — trust anchor via halo effect
  → "What We Don't Know" section — paradoxically increases trust
  → Temporal freshness signals — low-cost, high-signal
  → Composite trust score — the "Bloomberg number" for executives

Phase 2: Corroboration + Calibration
  → Claim decomposition pipeline
  → Multi-source corroboration (only after source independence is solved)
  → Synthetic ground truth testing

Phase 3: Advanced UX + Feedback
  → Epistemic status labels
  → User correction loop
  → Confidence decay
```
</details>

## Key Insights

> "The framework's most dangerous failure mode is success." — A false claim from syndicated sources receives the highest confidence score, causing users to trust wrong outputs *more* than without the framework.

> "Instead of proving claims correct (unbounded), detect when the system is likely wrong (tractable)." — The Inversion Principle

> "Trust is built through accountability, track record, and simplicity — not through 17 layers of epistemological metadata."

## Installation

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/wan-huiyan/ai-trust-evaluation.git ~/.claude/skills/ai-trust-evaluation
```

Or manually copy `SKILL.md` to `~/.claude/skills/ai-trust-evaluation/SKILL.md`.

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

## License

MIT
