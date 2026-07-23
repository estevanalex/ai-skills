# Framework Code Verification

## Why this module exists

Control codes (ISO 27001 Annex A numbering, NIST CSF 2.0 subcategories,
NIST SP 800-53 identifiers, NIST AI RMF function/category codes, MITRE
ATT&CK/ATLAS technique IDs) are exactly the class of fact that gets
misremembered with high confidence: they're numerous, structurally similar
to each other, and revised between framework versions (ISO 27001:2013
Annex A numbering is completely different from ISO 27001:2022's; NIST CSF
1.1 categories don't map 1:1 onto CSF 2.0's; NIST AI RMF is new enough that
its category numbering is unfamiliar to most readers). A wrong control code
cited to a CISO or auditor-literate reader is a credibility failure
disproportionate to how small the error looks.

**Treat every framework code as unverified until checked against a current
source in this conversation, not as something known from training.**

## What to verify and how

- **MITRE ATT&CK**: search or fetch `attack.mitre.org` for the specific
  technique ID before citing it (e.g. confirm T1611 is actually "Escape to
  Host" and not something renumbered). **Verify sub-technique suffixes too**
  (`T1611.001`-style) — sub-techniques are where mis-citation is most
  common, and a wrong suffix silently narrows or widens what the technique
  actually describes. If you're mapping a stage that doesn't cleanly fit an
  existing technique, say so rather than forcing the nearest ID. Also
  confirm you're citing from the right matrix (Enterprise / Cloud / Mobile /
  ICS) for the surface in question — a technique that exists in one matrix
  often has no equivalent in another.
- **MITRE ATLAS**: search or fetch `atlas.mitre.org`. ATLAS uses its own
  `AML.T0xxx` / `AML.TAxxx` numbering distinct from ATT&CK — don't guess at
  IDs by analogy to ATT&CK numbering, they're independently assigned. Note
  that ATLAS is not comprehensively maintained — for newer agentic/ML
  techniques, an honest "no ATLAS mapping" is correct more often than a
  forced one.
- **ISO/IEC 27001:2022 Annex A**: verify against the current 2022 Annex A
  structure (93 controls across 4 themes: Organizational, People, Physical,
  Technological — numbered A.5 through A.8). Explicitly confirm you're using
  2022 numbering, not 2013 — if the source material or the user's context
  implies an older certification, flag the version mismatch rather than
  silently using 2022 codes. For resilience and recovery controls, reach
  for **A.5.29** (information security during disruption) and **A.5.30**
  (ICT readiness for business continuity) — these are the Annex A controls
  that map to the resilience objective and are easy to overlook in favour
  of preventive controls.
- **SOC 2 Trust Services Criteria**: verify against the current AICPA TSC
  structure (CC-series for Common Criteria, plus Availability/Confidentiality/
  Processing Integrity/Privacy where in scope). Don't cite a CC subcode
  without checking it maps to the control theme you're claiming. For
  resilience outcomes, the **Availability** criterion (A-series) is the
  primary mapping — verify the specific A subcode rather than citing "A"
  generically.
- **NIST CSF 2.0**: verify function/category/subcategory codes (GOVERN,
  IDENTIFY, PROTECT, DETECT, RESPOND, RECOVER, each with lettered
  subcategories) against the current 2.0 structure — this is a different
  function set from CSF 1.1 (which lacked GOVERN as a standalone function).
  For resilience outcomes, **RECOVER** (RC.RP Recovery Plan, RC.CO
  Communications) is the function to reach for — verify the specific
  subcategory rather than citing "RECOVER" generically.
- **NIST SP 800-53**: verify control family and control number/enhancement
  against the current revision (Rev 5 as of this skill's authoring) — don't
  cite a control ID without confirming the family prefix (AC, SC, SI, CM,
  IA, IR, RA, AU, etc.) matches the control's actual content. For
  supply-chain-rooted stages, reach for the **SR family** (SR-3 supply
  chain risk management processes, SR-5 vulnerability testing, SR-6
  supplier assessments, SR-11 component authenticity) — Rev 5 elevated SR
  to a full family and it is the natural 800-53 mapping for ML-supply-chain
  and dependency-pipeline compromises. For resilience/continuity, reach for
  **CP** (Contingency Planning) family controls (CP-2, CP-9, CP-10).
- **NIST AI RMF (AI 100-1)**: verify against the current NIST AI 100-1
  structure (four functions: **GOVERN**, **MAP**, **MEASURE**, **MANAGE**,
  each with categories and subcategories). Used for AI/ML-specific and
  hybrid incidents alongside ISO/SOC2/CSF/800-53. Don't cite an AI RMF
  category from memory — the structure is new enough that numbering is
  unfamiliar and easy to confuse. Verify at `nist.gov/itl/ai-risk-management-
  framework` or the current NIST AI 100-1 reference.
- **CWE / CAPEC (optional, for code-defect root causes)**: when the root
  cause is a specific code defect (deserialization, injection, path
  traversal), consider citing the **CWE** weakness class (verify at
  `cwe.mitre.org`) and/or the **CAPEC** attack pattern (verify at
  `capec.mitre.org`). These complement ATT&CK: ATT&CK describes adversary
  behaviour, CWE describes the weakness that made the behaviour possible,
  and CWE is what drives secure-coding controls. Optional, not default
  output — use when the root cause is clearly a code defect and a CWE
  class sharpens the control recommendation.

## If verification isn't possible

If web search/fetch is unavailable in the current session, do not silently
fall back to memory and present codes as checked. State explicitly in the
report: "Framework codes below are drawn from general knowledge and have
not been verified against current source in this session — confirm before
citing externally." This caveat is a floor, not a substitute for actually
verifying when the tools are available — verification is the default
behaviour, this fallback is only for genuine tool unavailability.

## Efficient verification pattern

You don't need one search per code. Batch: search once for "ISO 27001:2022
Annex A control list" to get the authoritative full list, then map multiple
stages against that single retrieved reference rather than searching per
control. Same pattern for NIST CSF 2.0's subcategory list, the NIST AI RMF
category list, and the ATT&CK technique catalogue for the relevant tactic
(e.g. one search for "MITRE ATT&CK privilege escalation techniques" covers
several stages if multiple stages involve escalation).
