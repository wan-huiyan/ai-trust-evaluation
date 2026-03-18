---
name: ai-trust-evaluation
description: |
  Evaluate and design trust/accuracy frameworks for AI systems that extract, synthesize, or
  generate intelligence from uncontrolled sources (open web, public data, multi-source RAG).
  Use when: (1) users don't trust AI-generated outputs and citations alone aren't sufficient,
  (2) classic RAG grounding techniques don't apply because the corpus is the open web,
  (3) designing confidence scoring, verification, or trust UX for an AI product,
  (4) evaluating whether a proposed trust framework will actually work,
  (5) someone asks "how do we make users trust our AI outputs?",
  (6) building evaluation pipelines for web-extracted intelligence.
  Covers: the trust taxonomy (relational vs informational), the inversion principle,
  source independence detection, failure mode analysis, persona-based trust UX,
  and the 39-issue registry of common pitfalls.
author: Claude Code (extracted via Claudeception)
version: 1.0.0
date: 2026-03-18
---

# AI Trust Evaluation Framework

## Problem

AI systems that extract or synthesize intelligence from uncontrolled sources (the open web,
public databases, multi-source retrieval) face a fundamental trust problem: users don't
believe the outputs, citations prove "a source said it" but not that it's true, and classic
RAG grounding techniques (faithfulness to retrieved passages) don't apply when the corpus
is the entire internet.

## Context / Trigger Conditions

Use this skill when:
- An AI product extracts structured insights from the public web
- Users (consultants, analysts, executives) need to stake decisions on AI-generated intelligence
- Citations have been added but trust hasn't improved
- The team is designing confidence scores, verification badges, or trust UX
- Someone asks "how do we evaluate accuracy when there's no ground truth?"
- A trust framework has been proposed and needs stress-testing

## The Three Laws of Trust (Meta-Framework)

Before designing any trust system, understand how trust actually gets built in information
products. Trust is **relational, not just informational**:

1. **Accountability** — Users trust systems that have consequences for being wrong. What
   happens when the system produces a materially incorrect insight? Without an answer,
   no amount of metadata builds trust.

2. **Track Record** — Users trust systems that have been right before. They remember the
   last time the system was wrong and how it handled it. Error recovery and transparency
   about past failures build more trust than confidence scores.

3. **Simplicity** — Users trust systems they can understand. The most trusted data sources
   in the world (Bloomberg, S&P, FICO) give you one number and let you drill down.

**Design backward from these three principles, not forward from a feature list.**

## The Inversion Principle

The most important non-obvious insight from adversarial review:

> Instead of trying to prove claims are correct (an unbounded problem), focus on
> detecting when the system is likely wrong (a tractable problem).

Anomaly detection, out-of-distribution detection, and uncertainty quantification are
more feasible engineering problems than comprehensive verification. Build "likely wrong"
detection alongside — or even instead of — "probably right" scoring.

## Trust Strategy Taxonomy (17 Strategies, 3 Layers)

### Layer 1: Claim-Level Intelligence (Backend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 1 | Claim Decomposition | Break insights into atomic claims | LLM nondeterminism — decomposition varies across runs |
| 2 | Multi-Source Corroboration | Count independent sources agreeing | Syndication inflates N — 10 URLs from 1 press release |
| 3 | Source Authority Weighting | Weight by source credibility | Authority is context-dependent, not a static hierarchy |
| 4 | Internal Consistency Checks | Cross-check facts against each other | Consistency ≠ correctness (disinformation is consistent) |
| 5 | Temporal Freshness | Tag claims with source dates | Publication date ≠ data date; staleness is domain-dependent |
| 6 | Fact-Type Stratification | Classify objective vs. synthesized | Must be prerequisite for UX layer, not parallel |
| 7 | Authoritative DB Cross-Reference | Verify against SEC, Crunchbase | Only covers ~15% of output (hard facts); streetlight effect |

### Layer 2: Trust UX (Frontend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 8 | Epistemic Status Labels | "Well-established", "Emerging", etc. | Self-referential — AI assessing its own reliability |
| 9 | Provenance Graph | Interactive source→insight visualization | Almost no one uses it; low ROI for actual users |
| 10 | Three-Tier Visual Hierarchy | Verified → corroborated → single-source | Effective but depends on accurate Layer 1 classification |
| 11 | "What We Don't Know" | Surface gaps and missing data | Most novel/valuable idea — protect and prioritize |
| 12 | Run-over-Run Stability | Show consistency across extractions | Stability ≠ accuracy (consistently wrong = maximally stable) |
| 13 | Confidence Decay | Insights visually age | Creates negative emotional response; reframe as "freshness" |
| 14 | Adversarial Counter-Evidence | Surface contradictory sources | Shifts cognitive burden to user without resolution help |

