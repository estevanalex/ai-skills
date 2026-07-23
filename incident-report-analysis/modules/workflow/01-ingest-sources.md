# Step 1: Ingest Sources

## What to do

1. Fetch every URL the user provided using `web_fetch`. Never summarise from
   a link's title or search-snippet alone.
2. For each source, extract:
   - Publication date (incidents get corrected/updated across disclosures —
     date order matters for Step 2)
   - Author/publishing entity (vendor self-disclosure vs third party vs
     downstream analyst commentary — these carry different evidentiary
     weight)
   - Every factual claim, kept as close to source wording as possible
     internally (for your own tracking; final output must still be
     paraphrased per copyright rules)
3. Tag each claim with its source in your working notes, e.g.
   `[HF, 16 Jul] remote-code dataset loader + template injection`.
   `[OpenAI, 21 Jul] GPT-5.6 Sol + pre-release model, reduced cyber refusals`.
4. If the user references a source they haven't linked ("the Varonis
   writeup", "that Simon Willison piece"), search for it rather than asking
   first — only ask if search comes back ambiguous or empty.
5. Actively search for secondary technical analysis (security researchers,
   the vendor's own GitHub issues/release notes, downstream incident
   trackers) even if not requested. Primary disclosures are frequently
   vague by design (legal review, ongoing investigation); secondary sources
   often fill in the version numbers and library names the primary source
   omitted. This is where most of the "specificity" in Step 3 comes from.

## Stop condition

Move to Step 2 once you have: a dated claim inventory per source, and at
least one specificity-oriented search pass beyond the sources the user
handed you directly. Two sources and zero secondary searches is under-
researched for this skill; the whole point is to not stop at the vendor's
press-release-grade account.
