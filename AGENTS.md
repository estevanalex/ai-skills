# AGENTS.md

Working agreement for AI agents (Devin, Claude, etc.) operating in this
repository. Read before making changes.

## Repository purpose

A collection of skills for AI companion apps. A skill is a modular,
framework-aware prompt that makes an AI agent behave like a domain SME on a
specific task, rather than producing generic output. Each skill is
self-contained and lives in its own directory at the repo root.

## Skill structure convention

Every skill follows this layout:

```
<skill-name>/
  SKILL.md                          # entry point: metadata, objective, workflow, non-negotiables
  modules/
    workflow/<NN>-<step>.md         # one module per workflow step, read on demand
    references/<topic>.md           # reusable conventions (tagging, verification, etc.)
    formatting/<template>.md        # output templates
```

Conventions:

- `SKILL.md` is the entry point. It states the objective, the workflow (as
  an ordered list of steps, each linking to its module), the
  non-negotiables that apply on every run, and the known limitations.
- Workflow modules are read on demand at the step they apply — do not
  front-load all modules into context at once. `SKILL.md` should reference
  each by path so the agent can read it when it reaches that step.
- Reference modules hold conventions reused across steps (confidence
  tagging, framework verification). Reference them by path from the steps
  that need them, don't duplicate the content.
- Formatting modules hold output templates. The final step of the workflow
  assembles the output per the template.
- No new external file dependencies: a skill's modules reference each other
  by relative path, all within the skill's own directory.

## Writing and editing skills

- **Specificity over generality.** The default failure mode for AI-generated
  analysis is generic output. Skills exist to force specificity: named
  components over categories, versioned claims over vague ones, concrete
  mechanisms over abstract principles.
- **Every claim that isn't verbatim from a primary source gets a confidence
  tag.** See `incident-report-analysis/modules/references/confidence-tagging.md`
  for the [C]/[I]/[V] convention — reuse it across skills where evidentiary
  grading applies.
- **No unverified framework codes.** ISO/SOC2/NIST/ATT&CK/ATLAS/AI RMF (and
  similar) codes are verified against a current source in the session, not
  recalled from training. See
  `incident-report-analysis/modules/references/framework-verification.md`
  for the verification discipline — reuse it across skills that cite
  frameworks.
- **Controls and recommendations must be implementable this sprint.** A
  principle a consultant could have written without reading the source
  material fails the bar. "Adopt least privilege" fails; "default-deny
  NetworkPolicy per namespace" passes.
- **Contradictions between sources get surfaced, not silently resolved.**
  Show both accounts and say which is authoritative for which fact and why.

## House style

- British/Australian English (e.g. "summarise", "colour", "behaviour").
- No em dashes — use spaced en dashes or restructure the sentence.
- Dense tables over prose wherever content is enumerable.
- Bold key terms, not whole sentences.
- No filler openers ("Great question", "I understand", "Certainly").
- Confidence tags stay visible in every table they apply to — they are
  load-bearing, not draft scaffolding.

## Git workflow

- One feature branch per skill (or per skill change): `<skill-name>-skill`
  for a new skill, `<skill-name>/<topic>` for a change to an existing one.
- Commit messages focus on why, not just what.
- Do not push to `main` directly — work on a feature branch and open a PR.
- Do not force-push or rewrite history on shared branches.

## Verification

This repo has no build or test infrastructure — skills are Markdown
documents, not executable code. Verification is by re-reading:

- After editing a skill, re-read every module touched and confirm all
  `modules/...` cross-references resolve to files that exist.
- Confirm any stated objective or non-negotiable in `SKILL.md` is
  propagated to the modules that implement it (search for the key phrase
  across the skill directory).
- Confirm the report/output template matches the workflow's output-shape
  descriptions (column lists, section order).

## Frameworks in scope

Skills in this repo cite, and must verify against current sources:

- ISO/IEC 27001:2022 Annex A (93 controls, A.5 through A.8)
- SOC 2 Trust Services Criteria (AICPA TSC, CC-series + A/C/PI/P)
- NIST CSF 2.0 (GOVERN, IDENTIFY, PROTECT, DETECT, RESPOND, RECOVER)
- NIST SP 800-53 Rev 5 (including the SR supply-chain family)
- NIST AI RMF (AI 100-1: GOVERN, MAP, MEASURE, MANAGE)
- MITRE ATT&CK (Enterprise / Cloud / Mobile / ICS matrices)
- MITRE ATLAS (`AML.T0xxx` / `AML.TAxxx`)
- CWE / CAPEC (optional, for code-defect root causes)
