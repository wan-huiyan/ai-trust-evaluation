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
  techniques from FActScore, SAFE, Semantic Entropy, RAGAS v0.4+, DeepEval, HHEM,
  Lynx, RefChecker, ChainPoll, and HELM.
author: wan-huiyan
version: 4.0.0
date: 2026-03-23
---

# AI Trust Evaluation Framework

## How This Skill Works

This is an **interactive diagnostic**, not a static document. When triggered, follow
the process below to produce recommendations tailored to the user's specific product,
architecture, and users. The knowledge base sections below are your reference material —
draw from them selectively based on what applies to the user's situation.

---

## Process

### Phase 0: Auto-Detect Context (before asking any questions)

Before asking the user anything, silently scan their project to pre-load context.
This saves the user from answering questions you could have figured out yourself.

**Step 1: Scan the project** (if working directory is a project, not ~)

```
# Look for trust-relevant code and config
Glob: **/*trust* **/*confidence* **/*score* **/*citation* **/*source*
Glob: **/*verify* **/*fact* **/*claim* **/*grounding*
Grep: "confidence_score|trust_score|credibility|corroboration|verified"
Grep: "citation|source_url|provenance|ground_truth"
```

**Step 2: Detect product signals** (keyword scan, case-insensitive, 3+ keywords trigger)

| Signal | Detection Keywords | Pre-loaded Context |
|--------|-------------------|-------------------|
| Web Extraction | `scrape`, `crawl`, `extract`, `selenium`, `playwright`, `beautifulsoup`, `httpx`, `public web` | Product extracts from web — syndication and source independence issues are primary |
| RAG/Retrieval | `retrieval`, `embedding`, `vector`, `chromadb`, `pinecone`, `weaviate`, `RAG`, `chunk` | RAG system — RAGAS faithfulness applies, grounding is partially tractable |
| LLM Generation | `openai`, `anthropic`, `claude`, `gpt`, `llm`, `generate`, `prompt`, `completion` | LLM in the loop — semantic entropy and behavioral consistency apply |
| Structured Data | `SEC`, `Crunchbase`, `Bloomberg`, `API`, `database`, `SQL`, `BigQuery` | Has authoritative data sources — cross-referencing is feasible for hard facts |
| Dashboard/UI | `dashboard`, `react`, `streamlit`, `gradio`, `frontend`, `component`, `visualization` | Has a UI — trust UX strategies (visual hierarchy, labels, decay) are relevant |
| Multi-Source | `corroboration`, `multi-source`, `aggregate`, `consensus`, `cross-reference` | Already doing multi-source — focus on source independence, not basic corroboration |

**Step 3: Check for existing trust mechanisms**

```
Grep: "confidence|trust|verified|citation|source" in UI components or API responses
```

Look for existing confidence scores, citation rendering, verification badges, or
trust-related UI elements. Note what already exists so you don't suggest what they have.

**Step 4: Summarize findings**

Present a brief context summary to the user:
"Before we dive in, I scanned your project and found: [signals detected, existing
trust mechanisms, relevant files]. Let me know if I'm reading this right."

