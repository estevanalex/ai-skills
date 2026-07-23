# ai-skills

Skills for AI companion apps — modular, framework-aware prompts that make an
AI agent behave like a domain SME on a specific task, rather than producing
generic output.

Each skill lives in its own directory at the repo root and is self-contained:
a `SKILL.md` entry point plus any supporting modules it references.

## Skills

| Skill | Purpose |
|---|---|
| [`incident-report-analysis`](./incident-report-analysis/SKILL.md) | Assess third-party security incident disclosures, breach reports, and vendor post-mortems with evidence-graded, framework-mapped, kill-chain-reasoned analysis. Reframed around prevention + detection + layering always in service of resilience improvement. Covers conventional infra/app incidents and AI/ML-specific incidents. Maps to ISO 27001:2022, SOC 2, NIST CSF 2.0, NIST SP 800-53, NIST AI RMF, and MITRE ATT&CK/ATLAS. |

## License

Apache License 2.0 — see [LICENSE](./LICENSE).
