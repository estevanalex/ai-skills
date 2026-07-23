# Report Template

Follow this section order. Omit a section only if genuinely inapplicable
(e.g. no reconciliation section if only one source exists) — never omit the
framework mapping, kill-chain, or resilience-improvement sections; they're
fixed default output for this skill.

## Header

One line: incident name/date, sources used (with dates), scope
classification from Step 2 with one-sentence reason. For AI/ML-specific and
hybrid incidents, state that NIST AI RMF is in scope. State which ATT&CK
matrix (Enterprise / Cloud / Mobile / ICS) is in use, if not Enterprise.
Add a one-line regulatory-notification flag from Step 2 ("potential
notification triggers to validate with counsel: [list or 'none identified'
]") — this is a flag, not legal advice.

## 1. Reconciliation (only if 2+ sources with any disagreement)

Short prose or a small table: what each source claims, where they diverge,
which account is treated as authoritative for which fact and why.

## 2. Attacked Components: Specifics, Not Categories

Table: component | identity (named product/library, not "a tool") |
version/detail | confidence tag. This is the output of Step 3 and is the
section that most differentiates this skill's output from a generic
summary — do not compress it into prose. Where a CVE was identified, include
the CVE ID, CVSS vector, and base score (cited to NVD or the vendor
advisory). Where the vendor claims a remediation, include it with a **[V]**
tag until independently corroborated.

## 3. Kill Chain Reconstruction

Table per Step 4: stage # | what happened | technique ID + name (ATT&CK
and/or ATLAS per scope, with the matrix named if not Enterprise) |
confidence tag if the stage is itself inferred. If multi-hop, clearly
delineate each hop's mini-chain and call out the pivot point.

## 4. Practical Layered Controls, Mapped to Kill-Chain Stage and Framework

Table per Step 5: kill-chain stage | practical control + concrete example |
**resilience improvement (one clause)** | framework mapping (ISO 27001 /
SOC 2 / NIST CSF 2.0 / NIST 800-53, + NIST AI RMF for AI/ML/hybrid,
verified per `modules/references/framework-verification.md`).

The resilience improvement column is load-bearing — it is how this skill
makes the resilience objective visible in every output. Do not omit it, do
not collapse it into the control column. The clause must name which
resilience outcome (reduced blast radius / faster detection / shorter
time-to-restore / fail-safe degradation / verified recoverability) and how
the control delivers it.

## 5. Framework Cross-Reference (quick lookup)

Second table, same controls grouped by theme (network/segmentation,
identity/credentials, secure coding/input handling, monitoring/detection,
incident response, logging/audit integrity, **resilience/recovery/
continuity**, **supply chain**) rather than by stage — this is the version
a reader skims when they want "what does this incident mean for our ISO
surveillance audit" or "what does this mean for our continuity posture"
rather than "walk me through what happened."

## 6. Validate the Resilience Improvement

For each control in Section 4, the validation step that confirms the
resilience improvement actually materialises — resilience is built by
*testing* the control, not just deploying it. A control listed as
"verified recoverability" is only as strong as the dated evidence behind
it.

Table: control (ref to Section 4 row) | validation action | last
performed (date or "not yet scheduled") | owner (if known).

Validation actions to reach for:

- **Tabletop this kill chain** against our environment; walk the chain
  through our current controls and note where it would have stopped,
  detected, or run unimpeded.
- **Test detection rules** against this TTP — replay the technique in a
  sandbox and confirm the alert fires.
- **Run a restore drill** for the asset class the control protects; date
  the drill and the measured RTO.
- **Run a dependency/SBOM query** for the specific named component and
  version floor the control targets; confirm the gate fails as designed.
- **Break-glass / fail-safe drill** — take the dependency down in a
  controlled window and confirm the system degrades to the safe state, not
  an open one.

If a control has no validation action, that is itself a finding — say so
explicitly ("no validation mechanism defined; resilience improvement is
asserted, not verified").

## 7. Action Items

Checklist, each item specific enough to be closed as done/not-done without
further interpretation. "Review our security posture" fails this bar.
"Run an SBOM query across all pipelines for [specific named dependency
below version X]" passes. Action items should cross-reference the
validation actions in Section 6 where applicable (e.g. "schedule restore
drill for Section 4 row 3 control, target RTO 4h").

## Formatting rules (inherit from house style, restated for this skill)

- Dense tables over prose wherever content is enumerable.
- Bold key terms, not whole sentences.
- No filler openers ("Great question", "I understand").
- British/Australian English, no em dashes.
- Confidence tags stay visible in every table they apply to — don't strip
  them in a "clean" final version. The tags are load-bearing, not draft
  scaffolding.
- The resilience improvement column in Section 4 is not optional and is not
  collapsible — it is the visible expression of this skill's objective.