### Layer 3: Continuous Calibration (Feedback Loop)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 15 | Synthetic Ground Truth Testing | Benchmark against known entities | Survivorship bias — tests where system is least likely to fail |
| 16 | User Correction Feedback | Let users flag/correct claims | Confirmation bias; <2% adoption without incentives |
| 17 | Sampling Audits | Human analyst verification | Often an afterthought; should be backbone, not footnote |

## Critical Failure Modes (The "39-Issue Registry")

### The Most Dangerous Failure: "Success"

A false claim that appears in multiple syndicated sources, with a recent publication date,
from "credible" outlets will receive the **highest** confidence score. Users trust it more
than they would with no trust framework. The system amplifies wrong answers.

### Source Independence Problem

Web content is massively duplicated. Press releases get syndicated verbatim to dozens of
outlets. "Confirmed by 5 sources" when all 5 trace to 1 press release is worse than no
signal — it manufactures false confidence.

**Fix:** Implement source clustering (URL domain, content similarity, publication timestamp
proximity) before counting corroboration. Report "confirmed by N independent source
clusters," not "confirmed by N sources."

### Circular Sourcing

Distinct from syndication: Source A cites Source B, Source B cites Source A. Creates
mutual-corroboration illusion. Requires citation-graph analysis, not just content dedup.

### The Streetlight Effect

Verified badges (Item 7) only work for hard facts (~15% of output). The valuable majority —
synthesized insights, competitive analysis, trend identification — has no verification
pathway. Building verification for the easy part and ignoring the hard part creates false
sense of completeness.

### Cognitive Overload

17 trust signals displayed simultaneously paralyze users. Trust signals should reduce
cognitive load, not add to it. Design for personas:
- **Executive:** One composite score, drill-down optional
- **Consultant:** Exportable proof with corroboration and verification badges
- **Analyst:** Full transparency, counter-evidence, gap analysis

### Common Missing Prerequisites

These are frequently omitted from trust frameworks:
- Entity resolution/disambiguation (upstream of everything)
- Source independence detection (upstream of corroboration)
- Language/locale handling (biases against non-English sources)
- Offline/export mode (consultants need trust metadata in PowerPoint)
- Versioning/audit trail (what was shown last week vs today)
- Multi-tenancy isolation (competing firms' corrections leaking)
- Paywalled source handling (highest-authority sources are often inaccessible)
- GDPR/data retention policies

## Recommended Implementation Sequence

**Phase 0 — Prerequisites:**
- Entity resolution/disambiguation
- Source independence detection (source clustering)
- Claim decomposition research spike (target >90% reproducibility)

**Phase 1 — Launch (high-ROI, low-friction):**
- Verified badges via authoritative databases (trust anchor, halo effect)
- "What We Don't Know" section (most differentiated feature)
- Composite trust score (one glanceable number)
- Temporal freshness signals (reframed as "Freshness Badges")

**Phase 2 — Deepen:**
- Multi-source corroboration (only after source independence solved)
- Source authority weighting
- Fact-type stratification + epistemic labels + visual hierarchy

**Phase 3 — Mature:**
- Stability signals, confidence decay, counter-evidence
- Benchmarks, user feedback, sampling audits

**Defer/Kill:**
- Provenance graph for users (keep as internal QA tool only)

## Verification

To verify your trust framework is sound, check:

1. **Failure mode for every component** — What happens when it fails? Is a failing trust
   signal worse than no signal?
2. **Source independence** — Does corroboration scoring account for syndication and circular
   sourcing?
3. **Calibration protocol** — When you say 80% confidence, are you right 80% of the time?
   (ECE, Platt scaling, calibration curves)
4. **User research** — Have you tested whether more transparency increases or decreases
   trust with actual users?
5. **Degradation model** — What happens with sparse data, no authoritative sources, or
   irreconcilable contradictions?
6. **Adversarial threat model** — How can the system be gamed? (SEO manipulation, planted
   misinformation, astroturfing)

## Key Quotes to Remember

> "The framework's most dangerous failure mode is success." — Risk perspective

> "This is a trust museum, not a trust shortcut." — User perspective

> "Instead of proving claims correct (unbounded), detect when the system is likely
> wrong (tractable)." — The inversion principle

> "Trust is built through accountability, track record, and simplicity — not through
> 17 layers of epistemological metadata." — Adversarial perspective

## Notes

- This framework was developed through structured brainstorming + a 5-reviewer adversarial
  panel with completeness audit and supreme judge arbitration
- The 39-issue registry is comprehensive but not exhaustive — new failure modes emerge
  with new data sources and user patterns
- The "inversion principle" is the single highest-value insight: anomaly detection is
  more tractable than comprehensive verification
- Always start with user research before building trust features — the assumption that
  "more evidence = more trust" is testable and may be wrong

## References

- Full analysis document: `~/Documents/brainstorm/monks_iq_complete_analysis.md`
- Review panel report: `~/Documents/brainstorm/monks_iq_trust_framework_review.md`
- Research methodology: ChatEval (ICLR 2024), AutoGen, Du et al. (ICML 2024),
  MachineSoM (ACL 2024), DebateLLM
