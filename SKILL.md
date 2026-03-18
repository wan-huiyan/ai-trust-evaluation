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
  the 39-issue registry of common pitfalls, and research-backed implementation
  techniques from FActScore, SAFE, Semantic Entropy, and RAGAS.
author: Claude Code (extracted via Claudeception)
version: 2.0.0
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

## Competitive Landscape

No existing Claude Code skill or tool provides a general-purpose AI trust evaluation
framework. The closest analogs are domain-specific:

| Tool | What It Does | Gap |
|------|-------------|-----|
| fact-checker (daymade) | Fact-checks claims in docs | No framework, just claim-level |
| academic-research-skills (Imbad0202) | Anti-hallucination for papers | Locked to academic pipeline |
| OSINT Skill (smixs) | A/B/C/D confidence grading | Purpose-built for person research |
| claude-deep-research-skill (199-bio) | CiteGuard + source credibility | Within research, not standalone |
| last30days-skill (mvanhorn) | Cross-platform convergence | Implicit trust, no explicit scores |

This skill fills the gap: a **general-purpose, domain-agnostic** framework for evaluating
and designing trust in any AI system that extracts from uncontrolled sources.

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

Research support: The Elaboration Likelihood Model (ELM) study (Emerald 2025) confirms
this dual-track: expert users rely on content quality (central route), casual users rely
on interface cues (peripheral route). Design for both.

## The Inversion Principle

The most important non-obvious insight from adversarial review:

> Instead of trying to prove claims are correct (an unbounded problem), focus on
> detecting when the system is likely wrong (a tractable problem).

Research-backed implementation: **Semantic Entropy** (Kuhn et al., Nature 2024) provides
the most validated "likely wrong" detector. Generate N responses (5-10) to the same query,
cluster by meaning using NLI (DeBERTa), compute entropy over clusters. High semantic
entropy = the model is uncertain = likely wrong. Task-agnostic, no training data needed.

For cheaper approximation: **Semantic Entropy Probes** (Kossen et al., ICLR 2025) achieve
similar detection from a single forward pass using hidden-state probes — 5-10x cheaper.

## Research-Backed Implementation Pipeline

Based on survey of 25+ papers (2023-2026), the optimal verification pipeline chains
these components:

### Step 1: Claim Decomposition (Adaptive)

**Use:** FActScore (EMNLP 2023) / SAFE (NeurIPS 2024) decomposition prompts.

**Critical refinement — Molecular Facts** (Kamoi et al., 2024): Raw atomic facts lose
context (ambiguous pronouns, missing qualifiers). After initial decomposition, run a
decontextualization pass that resolves pronouns and adds necessary qualifiers to produce
self-contained "molecular facts."

**Critical refinement — Adaptive decomposition** (EMNLP 2025): Decomposition helps for
complex multi-hop claims but **hurts** for simple claims (introduces noise, loses context).
Estimate claim complexity first (entity count, conjunctions, temporal references);
decompose only when complexity exceeds a threshold. Simple claims go directly to
verification.

**Quality check:** Use DecMetrics (2025) three dimensions — completeness, correctness,
semantic entropy — to validate decomposition quality before passing to verification.

### Step 2: Per-Claim Verification

**Primary method — SAFE pipeline** (Google DeepMind, NeurIPS 2024):
Decompose → Generate search queries → Verify against search results.
Open-source: `github.com/google-deepmind/long-form-factuality`
Outperforms human annotators (wins 76% of disagreement cases) at 20x lower cost.

**Grounding metric — RAGAS Faithfulness** (EACL 2024):
Fraction of claims supported by retrieved context. Implementation: decompose answer
into claims, check each against context using NLI. Achieved 0.762 precision for
hallucination detection.

**Key finding:** Don't rely on a single hallucination detector. EMNLP 2025 Industry
benchmarking found average balanced accuracy below 78% for any single method.
**Ensemble multiple methods** (RAGAS + self-eval + TrustBC) and take conservative estimate.

### Step 3: Source Independence Check

**Problem:** Web content is massively duplicated. Press releases syndicate to dozens
of outlets. "Confirmed by 5 sources" from 1 press release is worse than no signal.

**Method — Near-duplicate detection** (NAACL 2025 Industry):
1. Embed article bodies using sentence transformers
2. Cluster by cosine similarity > 0.95 (syndication threshold)
3. Build source graph using hyperlink structure (Srivastava et al., 2024)
4. Detect circular sourcing via citation-graph cycle detection
5. Treat each cluster as ONE source for corroboration

**When available:** Parse C2PA metadata (Content Provenance standard) for cryptographic
content origin verification.

