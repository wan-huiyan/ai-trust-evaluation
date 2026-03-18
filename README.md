# AI Trust Evaluation - Claude Code Skill

A Claude Code skill for evaluating and designing trust/accuracy frameworks for AI systems that extract, synthesize, or generate intelligence from uncontrolled sources (open web, public data, multi-source RAG).

## When to Use

- Users don't trust AI-generated outputs and citations alone aren't sufficient
- Classic RAG grounding techniques don't apply (corpus is the open web)
- Designing confidence scoring, verification, or trust UX for an AI product
- Evaluating whether a proposed trust framework will actually work
- Building evaluation pipelines for web-extracted intelligence

## What It Covers

- **The Three Laws of Trust** — accountability, track record, simplicity (trust is relational, not just informational)
- **The Inversion Principle** — detect when wrong (tractable) vs prove when right (unbounded)
- **17-Strategy Taxonomy** across 3 layers: backend intelligence, trust UX, continuous calibration
- **39-Issue Registry** of common pitfalls including source independence, circular sourcing, cognitive overload
- **Failure Mode Analysis** — the most dangerous failure is when false claims get the highest confidence scores
- **Implementation Roadmap** — phased approach from prerequisites through maturity
- **Persona-Based Design** — executive (one score), consultant (exportable proof), analyst (full transparency)

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

This skill was extracted from a structured brainstorming session + 5-agent adversarial review panel (Feasibility Analyst, Stakeholder Advocate, Risk Assessor, Statistical Rigor Reviewer, Devil's Advocate) with completeness audit and supreme judge arbitration. The methodology draws on ChatEval (ICLR 2024), AutoGen, Du et al. (ICML 2024), MachineSoM (ACL 2024), and DebateLLM research.

## License

MIT
