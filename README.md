# ai-skills

Skills for AI companion apps — modular, framework-aware prompts that make an
AI agent behave like a domain SME on a specific task, rather than producing
generic output.

Each skill lives in its own directory at the repo root and is self-contained:
a `SKILL.md` entry point plus any supporting modules it references.

## Skills

| Skill | Version | Purpose | Download |
|---|---|---|---|
| [`incident-report-analysis`](./incident-report-analysis/SKILL.md) | 1.1 | Analyse third-party security incident disclosures, breach reports, and vendor post-mortems to produce evidence-graded, kill-chain-reasoned, framework-mapped control guidance whose every recommendation is tied to a stated resilience improvement (reduced blast radius, faster detection, shorter time-to-restore, fail-safe degradation, or verified recoverability). Covers conventional infra/app incidents and AI/ML-specific incidents. Capabilities: source reconciliation with [C]/[I]/[V] confidence-tagged claims; specificity pass (named components, CVEs with CVSS vectors); kill-chain mapping to MITRE ATT&CK (Enterprise/Cloud/Mobile/ICS) and ATLAS; practical layered controls mapped to ISO 27001:2022, SOC 2, NIST CSF 2.0, NIST SP 800-53, and NIST AI RMF; regulatory notification flagging (GDPR/SEC/NIS2/US state/sectoral); vendor remediation verification; and a validation section prompting tabletop, detection-rule, restore-drill, and break-glass testing of every control. | [`incident-report-analysis-1.1.zip`](./releases/incident-report-analysis-1.1.zip) |

## Installing a skill

1. Download the zip for the skill and version you want from the table above
   (or browse all releases in the [`releases/`](./releases/) directory).
2. Create a directory named after the skill in your AI companion app's
   skills directory (e.g. `incident-report-analysis/`).
3. Unzip the downloaded file into that directory. The zip contains
   `SKILL.md` and `modules/` at the root — no parent folder wrapper — so
   the files go directly into the directory you created.
4. Restart your AI companion app (or start a new session). The skill is
   now available as a slash command (e.g. `/incident-report-analysis`) and
   may also be invoked autonomously by the agent when relevant.

   Skills directories by app:

   | App | Skills directory |
   |---|---|
   | Devin CLI (project) | `.devin/skills/` in your project root |
   | Devin CLI (global, Windows) | `%APPDATA%\devin\skills\` |
   | Devin CLI (global, macOS/Linux) | `~/.config/devin/skills/` |
   | Claude Code (project) | `.agents/skills/` in your project root |
   | Claude Code (global) | `~/.agents/skills/` |
   | Windsurf (project) | `.windsurf/skills/` in your project root |

### Pinning a version

Each version has its own zip in `releases/` (e.g.
`incident-report-analysis-1.0.zip`, `incident-report-analysis-1.1.zip`).
Old versions are kept, so you can pin to a specific version if a newer
release changes behaviour you depend on. Check the skill's `SKILL.md`
frontmatter (`metadata.version`) to confirm which version you have
installed.

## License

Apache License 2.0 — see [LICENSE](./LICENSE).