This context means you can skip Phase 1 questions that are already answered. If the
scan reveals nothing (e.g., user is in ~ or the project isn't code), proceed to Phase 1.

### Phase 1: Understand the Product (3-5 questions, one at a time)

Ask these questions one at a time. Adapt based on answers — skip questions where the
answer is already clear from context or Phase 0 auto-detection.

1. **What does the product do?**
   "Describe your product in one sentence — what does it extract/generate, from where,
   and for whom?"
   → Classify: extraction (structured data from web) / synthesis (insights from multiple
   sources) / generation (net-new content) / hybrid

2. **What output types do users consume?**
   Offer choices:
   - (a) Hard facts (revenue, HQ, founding year, employee count)
   - (b) Synthesized insights (competitive positioning, market trends, SWOT)
   - (c) Recommendations or predictions
   - (d) Mix — roughly what %?
   → This determines which strategies apply. Hard facts → authoritative DB cross-ref.
   Synthesized → corroboration + uncertainty quantification. Predictions → calibration.

3. **Who are the users and what decisions do they make with the output?**
   → Classify into personas: Executive (glance, decide), Consultant (export, present to
   client), Analyst (deep-dive, cross-reference). This shapes the UX layer entirely.

4. **What trust mechanisms exist today?**
   Offer choices:
   - (a) Citations/source links
   - (b) Confidence scores
   - (c) Nothing beyond the output itself
   - (d) Something else — describe
   → Identifies baseline and what's already been tried

5. **What's the trust failure mode you're most worried about?** (optional, ask if unclear)
   - (a) Users ignoring correct outputs because they don't trust them
   - (b) Users trusting incorrect outputs because confidence looks high
   - (c) Users not knowing WHEN to trust vs. verify manually
   - (d) Stakeholders blocking adoption due to perceived unreliability

### Phase 2: Diagnose (automated, no user input needed)

Based on Phase 1 answers, produce a **Trust Diagnosis** covering:

1. **Applicable failure modes** — scan the 39-Issue Registry below and list only the
   issues relevant to this product. For each, explain WHY it applies to their situation
   specifically.

2. **Strategy fit matrix** — score each of the 17 strategies as:
   - **High fit** — directly addresses their problem, feasible given their architecture
   - **Low fit** — doesn't apply or poor ROI for their situation
   - **Risky** — could backfire (explain how)

3. **Missing prerequisites** — identify what must exist before trust features can work
   (entity resolution? source independence? claim decomposition pipeline?)

4. **The "most dangerous failure"** — describe the specific scenario where their trust
   system would amplify a wrong answer for THEIR product. Make it concrete with a
   realistic example.

Present this diagnosis and ask: "Does this match your understanding? Anything I'm
missing about your situation?"

### Phase 3: Recommend (tailored action plan)

Based on diagnosis + user feedback, produce:

1. **Recommended architecture** — which of the 3 layers to invest in and why, specific
   to their product. Not all products need all 3 layers.

2. **Phased roadmap** — 3 phases, each with:
   - What to build (specific strategies from the taxonomy, adapted to their context)
   - Why this order (dependency graph, ROI reasoning)
   - What research/techniques to use (cite specific papers and tools from the reference
     sections below)
   - What to watch out for (specific failure modes from the registry)

3. **Quick wins** — 2-3 things they can ship in a sprint that meaningfully improve trust.
   Always consider:
   - "What We Don't Know" section (high impact, low effort)
   - Temporal freshness signals (almost always applicable)
   - Composite trust score (if users are executives)

4. **What NOT to build** — strategies that are poor fit for their situation and why.
   Be direct. "Provenance graph is low-ROI for executive users" is more useful than
   listing everything.

5. **Verification checklist** — customized from the checklist below, with only the items
   that apply to their recommended architecture.

### Phase 4: Deep Dive (optional, on request)

If the user wants to go deeper on any strategy, provide:
- Implementation details from the Research-Backed Pipeline section
- Specific papers and open-source tools to use
- Known pitfalls from the Issue Registry
- Example of what the output looks like for their product type

If the user wants to stress-test the recommendations, suggest running the
agent-review-panel skill on the tailored plan.

---

## Knowledge Base (Reference Material)

Everything below is reference material. Draw from it selectively during the process
above — never dump it all at once.

### The Three Laws of Trust (Meta-Framework)

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

### The Inversion Principle

The most important non-obvious insight from adversarial review:

> Instead of trying to prove claims are correct (an unbounded problem), focus on
> detecting when the system is likely wrong (a tractable problem).

Research-backed implementation: **Semantic Entropy** (Kuhn et al., Nature 2024) provides
the most validated "likely wrong" detector. Generate N responses (5-10) to the same query,
cluster by meaning using NLI (DeBERTa), compute entropy over clusters. High semantic
entropy = the model is uncertain = likely wrong. Task-agnostic, no training data needed.

For cheaper approximation: **Semantic Entropy Probes** (Kossen et al., ICLR 2025) achieve
similar detection from a single forward pass using hidden-state probes — 5-10x cheaper.

### Research-Backed Implementation Pipeline

Based on survey of 25+ papers (2023-2026), the optimal verification pipeline chains
these components:

#### Step 1: Claim Decomposition (Adaptive)

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

#### Step 2: Per-Claim Verification

**Primary method — SAFE pipeline** (Google DeepMind, NeurIPS 2024):
Decompose → Generate search queries → Verify against search results.
Open-source: `github.com/google-deepmind/long-form-factuality`
Outperforms human annotators (wins 76% of disagreement cases) at 20x lower cost.

**Grounding metric — RAGAS Faithfulness** (EACL 2024, now v0.4+):
Fraction of claims supported by retrieved context. Implementation: decompose answer
into claims, check each against context using NLI. Achieved 0.762 precision for
hallucination detection. RAGAS v0.4+ significantly expands scope beyond RAG:
multimodal evaluation (Multimodal Faithfulness, Multimodal Relevance), agent/tool
evaluation (ToolCallAccuracy, ToolCallF1), multi-turn conversation evaluation, and
a new `EvaluationDataset` API (replaces HuggingFace Datasets dependency).

**Key finding:** Don't rely on a single hallucination detector. EMNLP 2025 Industry
benchmarking found average balanced accuracy below 78% for any single method.
**Ensemble multiple methods** (RAGAS + self-eval + TrustBC) and take conservative estimate.

**Fast-pass detection — HHEM** (Vectara, 3.2k stars):
For high-throughput scenarios, HHEM-2.1-Open provides lightweight hallucination scoring without requiring an LLM judge. Measures factual consistency in summarization by comparing generated text against source documents. Use as a cheap first-pass filter before expensive LLM-based verification. The Vectara Hallucination Leaderboard benchmarks 100+ models on 7,700+ articles, finding hallucination rates range from <2% to >24% across models — useful for model selection decisions.

**Specialized detection models:**
- **Lynx** (Patronus AI, 2024): First open-source model beating GPT-4 on hallucination detection (8.3% more accurate on medical QA). Fine-tuned Llama-3 70B/8B variants for RAG faithfulness evaluation. Benchmark: HaluBench (15k samples). Use for high-stakes domains. (arXiv:2407.08488)
- **RefChecker** (Amazon Science, 2024): Decomposes responses into knowledge triplets `<subject, predicate, object>`, verifies each independently. Outperforms existing methods by up to 9 points. Best when you need to pinpoint *which specific facts* are wrong, not just binary faithful/hallucinated. (arXiv:2405.14486)

**Practical tooling — DeepEval** (14.2k stars, larger than RAGAS):
Provides 30+ pre-built metrics including FaithfulnessMetric, HallucinationMetric, and G-Eval (LLM-as-judge with CoT). Key advantage over RAGAS: debuggable/inspectable LLM judge reasoning, native Pytest integration for CI/CD, and the DAG metric builder for custom deterministic evaluation graphs. Use DeepEval when you need production CI/CD integration; use RAGAS when you need research-grade RAG-specific metrics.

#### Step 3: Source Independence Check

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

#### Step 4: Multi-Source Corroboration Score

**Method — Credibility-weighted aggregation** (MAFC, Scientific Reports 2026):

```
trust_score = Σ(source_credibility_i × support_i) / N_independent_sources
```

Track per-source reliability over time. Separate evidence retrieval agents from
verdict-rendering agents (DelphiAgent cognition/decision split) to prevent retrieval
biases from contaminating final scores.

**Efficiency:** Use importance sampling (FACT-AUDIT, ACL 2025) to focus verification
effort on claims where models are most uncertain, rather than uniformly checking everything.

#### Step 5: Uncertainty Quantification (Layered)

Route claims through increasingly expensive methods only when cheaper ones are inconclusive:

| Layer | Method | Cost | When to Use |
|-------|--------|------|-------------|
| 1 | Verbalized confidence + CoT | Cheap | All claims (first pass) |
| 2 | Logit entropy | Medium | If available (white-box models) |
| 3 | Semantic entropy (multi-sample) | Expensive | High-stakes or inconclusive claims |

**Critical:** Never trust raw LLM self-reported confidence without calibration (ICLR 2025).
Always apply post-hoc calibration — Platt scaling or conformal prediction for guaranteed
coverage probabilities (QA-Calibration, ICLR 2025).

#### Step 6: Behavioral Consistency Check (Reference-Free)

**TrustBC** (Kim et al., 2024): Generate multi-choice distractors from the LLM's own
response, re-query whether it selects its original answer. Low consistency = low trust.
This is a cheap, reference-free signal that doesn't require external knowledge.

**ChainPoll** (Galileo, 2023): Chain-of-thought prompting + multi-sample voting (typically 20 samples) for hallucination detection. Aggregating multiple CoT judgments significantly improves reliability over single-pass evaluation. Complementary to TrustBC — ChainPoll uses explicit reasoning chains while TrustBC uses behavioral probing. (arXiv:2310.18344)

#### Step 7: Adversarial Robustness Testing

Test pipeline against the **Fact-Saboteurs taxonomy** (USENIX Security 2023):
- Evidence manipulation (edits to source text that flip verdicts)
- Claim paraphrasing (minor rewording that evades detection)
- Knowledge poisoning (corrupting reference databases)

Key finding: Even GPT-4 is vulnerable to minor adversarial edits. Require multi-source
agreement to prevent single-source evidence manipulation.

**Hardening:** Adversarially train groundedness checkers using semantically-manipulated
evidence passages (AdvERSEM, *SEM 2025).

### Trust Strategy Taxonomy (17 Strategies, 3 Layers)

#### Layer 1: Claim-Level Intelligence (Backend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 1 | Claim Decomposition | Break insights into atomic claims | LLM nondeterminism — use adaptive decomposition + molecular facts |
| 2 | Multi-Source Corroboration | Count independent sources agreeing | Syndication inflates N — cluster first, then count |
| 3 | Source Authority Weighting | Weight by source credibility | Authority is context-dependent, not a static hierarchy |
| 4 | Internal Consistency Checks | Cross-check facts against each other | Consistency ≠ correctness (disinformation is consistent) |
| 5 | Temporal Freshness | Tag claims with source dates | Publication date ≠ data date; staleness is domain-dependent |
| 6 | Fact-Type Stratification | Classify objective vs. synthesized | Must be prerequisite for UX layer, not parallel |
| 7 | Authoritative DB Cross-Reference | Verify against SEC, Crunchbase | Only covers ~15% of output (hard facts); streetlight effect |

#### Layer 2: Trust UX (Frontend)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 8 | Epistemic Status Labels | "Well-established", "Emerging", etc. | Self-referential — AI assessing its own reliability |
| 9 | Provenance Graph | Interactive source→insight visualization | Almost no one uses it; low ROI for actual users |
| 10 | Three-Tier Visual Hierarchy | Verified → corroborated → single-source | Effective but depends on accurate Layer 1 classification |
| 11 | "What We Don't Know" | Surface gaps and missing data | Most novel/valuable idea — protect and prioritize |
| 12 | Run-over-Run Stability | Show consistency across extractions | Stability ≠ accuracy (consistently wrong = maximally stable) |
| 13 | Confidence Decay | Insights visually age | Creates negative emotional response; reframe as "freshness" |
| 14 | Adversarial Counter-Evidence | Surface contradictory sources | Shifts cognitive burden to user without resolution help |

#### Layer 3: Continuous Calibration (Feedback Loop)

| # | Strategy | What It Does | Key Pitfall |
|---|----------|-------------|-------------|
| 15 | Synthetic Ground Truth Testing | Benchmark against known entities | Survivorship bias — tests where system is least likely to fail |
| 16 | User Correction Feedback | Let users flag/correct claims | Confirmation bias; <2% adoption without incentives |
| 17 | Sampling Audits | Human analyst verification | Often an afterthought; should be backbone, not footnote |

### Critical Failure Modes (The "39-Issue Registry")

#### The Most Dangerous Failure: "Success"

A false claim that appears in multiple syndicated sources, with a recent publication date,
from "credible" outlets will receive the **highest** confidence score. Users trust it more
than they would with no trust framework. The system amplifies wrong answers.

#### Source Independence Problem

Web content is massively duplicated. Press releases get syndicated verbatim to dozens of
outlets. "Confirmed by 5 sources" when all 5 trace to 1 press release is worse than no
signal — it manufactures false confidence.

**Fix:** Implement source clustering (embedding similarity > 0.95, hyperlink graph analysis,
publication timestamp proximity) before counting corroboration. Report "confirmed by N
independent source clusters," not "confirmed by N sources."

#### Circular Sourcing

Distinct from syndication: Source A cites Source B, Source B cites Source A. Creates
mutual-corroboration illusion. Requires citation-graph cycle detection, not just content dedup.

#### The Streetlight Effect

Verified badges (Item 7) only work for hard facts (~15% of output). The valuable majority —
synthesized insights, competitive analysis, trend identification — has no verification
pathway. Building verification for the easy part and ignoring the hard part creates false
sense of completeness.

#### Cognitive Overload

17 trust signals displayed simultaneously paralyze users. Trust signals should reduce
cognitive load, not add to it. Design for personas (supported by ELM research):
- **Executive:** One composite score, drill-down optional (peripheral route)
- **Consultant:** Exportable proof with corroboration and verification badges (central route)
- **Analyst:** Full transparency, counter-evidence, gap analysis (central route, high elaboration)

#### Common Missing Prerequisites

These are frequently omitted from trust frameworks:
- Entity resolution/disambiguation (upstream of everything)
- Source independence detection (upstream of corroboration)
- Language/locale handling (biases against non-English sources)
- Offline/export mode (consultants need trust metadata in PowerPoint)
- Versioning/audit trail (what was shown last week vs today)
- Multi-tenancy isolation (competing firms' corrections leaking)
- Paywalled source handling (highest-authority sources are often inaccessible)
- GDPR/data retention policies

### Verification Checklist

To verify a trust framework is sound, check:

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

### Key Quotes to Remember

> "The framework's most dangerous failure mode is success." — Risk perspective

> "This is a trust museum, not a trust shortcut." — User perspective

> "Instead of proving claims correct (unbounded), detect when the system is likely
> wrong (tractable)." — The inversion principle

> "Trust is built through accountability, track record, and simplicity — not through
> 17 layers of epistemological metadata." — Adversarial perspective

### Open-Source Tools and Benchmarks

| Tool | What It Does | URL |
|------|-------------|-----|
| SAFE | Decompose → search → verify pipeline | github.com/google-deepmind/long-form-factuality |
| FActScore | Atomic fact evaluation | github.com/shmsw25/FActScore |
| RAGAS | RAG evaluation (faithfulness, relevance); 13.1k stars, v0.4+ now covers RAG + agents + multimodal | github.com/explodinggradients/ragas |
| TrustLLM | 6-dimension trustworthiness benchmark | github.com/HowieHwong/TrustLLM |
| FACTS Leaderboard | Comprehensive factuality benchmark | kaggle.com/benchmarks/google/facts |
| FactBench | Dynamic in-the-wild factuality eval | arxiv.org/abs/2410.22257 |
| DeepEval | 30+ LLM evaluation metrics; Pytest-native CI/CD | github.com/confident-ai/deepeval |
| Vectara Hallucination Leaderboard | Model hallucination rate ranking (100+ models, 7.7k articles) | github.com/vectara/hallucination-leaderboard |
| HHEM-2.1-Open | Lightweight hallucination scoring model (no LLM judge needed) | huggingface.co/vectara/hallucination_evaluation_model |
| HELM | Holistic model evaluation (safety, factuality, calibration) | github.com/stanford-crfm/helm |

**Benchmarking guidance — HELM** (Stanford CRFM, 2.7k stars, v0.5.13):
For holistic model evaluation covering trust dimensions (accuracy, calibration, robustness, fairness, toxicity), use Stanford HELM. Key leaderboards: HELM Capabilities (MMLU-Pro, GPQA, IFEval), HELM Safety v1.0 (HarmBench — Claude 3.5 Sonnet ranked highest), HELM Lite (lightweight task mix), and MedHELM (medical domain). Use when selecting which base LLM to recommend for a trust-sensitive application.

### Recommended Evaluation Pipeline (Tiered)

For production systems, use a tiered approach that balances cost and thoroughness:

| Tier | Method | Cost | When to Use |
|------|--------|------|-------------|
| 1 (Fast) | HHEM-2.1-Open or Lynx-8B | Low | All outputs — binary faithful/hallucinated filter |
| 2 (Scored) | DeepEval FaithfulnessMetric or RAGAS Faithfulness | Medium | Outputs passing Tier 1 — scored evaluation with reasoning |
| 3 (Granular) | RefChecker triplet extraction | High | High-stakes outputs — pinpoint specific hallucinated claims |
| 4 (High-confidence) | ChainPoll (20 CoT samples + majority vote) or Semantic Entropy | Expensive | Critical decisions — maximum detection confidence |

Route outputs through tiers based on stakes: low-stakes content stops at Tier 1-2, high-stakes content goes through all tiers. This prevents the common failure of applying expensive verification uniformly (wasteful) or skipping it entirely (dangerous).

### References

#### Key Research Papers
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
- Lynx — Ravi et al., arXiv 2024 (open-source hallucination detection beating GPT-4)
- RefChecker — Hu et al., arXiv 2024 (knowledge-triplet verification)
- ChainPoll — Friel & Sanyal, arXiv 2023 (multi-sample CoT hallucination detection)
- OpenFactCheck — Wang et al., COLING 2025 (customizable fact-checking framework)
- FactCheck-Bench — Wang et al., EMNLP 2024 Findings (multi-level annotation benchmark)
- HELM — Liang et al., arXiv 2022 (holistic language model evaluation, continuously updated)
- "LLM Hallucination: A Comprehensive Survey" — arXiv 2025 (arXiv:2510.06265)
- EdinburghNLP awesome-hallucination-detection — curated paper list (github.com/EdinburghNLP/awesome-hallucination-detection)
