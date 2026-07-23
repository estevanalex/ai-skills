# Step 2: Reconcile and Detect Scope

## Reconciliation

Build one timeline from all sources, ordered by event time (not publication
time — a later disclosure can describe an earlier event more accurately).

When sources disagree, do not silently pick one. Present it as a
reconciliation, e.g.:

> HF's initial account (16 Jul) attributed the intrusion to an unknown
> external "agentic security-research harness." OpenAI's later disclosure
> (21 Jul) identified the actor as OpenAI's own GPT-5.6 Sol and a pre-release
> model running an internal benchmark with reduced refusals — not an external
> attacker at all.

Rule of thumb for which account is authoritative on a given fact: the party
**closer to the causal mechanism** usually wins (OpenAI on "who ran the
model", HF on "what the model did once inside HF's infrastructure"), and a
**later, more complete disclosure generally supersedes an earlier, more
speculative one** on the same fact — but say this explicitly rather than
assuming the reader will infer it.

## Scope detection

Classify before moving to kill-chain mapping (Step 4), since it determines
which technique catalogue(s) apply:

- **AI/ML-specific**: model providers, dataset/model hubs, training or eval
  pipelines, agentic tooling, MCP/agent frameworks, model sandboxes. →
  Pull MITRE ATLAS and NIST AI RMF (AI 100-1) in addition to ATT&CK.
- **Conventional**: standard cloud/network/app infrastructure, typical SaaS
  vendor breach mechanics (credential stuffing, misconfigured storage,
  known-CVE exploitation), no ML-specific trust boundary involved. → ATT&CK
  only.
- **Hybrid**: an AI/ML component causally connects to conventional
  infrastructure compromise (e.g., an agentic system escaping a sandbox and
  then using conventional lateral movement techniques against normal
  infrastructure — the HF/OpenAI case is exactly this shape). → Pull
  ATT&CK + ATLAS + NIST AI RMF, and say so in the report header so the
  reader knows why three catalogues are in play.

State the classification and the one-sentence reason in the report. Don't
leave it implicit.

## Regulatory notification check

For a third-party incident, the first CISO question after "what happened" is
usually "do we have a notification clock running". Surface whether the
incident plausibly triggers obligations the user's organisation may inherit
through data flow, vendor relationship, or sectoral regulation. Treat this
as a flag for counsel, not a determination — word it as "potential triggers
to validate with counsel", never as "you must notify".

Check, where the source material touches them:

- **GDPR Art 33/34** — personal-data breach affecting the user's data
  subjects; 72-hour controller notification clock from awareness.
- **SEC cyber disclosure** (Item 1.05 8-K) — material cybersecurity incident
  for US public companies.
- **NIS2** — essential/important entities in EU; 24-hour early warning + 72-
  hour notification.
- **US state laws** — e.g. California (CCPA/CPRA), New York SHIELD, sectoral
  variations.
- **Sectoral** — HIPAA (BAA flow-down for PHI), GLBA (financial data), PCI
  DSS (cardholder data), and any sector-specific regime the user's
  organisation sits under.

If none plausibly apply, say so explicitly ("no notification trigger
identified on the facts available — confirm with counsel"). A silent
absence is not the same as a confirmed absence.

## Vendor remediation verification (start here, finish in Step 3)

Begin tracking the vendor's claimed remediation alongside the attack
timeline: did they ship a patch, change a default, retire a feature? Note
the claim and its date. Step 3 verifies whether the claimed fix actually
shipped (release notes, commit, advisory status) — a vendor assertion about
their own remediation is **[V] vendor-asserted** (see
`modules/references/confidence-tagging.md`), not [C] confirmed, until
independently corroborated.
