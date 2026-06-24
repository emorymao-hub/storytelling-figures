# Changelog — storytelling-figures

## v4.0

First public release. A single self-contained `SKILL.md`; the deliverable is one
paste-ready image-generation prompt (Claude), or a prompt plus a natively rendered
image (Codex).

It is positioned as a **guardrail, not a template**: it blocks the weird failures and
leaves everything else to the image model and the user.

**Interaction (intent-first).** Summoned mid-conversation, it first **harvests** the chat
instead of re-interviewing, splits into **understand-it** (converge, at most 3 questions)
vs **produce-it** (diverge, may go conversational), runs a **direction brainstorm**
(2–3 distinct visual directions, always with a "you decide" fallback), then writes the
prompt. A full questionnaire is only the cold-start fallback when there is nothing to
harvest.

**Content guardrails.** Content-first (a figure visualizes an answer you actually worked
out); a deep-dive gate that pushes to the real implementation level and verifies sources;
a **source-lock** allow-list so background words don't become diagram nodes; **input↔output
closure** so no output appears without its input; transparent invocation.

**Visual guardrails.** Default professional register (`lively` cartoon/glow is an opt-in
exception); **anti-tacky rule** — draw each concept as a real object, not a labeled box
wired by arrows or stacked feature-layer slabs; closed data flow; harmonized color;
in-place jargon notes; **clarity > completeness (a floor) > density**; differentiate +
beautify as the supreme law.

**Text & fonts.** All labels (CJK included) rendered in one shot; Arial-family, common
sizes, short words to reduce garble; anti-instruction-leak; one-ellipsis-per-column.

**Meta-rule (anti-overfitting).** A *promotion test*: only a failure mode may become a
rule; a style preference only goes into the inspiration menu. This keeps the skill from
slowly hardening into a rigid template.

**Cross-platform.** Works in both Claude Code (`~/.claude/skills/`) and Codex
(`~/.codex/skills/`); the body is written in platform-neutral actions.

Notable guardrails hardened from real test figures: every connector/line must be
anchored at both ends (no dangling line whose origin is unknown), and the direction
brainstorm is a hard gate (the 2–3 visual directions must actually be surfaced as a
clickable choice before the first figure, not silently skipped).
