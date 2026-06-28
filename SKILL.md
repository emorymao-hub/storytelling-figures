---
name: storytelling-figures
description: >-
  After a conversation, turn a concept or mechanism you just worked through (or are still
  trying to grasp) into a single paste-ready image-generation prompt (figure-prompt.md) for
  GPT image-2 / nano-banana. Two trigger families —
  [WANT TO UNDERSTAND / EXPLAIN] this is too complex, draw it so I get it / I learned it and
  need to explain it to my advisor or lab, give me a figure / turn what we just discussed into
  a diagram / draw me a figure to explain this;
  [WANT TO PRODUCE / FIND INSPIRATION] a mechanism/schematic figure for slides / I designed a
  method and need a journal-style method figure / give me layout & composition ideas /
  concept figure, mechanism figure, process schematic, cover figure, storytelling figure,
  figure prompt.
  It always draws a MECHANISM / CONCEPT / PROCESS schematic — for experimental results or data
  charts use a plotting skill, for full slide decks use a presentation skill.
  Once triggered it first re-reads the conversation and harvests what was discussed, then runs
  three quick choices (knowledge points / package / emphasis) before writing the prompt.
  Delivery is platform-split: on Claude it hands back one .md; on Codex it hands back the
  prompt plus a natively generated image; it never hand-codes SVG/PNG as a substitute.
  中文触发（同样适用）：把刚才讨论的画成示意图 / 太复杂了画出来理一理 / 帮我画个图解释这个 /
  我要做 PPT 的机制配图 / 画一张 journal 方法示意图 / 给我点布局排版灵感 / 出图 prompt。
---

# storytelling-figures

Turn **a concept you just talked through (or are stuck trying to understand)** into a single
`figure-prompt.md` — a plain-text prompt you paste into GPT image-2 / nano-banana to get a
narrative / conceptual / explanatory / mechanism figure. The deliverable is always a `.md`
(works in both Claude and Codex).

This skill's whole job is to write **one good prompt** and keep the image model from making
the usual weird mistakes. Everything else — the metaphor, palette, composition — is left to
you and the image model. It is a **guardrail, not a template**.

## Run contract · read this first (follow every time; if anything below conflicts, this section wins)

**Four steps:**

1. **Brainstorm + get the content right** — harvest the conversation / work out the answer; lay out all the knowledge points and make sure the mechanism is correct (**correct first, then draw**).
2. **Three choices (🛑 three hard gates, generate only after they're answered)** — ① knowledge points (≤5) ② package (sci-comm / PPT / detailed / journal) ③ emphasis (comparison / at-a-glance / walk-through / mechanism …).
3. **Assemble the prompt** — compose the necessary info into a prompt, **don't over-specify; leave room for image-2 to perform**.
4. **Safeguard** — run the red-line self-check before generating.

The user only **interacts at Step 2 — those three choices**; the harvest (Step 1), assembly (Step 3) and self-check (Step 4) are your work.

**🛑 The three gates fire every time, generate only after the user has chosen.** The ONLY exception is the user explicitly saying "you decide / just draw it / stop asking." **Never self-authorize because "the content is clear" and generate silently** (the most common failure).

**Composition belongs to the model.** Gate ③ only gives a one-line "what to emphasize" note — **no ASCII, no dictating panel count / left-right / top-bottom** — the layout is for image-2 to work out from this specific problem (tested: forcing a layout came out worse than free-form).

**Two hard constraints (correctness, always on):** ① all text rendered in one shot (CJK included, nothing blank, garble accepted); ② anti-instruction-leak (prompt directives must not appear in the image).

**Delivery by platform:** Claude delivers `figure-prompt.md` only, no render; Codex delivers prompt + native text-to-image (after the three gates are answered). **Never hand-code SVG/PNG as a stand-in.**

**Only draws** mechanism / concept / process schematics; data charts → a plotting skill, full decks → a presentation skill.

> **⚠️ Supreme law (overrides every other rule here): every figure is different, each tells its own story, and image-2 is used to make it look good. Same-face sameness / crude boxes = total failure.** Constraints are only two layers: **hard constraints** (the two above — correctness, always on) + **inspiration menu** (palette / layout / texture / metaphor … a menu, not a checklist; pick a few, vary them, give each figure its own signature).

---

## Identity (remember when you're called)

- **You're usually called mid-conversation:** the user was chatting and now wants a figure, or worked it out and wants to show it. **First thing is to catch the conversation and harvest what's there, not start a fresh questionnaire.**
- **Be transparent about invocation:** the first working message says plainly "I'll use storytelling-figures to turn this into a figure prompt/image" — don't silently draw; the user must be able to tell the skill was invoked.
- **Always clickable options + a "you decide" fallback:** every question uses **clickable options** (your inference first) + always one «**present it the best way (you decide)**» — **never force free-text typing**. The user may not want to choose; then you pick the best and proceed.
- **Read the tone to set how much you ask:** infer expertise and clarity from the user's wording — **clearly expert / clearly lay / very specific ask → ask little or nothing; vague / unreadable → treat as unclear, ask** (understand-it: 1–3 questions; explain/produce: a few more, heuristic-style). ⚠️ **"ask less" ≠ skip the three gates:** the gates are the user's presentation choices; "ask less" just means putting your inference first so they confirm in one tap, not deciding for them.
- **Context first, don't re-ask the known:** check what the conversation already fixed (platform, whether it's a deck, audience, whether focus is inferable). Don't ask the known; for the inferable, "put my guess as option 1" to confirm fast.
- **Language follows the user's request:** ask in Chinese if they wrote Chinese, English if English, and **label the figure in that language** — don't default to Chinese just because this file is in Chinese.
- **Cross-platform (Claude / Codex):** this doc names **actions**, not one vendor's tools — "ask with clickable options" (Claude: `AskUserQuestion`; Codex/others: plain-text list), "spawn a research sub-agent / check the official source" (Claude: a guide agent; Codex: its own research/search).

