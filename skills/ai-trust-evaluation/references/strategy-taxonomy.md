# Trust Strategy Taxonomy (17 Strategies, 3 Layers)

## Layer 1: Claim-Level Intelligence (Backend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 1 | Claim Decomposition | Break insights into atomic claims | LLM nondeterminism — use adaptive decomposition + molecular facts |
| 2 | Multi-Source Corroboration | Count independent sources agreeing | Syndication inflates N — cluster first, then count |
| 3 | Source Authority Weighting | Weight by source credibility | Authority is context-dependent, not a static hierarchy |
| 4 | Internal Consistency Checks | Cross-check facts against each other | Consistency ≠ correctness (disinformation is consistent) |
| 5 | Temporal Freshness | Tag claims with source dates | Publication date ≠ data date; staleness is domain-dependent |
| 6 | Fact-Type Stratification | Classify objective vs. synthesized | Must be prerequisite for UX layer, not parallel |
| 7 | Authoritative DB Cross-Reference | Verify against SEC, Crunchbase | Only covers ~15% of output (hard facts); streetlight effect |

## Layer 2: Trust UX (Frontend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 8 | Epistemic Status Labels | "Well-established", "Emerging", etc. | Self-referential — AI assessing its own reliability |
| 9 | Provenance Graph | Interactive source→insight visualization | Almost no one uses it; low ROI for actual users |
| 10 | Three-Tier Visual Hierarchy | Verified → corroborated → single-source | Effective but depends on accurate Layer 1 classification |
| 11 | "What We Don't Know" | Surface gaps and missing data | Most novel/valuable idea — protect and prioritize |
| 12 | Run-over-Run Stability | Show consistency across extractions | Stability ≠ accuracy (consistently wrong = maximally stable) |
| 13 | Confidence Decay | Insights visually age | Creates negative emotional response; reframe as "freshness" |
| 14 | Adversarial Counter-Evidence | Surface contradictory sources | Shifts cognitive burden to user without resolution help |

## Layer 3: Continuous Calibration (Feedback Loop)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 15 | Synthetic Ground Truth Testing | Benchmark against known entities | Survivorship bias — tests where system is least likely to fail |
| 16 | User Correction Feedback | Let users flag/correct claims | Confirmation bias; <2% adoption without incentives |
| 17 | Sampling Audits | Human analyst verification | Often an afterthought; should be backbone, not footnote |
