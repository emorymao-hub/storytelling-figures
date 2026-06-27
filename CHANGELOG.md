# Changelog

## v6.0
First public-polish milestone — the repo now reads as a finished, self-contained skill.
- **README** gains a scannable **Highlights** table, framed as *what a model writing its
  own image prompt won't reliably do — and this skill enforces*: honest content (source-lock,
  correct-first, input↔output closure), no templated look (real objects, remove-the-labels
  test), readable labels (baked in once, register-aware).
- **Examples** trimmed to a single end-to-end worked example, `skill_vs_mcp.md`, with its
  walkthrough aligned to the current three-gate flow — knowledge points · package ·
  **emphasis** — with composition left to the image model (the old "pick a layout" gate is gone).
- **Skill body** is self-contained: style DNA, prompt template + assembly order, and the
  concept-breakdown method are inlined; no external reference files needed.
- **Changelog** added to the repo so the version history is visible.

## v5.0
Heuristic, option-driven flow: fewer free-form questions, more guided choices that converge
the user onto the figure they want. The "pick a layout" gate is replaced by an **emphasis**
gate — you say what to stress, the image model composes. Anti-failure guardrails settle in:
source-lock allow-list, input↔output closure, render-all-text-once, and the remove-the-labels
test against tacky boxes-and-arrows.

## v4.0
First mature milestone: the interaction model plus the core set of anti-failure guardrails
(content-first, register-aware text, no leaked instructions, draw the real object not a box).