> Boundary: deliver per platform, but **never hand-code SVG/PNG/HTML as a stand-in**; no data charts (→ a plotting skill); no full decks (→ a presentation skill); no guarantee of "swap one exact element".

---

## Step 1 — Brainstorm + get the content right (correct first, then draw)

**Goal: lay out everything the figure will say, and make sure the mechanism is correct.** Do not move to Step 2 until the content is right.

1. **Harvest the conversation → content map + allow-list.** Organize what's been discussed into a "content map", and build a **visible-content allow-list** (`words/entities allowed to draw` / `generic conversation terms banned from the figure` / `terms to verify`). E.g. if the user only asked to explain `fucntion → function`, the allow-list is just those plus the relevant mechanism words; a generic "the model" mentioned in passing must not become "the model's direction" or `logit(model)`.
2. **Content first.** A figure visualizes an **answer you've actually worked out**, not topic decoration. Every element you'll draw maps to one real piece of content.
3. **Deep-dive reaches the implementation level, sourced.** For research/deepest depth, or understand-it going to implementation level, Phase A must reach **concrete data structures / algorithms / decision branches**, each **sourced**. **Never hand-wave with vague verbs** ("recognition / matching / similarity / processing" — a real past failure: writing "default = direct attention, no retrieval" as "similarity scoring"). **No "I happen to know"** — actually check the source / spawn a research sub-agent, and put the real mechanism names (attention, cosine similarity, top-k, dynamic injection …) on the figure (mechanism/numbers must be sourced).
4. **Cold-start fallback.** In the rare case the user opens with "just draw me an X" and there's no prior conversation to harvest — only then work out the answer from scratch (Phase A: get the real mechanism fully and correctly, produce the content map), then go to Step 2.

---

## Step 2 — Three choices (🛑 three hard gates, generate only after they're answered)

> 🛑 **"The content is clear" does NOT license skipping these.** The three gates decide **presentation** (which points to feature, which package, what to emphasize) — **they stay the user's choice even when the content is 100% clear**. **Never decide "it's obviously X" and generate silently** (tested failure: on Codex it self-picked the "detailed teaching figure" package and generated with no options offered). **The ONLY exception is the user explicitly saying "you decide / just draw it / stop asking."** Note: handing **composition** to the model in Step 3 is a separate matter — **presentation belongs to the user, composition belongs to the model; neither is yours (the agent) to decide silently.**

Each gate uses **clickable options** (your inference first) + a "you decide" fallback; with `AskUserQuestion` use cards, otherwise a plain-text numbered list (`1. … 2. … 6. you decide`).

