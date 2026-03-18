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

1. **Claim Decomposition** — break outputs into atomic, verifiable claims (FActScore, Molecular Facts, Adaptive Decomposition)
2. **Per-Claim Verification** — search-augmented fact-checking for each claim (SAFE pipeline from Google DeepMind)
3. **Source Independence Detection** — near-duplicate clustering + citation graph analysis to avoid counting syndicated content as corroboration (MAFC)
4. **Corroboration Scoring** — credibility-weighted aggregation across truly independent sources
5. **Uncertainty Quantification** — semantic entropy to detect hallucinations without ground truth (Semantic Entropy, Nature 2024)
6. **Behavioral Consistency** — reference-free trust signal via multi-run stability (TrustBC)
7. **Adversarial Robustness** — defend against the Fact-Saboteurs taxonomy of attacks (USENIX Security 2023)

### Competitive Landscape
No existing Claude Code skill or tool provides a general-purpose AI trust evaluation framework. This skill uniquely combines claim-level verification, trust UX design, and continuous calibration into a single reusable framework.

### Open-Source Tools Referenced
FActScore, SAFE (Google DeepMind), MiniCheck, RAGAS, LangSmith, Semantic Entropy

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

This skill was extracted from a structured brainstorming session + 5-agent adversarial review panel (Feasibility Analyst, Stakeholder Advocate, Risk Assessor, Statistical Rigor Reviewer, Devil's Advocate) with completeness audit and supreme judge arbitration.

v2.0 was enhanced through deep research across 25+ academic papers and competitive analysis of 15 existing tools. Key research foundations include:
- **FActScore** (EMNLP 2023) — atomic fact evaluation
- **SAFE** (NeurIPS 2024) — search-augmented factual evaluation from Google DeepMind
- **Semantic Entropy** (Nature 2024) — uncertainty quantification without ground truth
- **Molecular Facts** (2024) — decontextualized claim decomposition
- **MAFC** (2026) — multi-agent fact-checking with credibility weighting
- **TrustBC** (2024) — behavioral consistency as reference-free trust signal
- **Fact-Saboteurs** (USENIX Security 2023) — adversarial attack taxonomy
- **ELM Trust UX Model** (Emerald 2025) — persona-based trust interface design

## License

MIT
