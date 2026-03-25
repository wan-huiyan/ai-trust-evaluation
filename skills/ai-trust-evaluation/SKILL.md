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
  (6) building evaluation pipelines for web-extracted intelligence,
  (7) detecting hallucinations in AI outputs from web sources,
  (8) verifying AI accuracy or asking "how reliable are these AI citations?",
  (9) sources copy each other and you need to detect circular or non-independent evidence,
  (10) comparing evaluation tools like RAGAS vs DeepEval vs HELM for your pipeline.
  Also trigger when users say "how do you know this is true?", "our AI keeps making things up",
  "we need to verify accuracy", "are these citations reliable?", "the AI is hallucinating",
  "AI summaries from web sources", "citations aren't enough", "citations are not enough",
  "don't trust the AI output", "how accurate is the AI", "AI keeps getting things wrong",
  "our AI generates summaries but users don't trust them".
  Covers: the trust taxonomy (relational vs informational), the inversion principle,
  source independence detection, failure mode analysis, persona-based trust UX,
  the 39-issue registry of common pitfalls, and research-backed implementation
  techniques from FActScore, SAFE, Semantic Entropy, RAGAS v0.4+, DeepEval, HHEM,
  Lynx, RefChecker, ChainPoll, and HELM.
  Not for: internal knowledge base RAG with controlled corpora (use standard RAG evaluation),
  bias auditing or fairness assessments, general ML model evaluation (precision/recall/F1),
  standard software testing or unit tests, chatbot implementation or API integration,
  web performance optimization, or any task where the data source is a controlled internal corpus
  rather than the open web or public data.
author: wan-huiyan
version: 5.0.0
date: 2026-03-25
dependencies:
  required: [WebSearch, WebFetch, Read, Write]
  optional: [Bash, Grep, Glob]
scope: >
  Operates on the user's current project or a described product. Does not modify
  code — produces analysis, recommendations, and framework designs as markdown output.
  Reads project files for context (Phase 0) but all changes are user-initiated.
  Safe to run multiple times on the same project (idempotent — re-evaluates from scratch).
input: User description of AI product, architecture, and trust concerns (or project files for auto-detection)
output: Structured trust evaluation report with phased recommendations, technique comparisons, and implementation roadmap
---

# AI Trust Evaluation Framework

This is an **interactive diagnostic**. When triggered, follow the phased process below.
Reference material is in `references/` — read on-demand during Phases 2-4, not upfront.

---

## Process

### Phase 0: Auto-Detect Context (before asking questions)

Silently scan the project to pre-load context before asking anything.

**Step 1: Scan the project** (if working directory is a project, not ~)

```
Glob: **/*trust* **/*confidence* **/*score* **/*citation* **/*source*
Glob: **/*verify* **/*fact* **/*claim* **/*grounding*
Grep: "confidence_score|trust_score|credibility|corroboration|verified"
Grep: "citation|source_url|provenance|ground_truth"
```

**Step 2: Detect product signals** (keyword scan, case-insensitive, 3+ keywords trigger)

| Signal | Detection Keywords | Pre-loaded Context |
|--------|-------------------|-------------------|
| Web Extraction | `scrape`, `crawl`, `extract`, `selenium`, `playwright`, `beautifulsoup`, `httpx`, `public web` | Syndication and source independence issues are primary |
| RAG/Retrieval | `retrieval`, `embedding`, `vector`, `chromadb`, `pinecone`, `weaviate`, `RAG`, `chunk` | RAGAS faithfulness applies, grounding is partially tractable |
| LLM Generation | `openai`, `anthropic`, `claude`, `gpt`, `llm`, `generate`, `prompt`, `completion` | Semantic entropy and behavioral consistency apply |
| Structured Data | `SEC`, `Crunchbase`, `Bloomberg`, `API`, `database`, `SQL`, `BigQuery` | Cross-referencing is feasible for hard facts |
| Dashboard/UI | `dashboard`, `react`, `streamlit`, `gradio`, `frontend`, `component`, `visualization` | Trust UX strategies (visual hierarchy, labels, decay) are relevant |
| Multi-Source | `corroboration`, `multi-source`, `aggregate`, `consensus`, `cross-reference` | Focus on source independence, not basic corroboration |

**Step 3: Check for existing trust mechanisms**

```
Grep: "confidence|trust|verified|citation|source" in UI components or API responses
```

Note what already exists so you don't suggest what they have.

**Step 4: Summarize findings**

"Before we dive in, I scanned your project and found: [signals detected, existing
trust mechanisms, relevant files]. Let me know if I'm reading this right."

Skip Phase 1 questions already answered. If scan reveals nothing, proceed to Phase 1.

### Phase 1: Understand the Product (3-5 questions, one at a time)

Adapt based on answers — skip questions where the answer is already clear.

1. **What does the product do?**
   "Describe your product in one sentence — what does it extract/generate, from where, and for whom?"
   → Classify: extraction / synthesis / generation / hybrid