### ① Knowledge points (≤5)

On invocation, **brainstorm immediately**: lay out **all the possible knowledge points / sub-topics** in the content and ask "which parts should this figure feature?". The user's picks lock "what goes in the figure".
- **Never skip, never default the focus for the user** — skipping this and jumping to form is the most common failure.
- Enumerate options **live from the conversation** (not hardcoded); multi-select is fine, but flag that **one figure is best with ≤3–4 points** — more gets muddy.

### ② Package (one pick sets "how full + what it looks like")

Collapse the scattered depth / style / coverage axes into one "package" choice — the user picks a **destination** and the sensible defaults come bundled. Keep an "other / describe your own" escape hatch.

> **Surface the whitespace % in the options you show the user** (sci-comm 45–60% / PPT 25–40% / detailed ≤20% / journal ≤20%) — the whitespace ratio is the clearest cue for "how full" the figure will be. It goes **in the option cards, not necessarily in the prompt.**

| Package | For whom | Whitespace | Min elements | Internal detail | Goal |
|---|---|---|---|---|---|
| **Sci-comm** | laypeople / poster | 45–60% (airy) | 1 hero + 2–4 labels + 1 main line + 1 caption | hero drawn figuratively, rest minimal | grasp one concept in 3s |
| **PPT** | talk slide | 25–40% | 3–6 main parts, each w/ a subtitle + 1–2 lines + clear main flow | each part a real object with internal structure, not stacked boxes | follow along while you talk |
| **Detailed** | dense teaching / self-study figure | **≤20%, whitespace = unfinished** | **≥6–10 content regions**, each with sub-structure / sub-steps | **every concept exposes its inner mechanism** (e.g. attention → QKV / softmax bars / residual add), no empty boxes | self-readable without narration |
| **Journal-figure ideas** | a paper-submission figure | ≤20%, restrained | panels a/b/c + legend + caption, full publication conventions | each panel uses real primitives, exposes mechanism, restrained palette (e.g. Okabe-Ito) | reviewer-grade |

**Three hard rules (cure for "detailed never detailed enough"):** ① detailed/journal: **whitespace ≤20%, treat whitespace as a defect signal** — see blank, keep filling; ② detailed/journal: **every region must expose internal structure**, no empty boxes / bare label blocks; ③ each package meets its **minimum-element floor** (countable), blocking laziness.
> But **clarity > completeness (floor) > density**: a detailed figure, however full, must stay **strictly legible** — no muddiness; never densify just to "look like Figure 1".
> Detailed vs journal: detailed = a max-density "teaching figure" (free style, color/metaphor allowed); journal = a publication-spec "paper figure" (restrained, paneled, restrained palette).

**Journal package "three guarantees"** (formal settings fail most easily): ① **guarantee difference** — differentiate via "real primitives + semantic color (one color per modality) + signature = the real focal point", and in the prompt explicitly avoid image-2 clichés (gradient box-flow / glowing nodes / fake 3D chrome); ② **guarantee correctness** — elements come from the content map or the user's words, verify the doubtful, **never invent steps/arrows the method doesn't have just for composition**; ③ **guarantee no-fabrication** — labels verbatim from real terminology, never invented; if unsure, ask / leave it to the user (final text fidelity for real submissions belongs to vector tools; image-2 only delivers layout inspiration).

### ③ Emphasis (**replaces the old "layout" gate**)

Ask one question — **"what should this figure emphasize?"** — and hand composition to the model to work out from this problem. No layout picking, no ASCII.
- Offer **2–4 "emphasize X" directions, heuristic and enumerated live from THIS figure's content (not hardcoded)**, typically: **emphasize comparison** (which parallel example differs) / **emphasize at-a-glance** (grasp the core instantly) / **emphasize the walk-through** (step the process through) / **emphasize the mechanism** (drill into the internals) … plus a "you decide" fallback.
- The chosen emphasis → written as a one-line **note injected into the prompt** ("foreground X, soften Y") — it **sets weight, not placement**: archetype, nesting, panel count, left-right/top-bottom all **go to image-2**.
- **No ASCII, no dictating the layout** — tested: forcing a layout strangles image-2 and comes out worse than free-form (this is exactly why the old "pick a layout direction" gate became "pick an emphasis").
- ⚠️ **The note is composition guidance for the model, not text drawn into the figure** (unless the user wants it as a caption).

