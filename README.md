# storytelling-figures

> A **Claude Code / Codex skill** that turns a conversation into a clean, paste‑ready **image‑generation prompt** for scientific, conceptual and explanatory figures — and quietly keeps the model from making the usual weird mistakes.

You're chatting with an AI about some mechanism — how an LLM tokenizes a typo, how an agent picks a skill, your own method's pipeline. At some point you think *"this is hard to explain, let me just draw it."* This skill is what catches that moment: it reads the conversation you already had, engineers a single high‑quality prompt for **GPT image‑2 / nano‑banana**, and hands it back ready to paste.

It is **not** a figure renderer and **not** a chart library. Its whole job is to write *one good prompt* and avoid the failure modes that make AI‑generated figures look wrong or tacky.

---

## What it actually does for you

The model can already write an image prompt. What it can't reliably do on its own is avoid these:

- **Gibberish / dropped labels** — Chinese and dense text garble. The skill bakes every label in once, with explicit "render exactly" instructions and short‑word budgeting.
- **Made‑up / leaked content** — a background word from the chat ("the model") sneaks in as a diagram concept. A *source‑lock* allow‑list blocks that.
- **Logical inconsistency** — an output appears with no matching input. An *input↔output closure* check forbids half‑drawn examples.
- **Tacky, dated look** — stacked "feature‑layer" slabs, everything a labeled rectangle wired by arrows. Hard rule: draw each concept as the **real object**, not a box.
- **Not scientific enough** — defaults to a professional schematic register, not a glowing‑robot cartoon.
- **Imprecise wording** — for deep/implementation figures it verifies the real mechanism instead of hand‑waving.
- **Font / layout traps** — Arial‑family, common sizes, one‑ellipsis‑per‑column, no instruction text leaking into the image.

Everything else — the metaphor, the palette, the composition — is left to you and the image model. The skill stays a guardrail, not a template.

---

## Install

It's pure markdown — no dependencies, no API keys. Just drop the folder into your skills directory.

**Claude Code**
```bash
git clone https://github.com/emorymao-hub/storytelling-figures.git \
  ~/.claude/skills/storytelling-figures
```

**Codex**
```bash
git clone https://github.com/emorymao-hub/storytelling-figures.git \
  ~/.codex/skills/storytelling-figures
```

(Or download the ZIP and unzip into that path.) Start a new session — the skill auto‑discovers and triggers on intent.

---

## How to use it

You don't invoke it by hand. Talk through a concept, then say something like:

- *"draw what we just discussed"*
- *"this is too confusing — draw it so I get it"*
- *"I need a journal method figure, give me some layout directions"*

It then:
1. **Harvests** the conversation (content, focus, depth) instead of re‑interviewing you.
2. **Splits by intent** — *understand‑it* (converge fast, ≤3 questions) vs *produce‑it* (diverge, can go conversational).
3. **Brainstorms 2–3 distinct visual directions** for you to pick (always with a *"just do it best"* fallback).
4. **Engineers the prompt** and delivers it.

**Output by platform:** on **Claude** you get one `figure-prompt.md` to paste into the image model (generate 3–4, pick one). On **Codex** with a native text‑to‑image tool, you get the prompt **and** the rendered image together. Neither ever hand‑codes SVG/PNG.

---

## What's inside

```
SKILL.md       # the whole skill: workflow + guardrails, self-contained
README.md      # this file
CHANGELOG.md   # design summary — why the guardrails exist
LICENSE        # MIT
```

`SKILL.md` is self-contained — the workflow, the two hard text constraints, the style
families, and the full pre-delivery self-check all live in one file. The
*"promotion test"* meta-rule described in the changelog is what keeps the skill from
slowly turning into a rigid template.

---

## Scope

Draws **mechanism / concept / process schematics**. For data charts use a plotting skill; for full slide decks use a presentation skill. It writes prompts and (on Codex) renders — it never substitutes a hand‑coded SVG.

## License

MIT © 2026 emorymao‑hub