2. **What output types do users consume?**
   - (a) Hard facts (revenue, HQ, founding year)
   - (b) Synthesized insights (competitive positioning, market trends)
   - (c) Recommendations or predictions
   - (d) Mix — roughly what %?

3. **Who are the users and what decisions do they make?**
   → Classify: Executive (glance, decide) / Consultant (export, present) / Analyst (deep-dive)

4. **What trust mechanisms exist today?**
   - (a) Citations/source links
   - (b) Confidence scores
   - (c) Nothing beyond the output itself
   - (d) Something else — describe

5. **What's the trust failure mode you're most worried about?** (optional, ask if unclear)
   - (a) Users ignoring correct outputs (distrust)
   - (b) Users trusting incorrect outputs (overconfidence)
   - (c) Users not knowing when to trust vs. verify
   - (d) Stakeholders blocking adoption

### Phase 2: Diagnose (automated, no user input needed)

Read `references/failure-modes.md` and `references/strategy-taxonomy.md` for this phase.

Produce a **Trust Diagnosis** covering:

1. **Applicable failure modes** — from the 39-Issue Registry, list only issues relevant
   to this product. Explain WHY each applies to their situation specifically.

2. **Strategy fit matrix** — score each of the 17 strategies as:
   - **High fit** — directly addresses their problem, feasible given architecture
   - **Low fit** — doesn't apply or poor ROI
   - **Risky** — could backfire (explain how)

3. **Missing prerequisites** — what must exist before trust features can work
   (entity resolution? source independence? claim decomposition?)

4. **The "most dangerous failure"** — the specific scenario where their trust system
   would amplify a wrong answer. Make it concrete with a realistic example.

Ask: "Does this match your understanding? Anything I'm missing?"

### Phase 3: Recommend (tailored action plan)

Read `references/verification-pipeline.md` and `references/tools-and-papers.md` for this phase.

Produce:

1. **Recommended architecture** — which of the 3 layers to invest in and why.
   Not all products need all 3 layers.

2. **Phased roadmap** — 3 phases, each with:
   - What to build (specific strategies from the taxonomy)
   - Why this order (dependency graph, ROI reasoning)
   - What research/techniques to use (cite specific papers and tools)
   - What to watch out for (specific failure modes)

3. **Quick wins** — 2-3 things shippable in a sprint:
   - "What We Don't Know" section (high impact, low effort)
   - Temporal freshness signals (almost always applicable)
   - Composite trust score (if users are executives)

4. **What NOT to build** — strategies that are poor fit and why. Be direct.

5. **Verification checklist** — customized with only items that apply:
   - Failure mode for every component
   - Source independence (syndication + circular sourcing)
   - Calibration protocol (Platt scaling or conformal prediction; measure ECE)
   - User research (ELM: depends on persona)
   - Degradation model (sparse data, no authoritative sources)
   - Adversarial threat model (Fact-Saboteurs taxonomy)
   - Decomposition quality (adaptive decomposition, molecular facts)
   - Ensemble verification (no single method > 78% balanced accuracy)

### Phase 4: Deep Dive (optional, on request)

Read relevant `references/` files for implementation details. Provide:
- Implementation details from the verification pipeline
- Specific papers and open-source tools
- Known pitfalls from the Issue Registry
- Example output for their product type

If the user wants to stress-test recommendations, suggest the agent-review-panel skill.

### Error Handling

- **No project context** (user is in ~ or non-code directory): Skip Phase 0, proceed to Phase 1.
- **Minimal input** (e.g., just "trust"): Ask for context — what kind of product, what kind of trust problem.
- **Out of scope** (internal knowledge base, controlled corpus): Acknowledge this is a different problem. Suggest standard RAG evaluation techniques (RAGAS faithfulness on controlled corpus). Do not force-fit the open-web framework.
- **Ambiguous scope** (could be open-web or controlled): Ask one clarifying question about the data source before proceeding.

### Handoff Points

- **To agent-review-panel**: When the user wants adversarial stress-testing of the recommended framework.
- **From standard RAG evaluation**: When a user starts with controlled-corpus RAG but discovers they also pull from open web.
- **To deep-research**: When the user needs current literature on a specific verification technique.

---

## Core Principles (reference during all phases)

Read `references/trust-laws.md` for full context. Key points:

1. **Accountability** — Users trust systems with consequences for being wrong
2. **Track Record** — Error recovery builds more trust than confidence scores
3. **Simplicity** — One number with drill-down beats 17 metadata layers

**The Inversion Principle:** Instead of proving claims correct (unbounded), detect when
the system is likely wrong (tractable). Semantic Entropy is the most validated detector.

**Key Quotes:**
> "The framework's most dangerous failure mode is success."
> "Instead of proving claims correct (unbounded), detect when likely wrong (tractable)."
> "Trust is built through accountability, track record, and simplicity."
