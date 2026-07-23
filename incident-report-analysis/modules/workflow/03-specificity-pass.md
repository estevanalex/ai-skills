# Step 3: Specificity Pass

## The core discipline

Every vague technical phrase in a primary disclosure is a search query, not
a sentence to restate. Vendor disclosures are systematically vague about
component/version/CVE detail — legal review, ongoing investigation, and
liability concerns all push toward generic language ("a vulnerability",
"a third-party tool", "a code-execution path"). Your job is to de-generalise
this wherever the information exists publicly, and to say clearly when it
doesn't.

## Method

For each vague claim:

1. Identify what's missing: product name? version? CVE? specific mechanism
   (which flag, which config field, which serialization format)?
2. Search for it. Good targets: the vendor's own GitHub repo/release notes,
   independent security researcher writeups (these often do the forensic
   legwork the vendor's PR-reviewed post won't), CVE databases, Hacker
   News / community discussion threads attached to the disclosure (frequently
   contain informed speculation from people who read the vendor's source
   code — cite this as informed inference, not fact).
3. If found: state it, cited, confidence-tagged **[C]** if a primary source
   confirms it, **[I]** if it's a plausible reconstruction from secondary
   analysis.
4. If not found: say so explicitly. "Not disclosed by [vendor] as of
   [date]" is a valid, useful sentence — better than papering over the gap
   with vague language that reads as more confident than the evidence
   supports.
5. **When a CVE is identified**, extract the CVSS vector and base score
   (v3.1, or v4.0 where the advisory provides it) from NVD or the vendor
   advisory. A SME always wants the vector + base score for prioritisation —
   the score alone is less useful than the vector, which shows *why* it
   scores where it does (network vector, low complexity, no privileges, high
   impact). Cite the source (NVD entry URL or advisory).
6. **Verify the vendor's claimed remediation actually shipped** (started in
   Step 2). Check the vendor's release notes, GitHub commits/tags, security
   advisory status (fixed / patched / withdrawn). A vendor statement "we
   have fixed this" is **[V] vendor-asserted** until you can point at the
   commit, release, or advisory that confirms it — at which point it
   upgrades to **[C]**. If the claimed fix cannot be located in any
   verifiable artefact, say so explicitly: "remediation claimed by [vendor]
   on [date]; no corresponding release/commit located as of [date]". This
   matters directly for the resilience framing — a control that depends on
   a vendor patch that hasn't actually shipped is not a resilience
   improvement yet.

## Worked example (from the HF/OpenAI incident)

Vague claim: "a malicious dataset abused two code-execution paths... a
remote-code dataset loader."

De-generalised: cross-referencing independent analysis, this almost
certainly refers to Hugging Face's `datasets` Python library, which removed
its `trust_remote_code=True` flag entirely in the 4.0.0 release (July 2025).
If the attack used this library post-4.0.0, the most likely mechanism is a
dependency pin to `datasets<4.0.0` in the malicious dataset's requirements,
reintroducing a capability the maintainers had deliberately removed.
**[I]** — not confirmed by Hugging Face directly.

Note what this unlocks for the control-mapping step: "harden your dataset
loader" (generic, useless) becomes "run an SBOM gate that fails CI if any
transitive dependency resolves to `datasets<4.0.0`" (specific, actionable,
testable this sprint).

## Confidence tagging convention

See `modules/references/confidence-tagging.md` for the full rule. Short
version: **[C]** = a named primary source states this directly. **[I]** =
reconstructed/inferred from secondary analysis, absent explicit vendor
confirmation. **[V]** = the vendor's own claim about their remediation or
controls (distinct from [C], which covers the vendor's description of what
happened) — stays [V] until independently corroborated. Never drop the tag
once assigned, including in summary sections — a claim doesn't get more
confirmed by being repeated later in the same document.
