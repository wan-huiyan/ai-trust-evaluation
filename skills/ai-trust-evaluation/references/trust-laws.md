# The Three Laws of Trust (Meta-Framework)

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

> Instead of trying to prove claims are correct (an unbounded problem), focus on
> detecting when the system is likely wrong (a tractable problem).

Research-backed implementation: **Semantic Entropy** (Kuhn et al., Nature 2024) provides
the most validated "likely wrong" detector. Generate N responses (5-10) to the same query,
cluster by meaning using NLI (DeBERTa), compute entropy over clusters. High semantic
entropy = the model is uncertain = likely wrong. Task-agnostic, no training data needed.

For cheaper approximation: **Semantic Entropy Probes** (Kossen et al., ICLR 2025) achieve
similar detection from a single forward pass using hidden-state probes — 5-10x cheaper.
