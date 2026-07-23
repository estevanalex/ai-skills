# Step 5: Control Mapping

## The bar for a valid control

A control passes the bar for this skill if a DevOps or dev team lead could
read it and know **what setting to change or what to build**, without
further research, **and** can see in one clause what resilience improvement
it delivers. It fails the bar if it's a principle a consultant could have
written without reading the incident at all, or if it resists an attack but
cannot be tied to a resilience outcome.

Three required elements: (a) the mechanism, (b) a concrete example, (c) the
resilience improvement in one clause. All three. Missing any one fails the
bar.

Fails: "Adopt least privilege." "Improve monitoring." "Implement zero
trust." "Follow secure coding practices." Also fails: "Default-deny
NetworkPolicy per namespace." (good mechanism, no resilience outcome
stated).

Passes: "No cloud IAM role attached to nodes running the eval sandbox;
broker per-task, sub-hour-TTL tokens instead — limits a harvested token to
one task and one hour, so a sandbox compromise can't exfiltrate at
infrastructure scope." Passes: "Kubernetes `NetworkPolicy` default-deny
egress per namespace, independent of any single application-layer proxy —
limits blast radius of a sandbox escape to one namespace, so a single
zero-day doesn't grant cross-namespace movement." Passes: "CI gate fails
the build if any transitive dependency resolves below a named
security-relevant version floor — blocks reintroduction of a deliberately
removed capability (e.g. `datasets<4.0.0`) at merge time, before it reaches
production."

Resilience outcomes to reach for, named explicitly so the clause is
specific not gestural: **reduced blast radius**, **faster detection**,
**shorter time-to-restore**, **fail-safe degradation**, **verified
recoverability**. "Improves resilience" is not a valid clause — name which
one.

## Method

For each kill-chain stage from Step 4:

1. State the practical control (the mechanism, not the principle).
2. Give a concrete example of what implementing it looks like — a specific
   config, gate, or architectural pattern. If you can't produce a concrete
   example, the control is still too abstract; revise it.
3. State the resilience improvement in one clause — which of the named
   outcomes above, and how this control delivers it. If you can't state it
   concretely, the control isn't earning its place; revise or drop it.
4. Map to the relevant frameworks (always default output for this skill,
   per the SKILL.md non-negotiables): ISO 27001:2022 Annex A, SOC 2 Trust
   Services Criteria, NIST CSF 2.0, NIST SP 800-53, and — for AI/ML-specific
   and hybrid incidents — NIST AI RMF (AI 100-1). Verify every control code
   per `modules/references/framework-verification.md` before citing.
5. Where a stage has no clean prevention control (e.g. a zero-day), the
   control should target **detection, friction, fail-safe degradation, or
   verified recoverability** — not prevention. This skill's framing is
   "slow down, detect, and recover from the chain", not "prevent the
   unpreventable." Say this explicitly when it applies, rather than forcing
   a prevention-shaped control onto an unpreventable stage. Recovery
   controls here are not a separate category — they are the resilience
   backstop for the stages prevention and detection can't fully cover.

## Resilience backstop controls (for unpreventable stages)

For zero-day, supply-chain trust, and vendor-remediation-lag stages, reach
for recovery and continuity controls as the resilience improvement — these
are what keep the business running when prevention fails:

- **Tested restore, not just backup** — restore drill from immutable
  snapshots, dated; resilience improvement = verified recoverability within
  a stated RTO.
- **Immutable / air-gapped copies** — snapshots that can't be overwritten by
  a compromised worker; resilience improvement = survives an attacker who
  has write access to the live system.
- **Infrastructure-as-code rebuild** — the whole environment is
  reconstructable from version-controlled config; resilience improvement =
  shorter time-to-restore after a destructive event, and known-good state
  on rebuild.
- **Fail-safe defaults** — default-deny on outage, break-glass with audit;
  resilience improvement = degrades to a safe (read-only / denied) state
  instead of an open one when a dependency is down.
- **Pre-staged comms templates** — notification + customer comms drafted
  against this TTP; resilience improvement = shorter time-to-respond when
  the clock is running.

Map these to **ISO 27001:2022 A.5.29** (information security during
disruption) and **A.5.30** (ICT readiness for business continuity) where
they apply — these are the resilience controls in Annex A, and a SME
mapping to ISO for a board audience should reach for them whenever a stage
shows recovery gaps. Map to **NIST CSF 2.0 RECOVER** (RC.RP, RC.CO) where
the control is about restoration and comms. Map supply-chain-rooted
controls to **NIST SP 800-53 SR** (SR-3 supply chain risk management
processes, SR-5 vulnerability testing, SR-6 supplier assessments, SR-11
component authenticity) where the stage is supply-chain-rooted.

## Layering principle

Do not produce a single perimeter control and call it done — the pattern in
almost every real incident (including HF/OpenAI) is one control point
failing once and the chain running unimpeded afterward (one proxy, one
zero-day, full internet; one dataset loader, full RCE). For each stage,
prefer a control that assumes an earlier control already failed:
segmentation that limits blast radius if the perimeter is breached,
velocity/anomaly detection that catches the actor after initial access
rather than only trying to prevent it, credential scoping that limits what
a harvested token can do rather than only trying to prevent harvesting.

**Layering includes recovery layers, not just preventive and detective
ones.** The "assume an earlier control failed" framing extends to: tested
restore that assumes the destructive event happened, immutable copies that
assume the live system was compromised, fail-safe defaults that assume the
dependency is down. A resilience posture built only on prevention and
detection is one failed control away from total loss; a posture with
recovery layers degrades gracefully instead.

## Output shape

One table: kill-chain stage | practical control + concrete example |
resilience improvement (one clause) | framework mapping (ISO / SOC2 / NIST
CSF / 800-53, + AI RMF for AI/ML/hybrid), followed by a quick-lookup
cross-reference table grouping controls by theme (see
`modules/formatting/report-template.md` for the exact structure used in the
HF/OpenAI reference report).