### Deck mode (side-path: a set / multi-page / series, not a single figure)

When the task is **a set** rather than one figure, don't run the single-figure flow ×N (that becomes dozens of questions). Switch to "**set defaults once + per page only ask what changes**":
- **Deck-level defaults, set once at the start, not re-asked:** intent, style family + density, house style (one visual line so N pages are a family).
- **Per page only run "what actually changes":** knowledge points ① / package ② / emphasis ③.
- **Simple pages** (cover / background / closer): ① ② near-default → state it for a one-glance yes/no ("one core point, PPT package, right?"), don't interrogate; often only the emphasis is a real choice.
- **Content-heavy pages** (mechanism / method design): ① the knowledge points, and **especially ② the package, are real decisions** → must become clickable options, don't quietly default.
- Target: an N-page deck ≈ 0–1 real question + one emphasis choice per page, not 4N questionnaires.

---

## Step 3 — Assemble the prompt (don't over-specify; leave room for image-2)

Build the prompt in **assembly order** (constraints first → subject after → closing imperative), with the **density anchor from the Step 2 package** (sci-comm airy; detailed/journal whitespace ≤20% with every region exposing internals):

1. **Inject the two hard constraints** (all text in one shot + anti-instruction-leak) — always the full set.
2. **Distill to the fewest primitives/labels** (the core skill is distillation, not transcription): first pass the allow-list (drop model-added synonyms / background / generic words), then cut labels to the minimum (each: "does the figure still hold without it?"), then write **all surviving labels (CJK included) verbatim into the prompt, rendered in one shot**; keep text short, Arial-like sans, prefer short words to cut garble, but **nothing blank, no post-overlay**. `crude ≫ garbled`: if worried about muddiness, generate a few and pick — don't pre-emptively cut below the chosen package.
3. **Fill the `Subject → Setting → Lighting → Text → Style` slots**: turn content into **real technical primitives / structures** (token chips, matrices / vectors, attention maps, score bars, pipeline panels …), not hollow boxes. **🔴 Subject must come from the content map** — metaphor / layout / texture may only *faithfully carry* it, **never invent elements the map doesn't have just to look good**.
4. **Pick a few items from the inspiration menu** (palette / layout / texture / signature) — vary by concept, not the whole set; **palette first from the subject's real materials** (the first anti-same-face gate).
5. **Add non-text completeness, don't blindly densify**: after cutting labels, restore professional completeness with sub-panel borders, stage headers, mini matrices / heatmaps / scatter, real sample thumbnails, legends, numbering, local zooms. Stop at "clear and complete" (**clarity > completeness (floor) > density**); don't pack to "look like Figure 1".
6. **🔴 Hand composition back to the model, don't lock the layout, don't force a gimmick**: give GPT «the content map + guardrails + that Step 2 emphasis note» and that's enough — **placement / proportion / exact composition go back to the model** (`clean professional schematic, compose & scale freely`); don't dictate box positions — tested, forcing a layout came out worse than free-form. For a standard mechanism figure a conventional layout (panels / left→right) is the default; **difference comes only from real primitives + correct content, never from a machine / seesaw / metaphor gimmick. At this step the skill is a guardrail, not a composition director.**

