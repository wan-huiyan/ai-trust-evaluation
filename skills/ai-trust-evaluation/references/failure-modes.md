# Critical Failure Modes (The "39-Issue Registry")

## The Most Dangerous Failure: "Success"

A false claim that appears in multiple syndicated sources, with a recent publication date,
from "credible" outlets will receive the **highest** confidence score. Users trust it more
than they would with no trust framework. The system amplifies wrong answers.

## Source Independence Problem

Web content is massively duplicated. Press releases get syndicated verbatim to dozens of
outlets. "Confirmed by 5 sources" when all 5 trace to 1 press release is worse than no
signal — it manufactures false confidence.

**Fix:** Implement source clustering (embedding similarity > 0.95, hyperlink graph analysis,
publication timestamp proximity) before counting corroboration. Report "confirmed by N
independent source clusters," not "confirmed by N sources."

## Circular Sourcing

Distinct from syndication: Source A cites Source B, Source B cites Source A. Creates
mutual-corroboration illusion. Requires citation-graph cycle detection, not just content dedup.

## The Streetlight Effect

Verified badges (Item 7) only work for hard facts (~15% of output). The valuable majority —
synthesized insights, competitive analysis, trend identification — has no verification
pathway. Building verification for the easy part and ignoring the hard part creates false
sense of completeness.

## Cognitive Overload

17 trust signals displayed simultaneously paralyze users. Trust signals should reduce
cognitive load, not add to it. Design for personas (supported by ELM research):
- **Executive:** One composite score, drill-down optional (peripheral route)
- **Consultant:** Exportable proof with corroboration and verification badges (central route)
- **Analyst:** Full transparency, counter-evidence, gap analysis (central route, high elaboration)

## Common Missing Prerequisites

These are frequently omitted from trust frameworks:
- Entity resolution/disambiguation (upstream of everything)
- Source independence detection (upstream of corroboration)
- Language/locale handling (biases against non-English sources)
- Offline/export mode (consultants need trust metadata in PowerPoint)
- Versioning/audit trail (what was shown last week vs today)
- Multi-tenancy isolation (competing firms' corrections leaking)
- Paywalled source handling (highest-authority sources are often inaccessible)
- GDPR/data retention policies
