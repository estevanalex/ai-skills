---
name: incident-report-analysis
metadata:
  version: "1.0"
  created: "July 2026"
  author: "Estevan Chaves"
description: >
  Assess third-party security incident disclosures, breach reports, and
  vendor post-mortems with practical, specific, evidence-driven analysis
  instead of generic summarisation. Trigger whenever the user shares a link
  to, or asks to assess/analyse/compare, a security incident writeup, breach
  disclosure, post-mortem, or vendor security bulletin, including
  multi-source cases where a vendor's account and a third party's account of
  the same incident need reconciling. Also trigger on "turn this into control
  guidance", "map this to ISO/SOC2/NIST", "what should we learn from this
  breach", or "build a kill chain for this incident". Covers conventional
  infrastructure/application incidents AND AI/ML-specific incidents (model
  provider breaches, dataset pipeline compromises, agentic attack tooling, ML
  supply chain compromise). Not for internal incident response or CVE
  reachability in the user's own codebase (use cve-reachability-analyzer).
---

# Incident Report Analysis

Produces evidence-graded, framework-mapped, kill-chain-reasoned assessments of
published security incidents. The output is a reference document the user can
brief upward (CISO/board) or hand to engineering teams as control guidance —
not a news summary.

## Objective

**Prevention + detection + layering, always in service of resilience
improvement.** Every control this skill produces must state, in one clause,
what resilience outcome it delivers — reduced blast radius, faster detection,
shorter time-to-restore, fail-safe degradation, or verified recoverability.
A control that resists an attack but cannot be tied to a resilience
improvement fails the bar, the same way a generic "adopt least privilege"
fails it. Specificity (named components, versioned claims, concrete
mechanisms) is the means; resilience improvement is the end the user is
actually buying.

Recovery and continuity controls are not a separate category here — they are
the resilience backstop for stages where prevention and detection cannot
fully cover the failure mode (zero-days, supply-chain trust failures, vendor
remediation lag). The layering principle in Step 5 explicitly includes
recovery layers alongside preventive and detective ones.

## Why this exists

