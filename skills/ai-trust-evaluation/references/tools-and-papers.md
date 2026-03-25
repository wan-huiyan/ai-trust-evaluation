# Open-Source Tools and Benchmarks

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

## Recommended Evaluation Pipeline (Tiered)

For production systems, use a tiered approach that balances cost and thoroughness:

| Tier | Method | Cost | When to Use |
|------|--------|------|-------------|
| 1 (Fast) | HHEM-2.1-Open or Lynx-8B | Low | All outputs — binary faithful/hallucinated filter |
| 2 (Scored) | DeepEval FaithfulnessMetric or RAGAS Faithfulness | Medium | Outputs passing Tier 1 — scored evaluation with reasoning |
| 3 (Granular) | RefChecker triplet extraction | High | High-stakes outputs — pinpoint specific hallucinated claims |
| 4 (High-confidence) | ChainPoll (20 CoT samples + majority vote) or Semantic Entropy | Expensive | Critical decisions — maximum detection confidence |

Route outputs through tiers based on stakes: low-stakes content stops at Tier 1-2, high-stakes content goes through all tiers.

## Key Research Papers

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