### Step 4: Multi-Source Corroboration Score

**Method — Credibility-weighted aggregation** (MAFC, Scientific Reports 2026):

```
trust_score = Σ(source_credibility_i × support_i) / N_independent_sources
```

Track per-source reliability over time. Separate evidence retrieval agents from
verdict-rendering agents (DelphiAgent cognition/decision split) to prevent retrieval
biases from contaminating final scores.

**Efficiency:** Use importance sampling (FACT-AUDIT, ACL 2025) to focus verification
effort on claims where models are most uncertain, rather than uniformly checking everything.

### Step 5: Uncertainty Quantification (Layered)

Route claims through increasingly expensive methods only when cheaper ones are inconclusive:

| Layer | Method | Cost | When to Use |
|-------|--------|------|-------------|
| 1 | Verbalized confidence + CoT | Cheap | All claims (first pass) |
| 2 | Logit entropy | Medium | If available (white-box models) |
| 3 | Semantic entropy (multi-sample) | Expensive | High-stakes or inconclusive claims |

**Critical:** Never trust raw LLM self-reported confidence without calibration (ICLR 2025).
Always apply post-hoc calibration — Platt scaling or conformal prediction for guaranteed
coverage probabilities (QA-Calibration, ICLR 2025).

### Step 6: Behavioral Consistency Check (Reference-Free)

**TrustBC** (Kim et al., 2024): Generate multi-choice distractors from the LLM's own
response, re-query whether it selects its original answer. Low consistency = low trust.
This is a cheap, reference-free signal that doesn't require external knowledge.

### Step 7: Adversarial Robustness Testing

Test pipeline against the **Fact-Saboteurs taxonomy** (USENIX Security 2023):
- Evidence manipulation (edits to source text that flip verdicts)
- Claim paraphrasing (minor rewording that evades detection)
- Knowledge poisoning (corrupting reference databases)

Key finding: Even GPT-4 is vulnerable to minor adversarial edits. Require multi-source
agreement to prevent single-source evidence manipulation.

**Hardening:** Adversarially train groundedness checkers using semantically-manipulated
evidence passages (AdvERSEM, *SEM 2025).

## Trust Strategy Taxonomy (17 Strategies, 3 Layers)

### Layer 1: Claim-Level Intelligence (Backend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 1 | Claim Decomposition | Break insights into atomic claims | LLM nondeterminism — use adaptive decomposition + molecular facts |
| 2 | Multi-Source Corroboration | Count independent sources agreeing | Syndication inflates N — cluster first, then count |
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

**Fix:** Implement source clustering (embedding similarity > 0.95, hyperlink graph analysis,
publication timestamp proximity) before counting corroboration. Report "confirmed by N
independent source clusters," not "confirmed by N sources."

### Circular Sourcing

Distinct from syndication: Source A cites Source B, Source B cites Source A. Creates
mutual-corroboration illusion. Requires citation-graph cycle detection, not just content dedup.

### The Streetlight Effect

Verified badges (Item 7) only work for hard facts (~15% of output). The valuable majority —
synthesized insights, competitive analysis, trend identification — has no verification
pathway. Building verification for the easy part and ignoring the hard part creates false
sense of completeness.

### Cognitive Overload

17 trust signals displayed simultaneously paralyze users. Trust signals should reduce
cognitive load, not add to it. Design for personas (supported by ELM research):
- **Executive:** One composite score, drill-down optional (peripheral route)
- **Consultant:** Exportable proof with corroboration and verification badges (central route)
- **Analyst:** Full transparency, counter-evidence, gap analysis (central route, high elaboration)

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
- Source independence detection (near-duplicate clustering + citation graph)
- Claim decomposition research spike (adaptive decomposition + molecular facts; target >90% reproducibility)

**Phase 1 — Launch (high-ROI, low-friction):**
- Verified badges via authoritative databases (trust anchor, halo effect)
- "What We Don't Know" section (most differentiated feature)
- Composite trust score (one glanceable number — use credibility-weighted aggregation)
- Temporal freshness signals (reframed as "Freshness Badges")
- Semantic entropy as "likely wrong" detector (inversion principle)

**Phase 2 — Deepen:**
- SAFE pipeline for per-claim verification (decompose → search → verify)
- Multi-source corroboration (only after source independence solved)
- Source authority weighting with credibility tracking
- Fact-type stratification + epistemic labels + visual hierarchy
- TrustBC behavioral consistency checks

