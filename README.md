# ai-skills

Skills for AI companion apps — modular, framework-aware prompts that make an
AI agent behave like a domain SME on a specific task, rather than producing
generic output.

Each skill lives in its own directory at the repo root and is self-contained:
a `SKILL.md` entry point plus any supporting modules it references.

## Skills

| Skill | Version | Purpose | Download |
|---|---|---|---|
| [`incident-report-analysis`](./incident-report-analysis/SKILL.md) | 1.2 | Analyse third-party security incident disclosures, breach reports, and vendor post-mortems to produce evidence-graded, kill-chain-reasoned, framework-mapped control guidance whose every recommendation is tied to a stated resilience improvement. Covers conventional infra/app AND AI/ML incidents. Includes [C]/[I]/[V] confidence tagging, CVSS extraction, regulatory flagging, vendor remediation verification, and validation (tabletop, restore drill, break-glass). | [`incident-report-analysis-1.2.zip`](./releases/incident-report-analysis-1.2.zip) |

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