The default failure mode when asked to "summarise this security incident" is
generic output: restated facts, vague root causes ("a vulnerability was
exploited"), and control recommendations copy-pasted from a best-practices
list that could apply to any incident ever published. This skill exists to
force specificity at every stage: named components over categories, versioned
claims over vague ones, confidence-graded claims over blended fact/inference,
and controls with a concrete implementation mechanism over abstract
principles ("adopt zero trust" is not a control; "default-deny NetworkPolicy
per namespace" is) — each tied to a stated resilience improvement.

## Workflow

Run these steps in order. Each has its own module — read it when you reach
that step, don't front-load all of them.

1. **Ingest sources** → `modules/workflow/01-ingest-sources.md`
   Fetch every source provided (and any the user references but hasn't
   linked — ask if genuinely ambiguous). Extract claims with source + date
   attribution. If multiple sources describe the same incident, do NOT
   merge them yet — keep them separate until Step 2.

2. **Reconcile and detect scope** → `modules/workflow/02-reconcile-and-scope.md`
   Build a single timeline across all sources. Surface contradictions
   explicitly (attribution, root cause, severity) rather than silently
   picking one account. Classify the incident: conventional infra/app
   incident, AI/ML-specific incident, or hybrid. This determines which
   reference modules apply downstream (ATT&CK only, vs ATT&CK + ATLAS +
   NIST AI RMF). Also run the regulatory-notification check and start
   tracking vendor remediation claims (both finished in Step 3).

3. **Specificity pass** → `modules/workflow/03-specificity-pass.md`
   For every vague claim in the primary sources ("a vulnerability was
   exploited", "a third-party tool"), actively search for the named
   component, product, version, or CVE. Grade every specific claim
   confirmed **[C]**, inferred **[I]**, or vendor-asserted **[V]** per
   `modules/references/confidence-tagging.md`. Never present an inferred
   specific as if the primary source confirmed it. When a CVE is found,
   extract the CVSS vector and base score. Verify the vendor's claimed
   remediation actually shipped (commit/release/advisory) — unverified
   vendor remediation stays **[V]**.

4. **Kill chain reconstruction** → `modules/workflow/04-kill-chain-mapping.md`
   Break the incident into discrete stages and map each to MITRE ATT&CK
   (and MITRE ATLAS if AI/ML-scoped per Step 2) tactics/techniques. Pick the
   ATT&CK matrix per surface (Enterprise / Cloud / Mobile / ICS) and state
   it in the report header. Verify technique IDs and sub-technique suffixes
   against source per `modules/references/framework-verification.md` — do
   not cite an ATT&CK/ATLAS ID from memory without checking it.

5. **Control mapping** → `modules/workflow/05-control-mapping.md`
   For every kill-chain stage, produce one practical control: a concrete
   mechanism (a specific setting, gate, or architectural pattern), a
   concrete example, AND a one-clause resilience improvement (which named
   outcome the control delivers). Not a principle. Map each to ISO
   27001:2022 Annex A, SOC 2 TSC, NIST CSF 2.0, and NIST SP 800-53 — plus
   NIST AI RMF for AI/ML/hybrid incidents — verified per the same
   verification module, always. For unpreventable stages (zero-days,
   supply-chain trust, vendor-remediation lag), reach for recovery and
   continuity controls (A.5.29/A.5.30, CSF RECOVER, 800-53 SR/CP) as the
   resilience backstop. Always include the framework mapping; this is not
   optional output for this skill (fixed default).

6. **Assemble output** → `modules/formatting/report-template.md`
   Follow the section order and table structure in the template. Dense,
   scannable, no filler. Bold key terms. Tables over prose wherever the
   content is enumerable.

## Scope detection (Step 2, quick reference)

| Signal in the source material | Classification |
|---|---|
| Model provider, dataset/model hub, training pipeline, agentic tooling, MCP/agent frameworks, model eval sandbox | AI/ML-specific → pull ATLAS + NIST AI RMF (AI 100-1) |
| Conventional network/app/cloud infra, standard SaaS vendor breach, credential stuffing, ransomware | Conventional → ATT&CK only |
| Both present (e.g. an agentic AI system attacking conventional infrastructure, or an ML pipeline compromise leading to conventional lateral movement) | Hybrid → pull ATT&CK + ATLAS + NIST AI RMF, and say so explicitly in the report header |

## Framework mapping is always default output

Per the working agreement for this skill: every report includes the
ISO 27001 / SOC 2 / NIST CSF 2.0 / NIST 800-53 cross-reference table, and
the ATT&CK/ATLAS kill-chain mapping, regardless of whether the user asked
for framework mapping explicitly. For AI/ML-specific and hybrid incidents,
NIST AI RMF (AI 100-1) is added to the same cross-reference table. If the
user wants a lighter-weight output for a specific request, they'll say so —
don't pre-emptively trim this.

## Non-negotiables (apply on every run)

- **Every control states its resilience improvement in one clause.** The
  mechanism is necessary but no longer sufficient — the control must also
  name what resilience outcome it delivers (reduced blast radius, faster
  detection, shorter time-to-restore, fail-safe degradation, or verified
  recoverability). Fails: "Default-deny NetworkPolicy per namespace." (good
  control, no resilience outcome stated). Passes: "Default-deny
  NetworkPolicy per namespace — limits blast radius of a sandbox escape to
  one namespace, so a single zero-day doesn't grant cross-namespace
  movement." See `modules/workflow/05-control-mapping.md`.
- **No generic root causes.** If the source says "a vulnerability", search
  for the actual library/product/version before writing the sentence. If it
  can't be found, say so explicitly ("not disclosed by [source] as of
  [date]") rather than writing around the gap with vague language.
- **No unverified framework codes.** ISO/SOC2/NIST/ATT&CK/ATLAS/AI RMF
  codes get checked against source, not recalled from memory. See
  `modules/references/framework-verification.md`.
- **No best-practices-list controls.** Every control needs a stated
  mechanism a DevOps/dev team could implement this sprint. "Adopt least
  privilege" fails this bar. "No cloud IAM role attached to eval-sandbox
  nodes; broker per-task scoped tokens instead" passes.
- **Confidence tagging is mandatory on every specific claim** that isn't
  verbatim from a primary source. See `modules/references/confidence-tagging.md`.
- **Contradictions between sources get surfaced, not resolved by silent
  selection.** If HF says one thing and OpenAI says another about the same
  event, show both and say which one is authoritative and why (e.g. the
  party closer to the causal mechanism, or the later/more complete
  disclosure).

## Reference files

| File | Purpose |
|---|---|
| `modules/references/confidence-tagging.md` | The [C]/[I]/[V] convention and when to apply it |
| `modules/references/framework-verification.md` | How to verify ATT&CK/ATLAS/ISO/SOC2/NIST/AI RMF codes before citing them |
| `modules/formatting/report-template.md` | Output section order and table skeletons |

## Known limitations

- This skill produces analysis of *published, third-party* disclosures. It
  does not replace an internal IR process for the user's own incidents.
- Framework code verification depends on web search access. If search is
  unavailable, the report must flag every framework citation as unverified
  rather than presenting them as checked.
- Attribution chains across multiple disclosures (as in the HF/OpenAI case)
  can still change after publication — date-stamp every source used and note
  that later corrections may supersede this analysis.
- Resilience and recovery controls are only as good as the last drill — this
  skill recommends them and the report's validation section prompts testing,
  but it cannot verify a drill was actually run. A control listed as
  "verified recoverability" is only as strong as the dated evidence behind
  it.