**Phase 3 — Mature:**
- Stability signals, confidence decay, counter-evidence
- Benchmarks (use FACTS Leaderboard methodology, stratified by entity prominence)
- User feedback with bias correction
- Sampling audits with stratified sampling and inter-rater reliability (Cohen's kappa > 0.7)
- Adversarial red-teaming against Fact-Saboteurs taxonomy

**Defer/Kill:**
- Provenance graph for users (keep as internal QA tool only)

## Open-Source Tools and Benchmarks

| Tool | What It Does | URL |
|------|-------------|-----|
| SAFE | Decompose → search → verify pipeline | github.com/google-deepmind/long-form-factuality |
| FActScore | Atomic fact evaluation | github.com/shmsw25/FActScore |
| RAGAS | RAG evaluation (faithfulness, relevance) | github.com/explodinggradients/ragas |
| TrustLLM | 6-dimension trustworthiness benchmark | github.com/HowieHwong/TrustLLM |
| FACTS Leaderboard | Comprehensive factuality benchmark | kaggle.com/benchmarks/google/facts |
| FactBench | Dynamic in-the-wild factuality eval | arxiv.org/abs/2410.22257 |

## Verification Checklist

To verify your trust framework is sound, check:

1. **Failure mode for every component** — What happens when it fails? Is a failing trust
   signal worse than no signal?
2. **Source independence** — Does corroboration scoring account for syndication and circular
   sourcing? (Use embedding clustering + citation graph)
3. **Calibration protocol** — When you say 80% confidence, are you right 80% of the time?
   (Apply Platt scaling or conformal prediction; measure ECE)
4. **User research** — Have you tested whether more transparency increases or decreases
   trust with actual users? (ELM predicts it depends on persona)
5. **Degradation model** — What happens with sparse data, no authoritative sources, or
   irreconcilable contradictions?
6. **Adversarial threat model** — Test against Fact-Saboteurs taxonomy: evidence manipulation,
   claim paraphrasing, knowledge poisoning
7. **Decomposition quality** — Are you using adaptive decomposition (skip simple claims)?
   Are molecular facts decontextualized?
8. **Ensemble verification** — Are you using multiple detection methods? No single method
   exceeds 78% balanced accuracy alone.

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
- v2.0 incorporates findings from 25+ research papers (2023-2026) and competitive analysis
  of 15 existing skills/tools in the ecosystem
- The "inversion principle" is the single highest-value insight: anomaly detection is
  more tractable than comprehensive verification
- Always start with user research before building trust features — the assumption that
  "more evidence = more trust" is testable and may be wrong
- No existing tool in the Claude Code ecosystem provides general-purpose trust evaluation —
  this skill fills that gap

## References

### Analysis Documents
- Full analysis: `~/Documents/brainstorm/monks_iq_complete_analysis.md`
- Review panel report: `~/Documents/brainstorm/monks_iq_trust_framework_review.md`

### Key Research Papers
- FActScore — Min et al., EMNLP 2023 (atomic fact evaluation)
- SAFE — Wei et al., NeurIPS 2024 (search-augmented factual evaluation)
- Semantic Entropy — Kuhn et al., Nature 2024 (hallucination detection via meaning clusters)
- Semantic Entropy Probes — Kossen et al., ICLR 2025 (cheap single-pass uncertainty)
- RAGAS — Shahul Es et al., EACL 2024 (RAG faithfulness metric)
- TrustLLM — Huang et al., ICML 2024 (6-dimension trustworthiness benchmark)
- TrustScore/TrustBC — Kim et al., 2024 (behavioral consistency for reference-free trust)
- Molecular Facts — Kamoi et al., 2024 (decontextualized atomic claims)
- MAFC — Scientific Reports 2026 (credibility-weighted multi-agent fact-checking)
- DelphiAgent — Information Processing & Management 2025 (cognition/decision split)
- FACT-AUDIT — ACL 2025 (importance-sampling-driven evaluation)
- Fact-Saboteurs — Abdelnabi & Fritz, USENIX Security 2023 (adversarial attack taxonomy)
- AdvERSEM — *SEM 2025 (adversarial robustness for groundedness evaluators)
- QA-Calibration — ICLR 2025 (calibrated confidence with statistical guarantees)
- ELM for AIGC Trust — Emerald 2025 (dual-track trust UX: expert vs casual users)
- Decomposition Dilemmas — EMNLP 2025 (adaptive decomposition: helps complex, hurts simple)
- DecMetrics — 2025 (decomposition quality validation)
- FACTS Leaderboard — Google DeepMind 2025 (comprehensive factuality benchmark)

### Review Panel Methodology
- ChatEval (ICLR 2024), AutoGen, Du et al. (ICML 2024),
  MachineSoM (ACL 2024), DebateLLM
