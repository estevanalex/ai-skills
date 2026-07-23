# Step 4: Kill Chain Reconstruction

## Method

1. Break the incident into discrete, numbered stages in causal order. Each
   stage should be a single action the actor took (initial access, one
   escalation, one lateral move), not a compressed paragraph covering three
   actions at once. Granularity matters: the point of this step is to find
   every place a control *could* have added friction, and a compressed
   stage hides those seams.
2. For a multi-hop incident (attacker compromises system A, pivots to
   system B), reconstruct each hop as its own mini-chain, then note the
   pivot point explicitly — this is usually the most consequential control
   gap (see the HF/OpenAI case: the pivot from OpenAI's own sandbox into
   HF's production infrastructure was the single highest-leverage point to
   have stopped the whole campaign, because both a proxy zero-day AND a
   dataset RCE had to independently succeed for the full chain to work).
3. Map each stage to the closest-fitting technique ID per the scope
   determined in Step 2 (ATT&CK, ATLAS, or both). It is fine, and expected,
   that some stages (decoy generation, model-driven inference-based
   reconnaissance without tooling) won't have an exact ID — say so rather
   than forcing a mapping. A stretched mapping is worse than an honest gap.
   ATLAS in particular is not comprehensively maintained — honest gaps are
   expected for newer agentic/ML techniques, and the right move is to say
   "no ATLAS technique fits; closest is AML.Txxxx with caveats" or "no
   mapping" rather than to force one.
4. **Pick the ATT&CK matrix per surface, and state it in the report header.**
   ATT&CK is not one catalogue — it has Enterprise, Cloud, Mobile, and ICS
   matrices, and a stage that lives in one often has no equivalent in
   another. A cloud-SaaS breach maps to Cloud techniques that don't exist in
   Enterprise; an ICS vendor breach needs the ICS matrix. Default to
   Enterprise only for conventional endpoint/server incidents; pick Cloud
   for SaaS/cloud-infra stages, ICS for OT/vendor-device stages, and say
   which matrix (or matrices) each stage is mapped against.
5. **Verify every technique ID before citing it** — see
   `modules/references/framework-verification.md`. Do not rely on memorised
   ATT&CK/ATLAS numbering; these get revised and misremembering a technique
   ID undermines the credibility of the whole document for a
   framework-literate reader (which, for a CISO/board audience, is exactly
   who's reading). **Verify sub-technique suffixes too** (`T1611.001`-style)
   — sub-techniques are where mis-citation is most common, and a wrong
   suffix silently narrows or widens what the technique actually describes.

## Output shape

A table: stage number | what happened (specific, from Steps 1-3) | technique
ID + name | confidence tag if the stage itself is inferred rather than
disclosed.

## Why granular stages matter for the next step

Step 5 (control mapping) produces one control per stage. A compressed
3-action stage produces one generic control that has to cover three
different failure modes — which is exactly the abstraction this skill exists
to avoid. Granular stages force granular, specific controls.
