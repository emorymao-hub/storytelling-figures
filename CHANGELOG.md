# Changelog — storytelling-figures

## v5.7 — cross-platform consistency: the axis question is a hard gate

Same skill, but Claude ran the full v5 flow while GPT only popped the Step 2.5 ASCII direction choice
and skipped focus/depth/style entirely. Root cause: the axis-surfacing rule wasn't as loud as the
direction hard gate, and didn't spell out the plain-text fallback. Fix: **surfacing focus/depth/style
is now a hard gate at the same tier as the direction choice** ("the direction pick does not replace
the axes — surface both"), with an explicit **platform fallback** (no clickable tool → list options as
plain numbered text; never skip asking just because you can't render cards).

## v5.6 — match the user's language

An English ask ("Help me draw a diagram…") got answered entirely in Chinese (the skill inherited a
project default / the Chinese skill file). New rule: interact and **label the figure in the language
the user asked in** — Chinese ask → Chinese, English ask → English; never default to one language
just because the skill file or project is in it. A clear user override wins.

## v5.5 — don't rush to ship: gather fully, surface every axis, real brainstorm

Tested failures, one root cause — the skill rushed to output instead of helping the user think it
through: (1) an "explain skill vs MCP" ask was classed understand-it and only the direction got
asked while focus/coverage/depth/style were decided silently; (2) the ASCII brainstorm was too crude
to be a real brainstorm; (3) it generated the prompt before the user finished saying what the figure
should express. Fixes: always **surface focus/depth/style as options** (inferred #1, never silent;
understand-it may bundle into 1–2 quick screens); **don't generate until the content is fully in**
(confirm "what else should it show?" first); the **brainstorm is real** — each direction has a core
approach + why-it-differs + a legible sketch, not a skeletal ASCII standing in for a description.

## v5.4 — output contract: body = pure prompt (one select-all to copy), metadata in the filename

Users copy with `Cmd/Ctrl+A → paste`, so the file body must be **only the engineered image prompt**
— no title, no brief, no notes mixed in (they get copied with the prompt and pollute it). All
metadata (concept + intent + style) goes in the **filename** (e.g.
`skill-vs-mcp.understand-ppt.figure-prompt.md`); hints for the user go in the chat reply, not the
file.

## v5.3 — visual effort follows focus (elaborate the core, stay quiet elsewhere)

The figure used the right "arrows not railway" but rendered everything at one uniform elaboration —
the core (the understand/produce fork, the thinking chain) drawn as small as the secondary stages,
while connectors got treated as if they mattered. New rule: **visual effort is the visual form of
focus.** Spend the rich, figurative detail on the focal/core elements (make them the larger heroes,
depicted as real illustrations — converge = many→one, diverge = one→many, a chain of choices
narrowing); keep connectors and secondary elements restrained (a plain arrow is enough). Don't draw
the whole figure at one level of elaboration.

## v5.2 — fix v5.1 + the "remove-the-labels" test + no machine metaphors

Two corrections from real use: (1) v5.1's "default toward complete" was wrong — detailed-vs-simple
is the **user's** coverage choice, not an agent default; the real failure is delivering *below* the
level they chose (e.g. sparse when they asked comprehensive). (2) A new **"remove-the-labels"
test**: cover an element's text — can you still tell what it is? If not (generic icon / empty box,
the word does all the work) it's decoration and fails; each element must depict its real content
(e.g. draw "direction choice" as real ASCII rows). Plus: don't use "icon + one word" throughout,
and draw flow/forks as clean arrows — never a machine metaphor (railway tracks, gears), too
unserious even for a PPT.

## v5.1 — calibration: don't under-build out of garble-fear

Tested reality: image models render legible dense text far better than feared (10+ labels +
sub-panels came out clean), and the actual failure was the opposite — a sparse, **under-built**
figure (a few icons + arrows + whitespace). So "cut labels" means cut *noise*, not make the figure
sparse; default toward **complete and substantial** (every stage with internal detail, fill the
frame), and if worried about garble, generate several and pick. Under-built is far more likely than
garbled.

## v5.0 — heuristic, option-driven guidance

Less freedom, more guidance — the process becomes a **heuristic, step-by-step, options-only**
walk that nudges the user toward the form they want, instead of inferring too much.

- **Read the tone, adapt the question count.** Infer the user's expertise and clarity from how
  they talk: distinctive signals → ask less / not at all; vague → ask. Explain-to-understand:
  1–3 questions. Explain-to-teach / produce: ask more, heuristically.
- **Options, never an interrogation.** Every question is clickable options (your inferred answer
  as #1) with an always-present "your own / you decide" open slot — give angles *and* room.
- **Information density via content coverage, not a density dial.** Don't ask "how dense"; ask
  "how much to cover" (just the core / + key supporting / comprehensive). Density = coverage ×
  depth, as a result the user can actually judge.
- **Register-aware fonts.** `journal` keeps strict Arial-like sans; `ppt` may use a friendlier or
  even **handwritten display font for titles** (a PPT-vs-journal differentiator), legibility first.

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
anchored at both ends (no dangling line whose origin is unknown); and the direction
brainstorm is a hard gate — before the first figure the 2–3 visual directions must be
surfaced as a clickable choice, **each carrying a small ASCII sketch** (with explicit
craft rules so the sketch is actually legible) so the user can compare them without
rendering anything.

The produce-it path runs a short **thinking chain** — a process, not scene-specific
advice: *what's the figure for* (sets register/density) → *what to emphasize* (sets
focus, options enumerated from the user's own content) → *how deep* (sets depth) → the
direction choice. Each step is a clickable choice mapping to one decision, always with a
"you decide" fallback.

**Context-first + deck mode.** The skill checks what the conversation already settled and
**doesn't re-ask the known** (if you're clearly making slides, it won't ask "what for");
inferable axes are confirmed with the inferred answer as option #1. For a **set** of
figures (a slide deck), it sets defaults once and per page asks only what varies —
**depth stays a required, explicit choice on any mechanism/method figure** (never decided
silently), while simple pages just confirm inferred focus/depth and pick a layout.
