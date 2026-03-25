# Research-Backed Verification Pipeline

Based on survey of 25+ papers (2023-2026), the optimal verification pipeline chains
these components:

## Step 1: Claim Decomposition (Adaptive)

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

## Step 2: Per-Claim Verification

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
HHEM-2.1-Open provides lightweight hallucination scoring without requiring an LLM judge. Measures factual consistency in summarization by comparing generated text against source documents. Use as a cheap first-pass filter before expensive LLM-based verification. The Vectara Hallucination Leaderboard benchmarks 100+ models on 7,700+ articles, finding hallucination rates range from <2% to >24% across models — useful for model selection decisions.

**Specialized detection models:**
- **Lynx** (Patronus AI, 2024): First open-source model beating GPT-4 on hallucination detection (8.3% more accurate on medical QA). Fine-tuned Llama-3 70B/8B variants for RAG faithfulness evaluation. Benchmark: HaluBench (15k samples). Use for high-stakes domains. (arXiv:2407.08488)
- **RefChecker** (Amazon Science, 2024): Decomposes responses into knowledge triplets `<subject, predicate, object>`, verifies each independently. Outperforms existing methods by up to 9 points. Best when you need to pinpoint *which specific facts* are wrong, not just binary faithful/hallucinated. (arXiv:2405.14486)

**Practical tooling — DeepEval** (14.2k stars, larger than RAGAS):
Provides 30+ pre-built metrics including FaithfulnessMetric, HallucinationMetric, and G-Eval (LLM-as-judge with CoT). Key advantage over RAGAS: debuggable/inspectable LLM judge reasoning, native Pytest integration for CI/CD, and the DAG metric builder for custom deterministic evaluation graphs. Use DeepEval when you need production CI/CD integration; use RAGAS when you need research-grade RAG-specific metrics.

## Step 3: Source Independence Check

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

## Step 4: Multi-Source Corroboration Score

**Method — Credibility-weighted aggregation** (MAFC, Scientific Reports 2026):

```
trust_score = Σ(source_credibility_i × support_i) / N_independent_sources
```

Track per-source reliability over time. Separate evidence retrieval agents from
verdict-rendering agents (DelphiAgent cognition/decision split) to prevent retrieval
biases from contaminating final scores.

**Efficiency:** Use importance sampling (FACT-AUDIT, ACL 2025) to focus verification
effort on claims where models are most uncertain, rather than uniformly checking everything.

## Step 5: Uncertainty Quantification (Layered)

Route claims through increasingly expensive methods only when cheaper ones are inconclusive:

| Layer | Method | Cost | When to Use |
|-------|--------|------|-------------|
| 1 | Verbalized confidence + CoT | Cheap | All claims (first pass) |
| 2 | Logit entropy | Medium | If available (white-box models) |
| 3 | Semantic entropy (multi-sample) | Expensive | High-stakes or inconclusive claims |

**Critical:** Never trust raw LLM self-reported confidence without calibration (ICLR 2025).
Always apply post-hoc calibration — Platt scaling or conformal prediction for guaranteed
coverage probabilities (QA-Calibration, ICLR 2025).

## Step 6: Behavioral Consistency Check (Reference-Free)

**TrustBC** (Kim et al., 2024): Generate multi-choice distractors from the LLM's own
response, re-query whether it selects its original answer. Low consistency = low trust.
This is a cheap, reference-free signal that doesn't require external knowledge.

**ChainPoll** (Galileo, 2023): Chain-of-thought prompting + multi-sample voting (typically 20 samples) for hallucination detection. Aggregating multiple CoT judgments significantly improves reliability over single-pass evaluation. Complementary to TrustBC — ChainPoll uses explicit reasoning chains while TrustBC uses behavioral probing. (arXiv:2310.18344)

## Step 7: Adversarial Robustness Testing

Test pipeline against the **Fact-Saboteurs taxonomy** (USENIX Security 2023):
- Evidence manipulation (edits to source text that flip verdicts)
- Claim paraphrasing (minor rewording that evades detection)
- Knowledge poisoning (corrupting reference databases)

Key finding: Even GPT-4 is vulnerable to minor adversarial edits. Require multi-source
agreement to prevent single-source evidence manipulation.

**Hardening:** Adversarially train groundedness checkers using semantically-manipulated
evidence passages (AdvERSEM, *SEM 2025).