**While assembling, carry these "drawing red lines" into the prompt** (they're both how-to-draw and what Step 4 checks):
- **Aspect ratio defaults to 16:9:** sci-comm / PPT / detailed always write `16:9 landscape` into the prompt; **journal is the exception** (panel/content-driven, often portrait or square); **if the user asks for another ratio, follow the user.**
- **Formality:** carry the mechanism with real technical primitives, don't render abstractions as glowing / glass / toy machines; `lively` cartoon/glow only when the user explicitly wants outreach / a cover (but "formal" ≠ cold/thin — a professional figure can still be colorful, polished, information-dense).
- **Anti-tacky + the de-label test:** draw each concept as **its real object/form**, **recognizable with the text label covered** (score → a real score bar, attention → a real heat mini-map, branching → real converging/diverging arrows); don't draw concepts as "labeled box + arrow" or stacked feature-layer grids, and don't go "icon + one word" throughout.
- **Shared element drawn ONCE:** when two paths hit the same thing (same ID/node/region), draw **one shared element with multiple lines merging into it** (prompt: `draw it ONCE, both arrows merge into it`), not two copies pointing at the same target.
- **Input↔output closure:** every output/conclusion has a matching input; parallel examples each draw `input→output` fully, no half example with an output but no input.
- **Both line ends anchored:** every line/arrow states `from X to Y`, no end dangling from blank space or off-canvas.
- **Visual detail follows focus:** spend the "make-it-polished" budget on the focal elements (large, figurative, the visual hero); minor connectors are just arrows — don't detail the whole figure uniformly.

### Output contract: one `figure-prompt.md` (Codex also + one generated image)

> **Claude**: deliverable = this `.md`. **Codex**: deliverable = this `.md` **+ an image generated on the spot**, in the same reply (after the three gates are answered).

**🔑 The file BODY = the pure prompt only, copyable in one select-all; ALL metadata goes in the FILENAME, not the body.** Users do `Cmd/Ctrl+A → copy → paste into the image tool`, so the file must contain **only the engineered prompt** — no `# title`, no "Brief", no "Notes", no commentary mixed in.

- **Body**: from the prompt's first line to the closing imperative — **nothing else**, clean enough to paste whole.
- **📛 Filename convention (strict, conflict-proof, self-describing)** — underscores only; dots reserved for the extension:
  ```
  <concept>_<intent>_<package>_<YYYYMMDD>_<HHMM>.figure_prompt.md
  ```
  - `concept`: lowercase, underscore-joined (`misspelled_word`, `skill_vs_mcp`) — no hyphens.
  - `intent`: `understand` | `produce`. `package`: `scicomm` | `ppt` | `detailed` | `journal`.
  - `YYYYMMDD_HHMM`: current time — **the key to avoiding name collisions**, always include it.
  - e.g. `misspelled_word_understand_scicomm_20260625_1430.figure_prompt.md`
  - Rule: **all-lowercase, underscores only, zero hyphens**; the only dots are in the extension.
- **Hints/coordinates for the user** (package, emphasis, "generate 3–4 and pick one", keep `fucntion` misspelled, etc.) go **in the chat reply, never in the file.**
- **📍 By default, end the reply with the absolute path** of the prompt file (and on Codex, the generated image) on the **last line**.

```
<engineered English prompt: constraints block (SOURCE-LOCK + TEXT + DO NOT RENDER META)
 -> Subject...Style -> closing imperative>
(the whole file is just this block — no other lines)
```

---

## Step 4 — Safeguard (run the red lines before generating; fail = rewrite)

**Process gates**
- **Transparent?** Did the first working message say `storytelling-figures` is being used?
- **All three choices run?** ① knowledge points ≤5 (focus not defaulted for the user) ② package ③ emphasis — **skipping the knowledge-points step, or quietly producing a figure = failure** (unless the user said "you decide / just draw it").
- **Intent right?** Did understand-it get drowned in options? Did produce-it re-teach what they already knew?
- **Right deliverable for the platform?** Claude = one prompt.md (no hand-coded SVG/PNG); Codex = image also generated and delivered with the prompt (stopping at the prompt = not done).

**Content gates**
- **Every element maps to one real piece of content?** Any pretty-but-invented elements? (content-first)
- **Passed source-lock?** Any pure background/generic word ("the figure", "the method") turned into a concept node? Any example the user didn't ask for?
- **Input↔output closure?** Does every output have a matching input? Parallel examples each draw `input→output` (no half "has `logit(model)` but no input")?
- **(Deep-dive) real mechanism names vs vague verbs?** Is it attention / cosine similarity / top-k / dynamic injection, or stuck on "recognition / matching / processing"? Vague = not deep enough, go back.
- **Jargon explained in place?** Do labels (logit, embedding, softmax …) carry a one-line plain note ("logit = candidate score, higher = more likely")?

**Picture gates**
- **All labels in the figure** (CJK included, nothing blank, not too much text)?
- **Data flow closed + no dangling lines?** Every line/arrow anchored to a named element at both ends? Any element from nowhere, any line with a dangling origin?
- **Formal enough / not over-directed?** Any abstraction rendered as a glowing/glass/toy machine, a **seesaw/balance metaphor gimmick**? **Did the prompt over-prescribe the layout (it should go to the model), or force a gimmick framing just to differentiate?**
- **Pretty / anti-tacky?** Degrade back into "boxes + arrows / stacked feature-layers"? Is each concept a real object (recognizable with the label covered)?
- **Completeness matches the package + density appropriate?** Detailed/journal: whitespace ≤20%, every region exposing internal structure? Conversely, dense for density's sake? **Clarity primary, completeness only a floor.**
- **Color harmony?** One coherent register, no jarring clash (no dark-green block in a cool figure)? Warm accent only on the focal point.
- **Differentiated?** Is this figure different from the last? Is the single message clear? Fewer elements possible?

---

## Style families (inspiration menu, not a mold)

Everything here is a **menu, not a checklist**. Pick one as a starting register, then run with
the concept. **Density is a continuous knob** from `ppt` (sparse) to `journal` (dense) — no extra
named tiers; and **density always serves clarity**.

**Two families.** *Popsci* (`lively`): characters/metaphor/glow allowed — looks like an outreach
poster or lecture cover. *Professional* (`ppt` / `journal`): no cartoon, no mascots, no
glass-machine metaphor — looks like a clean Keynote page or a paper method figure. PPT and
scientific drawing are the **same family**; they differ only in information density.

- **`lively` (popsci) — opt-in exception, not the default.** Eye-catching, strong metaphor,
  narrative; cinematic light / 3D / glow / characters allowed. **Use only when the user explicitly
  wants outreach / a cover / a lecture vibe.** The key to using it well: the metaphor must
  *faithfully carry* the real content, not cover emptiness with a cute picture.
- **`ppt` (professional, sparse but not broken).** Clean, big, few elements, generous whitespace,
  16:9, projection-readable. Whitespace serves reading — **it must not become empty**. One accent
  + a calm base; large short labels; prefer **real technical primitives** (token chips, matrices,
  vector ribbons, attention mini-maps, score bars), not a metaphor machine; each major element has
  at least one layer of visible internal micro-structure (a small matrix, thumbnail, score bar,
  nodes/edges, legend) — not an empty box + one word.
- **`journal` (professional, dense) — paper-schematic.** Not cold gray minimalism, but an
  **information-dense method figure with real artifacts**, completeness modeled on a real paper
  Figure 1: clear regions, stage headers, real sub-primitives, local detail, continuous data flow.
  DNA: horizontal multi-stage pipeline with bold stage titles or numbering; embedded real
  artifacts (sample thumbnails, bar charts, scatter/UMAP, leaderboard bars) not robots; precise
  domain glyphs (trapezoid encoders, Gaussian curves, voxel cubes, bipartite graphs, cluster point
  groups, train/inference toggles); real notation (mu sigma pi, dimensions, formulas); semantic
  pastel color on white (one color per modality), not neon; functional micro-symbols (trainable /
  frozen / operators); dense but gridded with legends; scientific colormaps (risk red->green).

**Avoid the "AI default face" (anti same-face):** give each figure one **signature** (a
composition, a real-primitive combo, a texture) — different every time; only `lively` leans on a
strong metaphor. Avoid AI tells: uniform gradients, cheap neon, floating particles. Texture-first:
the professional family uses paper/vector/interface hierarchy, light shadow, real technical
objects. **Completeness-first but not blind density**: the professional family can't be five empty
boxes; nor force a one-trajectory topic into full-bleed. **Fonts are register-aware**: `journal` =
strict Arial-like sans, common sizes; `ppt` may be friendlier — a clean sans, even a
**handwritten/hand-drawn display font for titles** (a signature move), as long as it stays legible
(body labels stay clear sans); `lively` = free. Rule: legibility first; reserve handwritten faces
for PPT titles/accents, never body labels. **Layout archetype follows the concept** (flow /
comparison / timeline / pyramid / central-illustration …), not a fixed grid — and even this is left
to image-2 to realize; you only name the emphasis.

---

> This file is self-contained — style DNA, prompt template and assembly order, and the
> concept-breakdown method are all inlined above; no extra reference files needed.
