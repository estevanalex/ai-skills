# Confidence Tagging Convention

Three tags, used inline on every specific claim that isn't a direct
restatement of something a named primary source states outright.

- **[C] Confirmed** — a named primary source (the vendor's own disclosure,
  or another directly-involved party) states this directly. Cite the
  source in the surrounding sentence or a table column, not just the tag.
- **[I] Inferred** — reconstructed from secondary analysis, technical
  reasoning, or informed speculation from people close to the relevant
  codebase/system, but not confirmed by a party with direct knowledge.
- **[V] Vendor-asserted** — the vendor's own claim about their remediation
  or controls (e.g. "we have patched this", "the issue is resolved", "no
  customer data was affected"). This is a distinct evidentiary class from
  [C]: [C] covers the vendor's description of what happened during the
  incident, [V] covers the vendor's assertion about the state of their own
  fix or controls after the incident. Vendor self-claims about their own
  remediation carry an inherent conflict of interest and stay **[V]** until
  independently corroborated (a verifiable commit, release, advisory
  status, or third-party confirmation), at which point they upgrade to
  **[C]**. This matters directly for resilience framing — a control that
  depends on a vendor patch the vendor merely claims to have shipped is not
  a resilience improvement yet.

## Rules

1. Tag at first mention. If you restate the claim later in the same
   document (e.g. in a summary or action-items section), it stays tagged —
   repetition doesn't upgrade [I] to [C], and it doesn't upgrade [V] to [C]
   either. Only independent corroboration upgrades a tag.
2. If a claim mixes confirmed, inferred, and vendor-asserted elements
   ("the loader was HF's `datasets` library **[I]**, which harvested cloud
   credentials from the compromised worker **[C]**; Hugging Face states the
   loader has been removed in 4.0.0 **[V]**"), tag each element separately
   rather than tagging the whole sentence with the weakest of the three —
   this loses information a reader needs.
3. Never present an [I] claim without at least a one-clause reason for the
   inference (why you believe it, not just that you believe it). "Almost
   certainly refers to X, because Y" — not just "likely X." For [V] claims,
   cite the vendor statement (URL + date) so the reader can see the
   assertion and its provenance.
4. If, after a genuine search effort, no specific detail can be found for a
   vague claim, don't force an [I] tag onto a guess. Write "not disclosed
   by [source] as of [date]" instead. A gap honestly stated is more useful
   than a low-confidence guess dressed up as inference.
5. A confidence tag describes the claim's evidentiary status, not the
   report author's certainty in prose form. Don't hedge in the sentence
   *and* tag it — pick one. ("This might possibly be the datasets library
   [I]" is redundant hedging; "This is the datasets library [I]" with the
   tag doing the hedging work is correct.)
6. **[V] is the default for unverified vendor remediation claims.** If a
   vendor says "fixed" and you cannot point at the commit/release/advisory
   that confirms it, the claim is [V] — not [C], and not omitted. A
   resilience-improvement clause that depends on a [V] patch should say so
   ("depends on vendor patch [V], not yet independently verified").
