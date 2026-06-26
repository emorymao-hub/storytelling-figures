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
  Once triggered it first re-reads the conversation and harvests what was already discussed
  instead of re-interviewing you; it only fills the gaps (often just the style), then writes
  the prompt.
  Delivery is platform-split: on Claude it hands back one .md; on Codex it hands back the
  prompt plus a natively generated image; it never hand-codes SVG/PNG as a substitute.
  中文触发（同样适用）：把刚才讨论的画成示意图 / 太复杂了画出来理一理 / 帮我画个图解释这个 /
  我要做 PPT 的机制配图 / 画一张 journal 方法示意图 / 给我点布局排版灵感 / 出图 prompt。
  （中文版规则见 SKILL.zh.md。）
---

# storytelling-figures

Turn **a concept you just talked through (or are stuck trying to understand)** into a single
`figure-prompt.md` — a plain-text prompt you paste into GPT image-2 / nano-banana to get a
narrative / conceptual / explanatory / mechanism figure. The deliverable is always a `.md`
(works in both Claude and Codex).

This skill's whole job is to write **one good prompt** and keep the image model from making
the usual weird mistakes. Everything else — the metaphor, palette, composition — is left to
you and the image model. It is a **guardrail, not a template**.

## Identity (read first)

- **You were probably summoned mid-conversation.** The user was chatting and only now wants a
  figure, or has finished understanding something and wants to show it. **First thing: pick up
  the conversation and harvest what's already there — do not start a fresh questionnaire.**
- **Be transparent about invocation.** Your first working message must say you're using
  `storytelling-figures` to turn the discussion into a figure prompt/image. Never draw
  silently — the user must be able to tell whether the skill actually fired.
- **Every choice keeps a "you decide" fallback.** Every set of clickable options includes one
  option like **"Just present it the best way (I'm not sure — you decide)"**, so a user who
  doesn't follow the options or doesn't want to choose can let you pick the best and proceed.
- **Read the tone, adapt how many you ask.** Before each prompt, infer the user's **expertise
  level** and how clear their ask is from **how they talk** (word choice, tone). **Distinctive
  signals (clearly an expert / clearly a layperson / very specific ask) → ask less or not at all;
  vague / unreadable → treat as unclear and ask.** (⚠️ **"ask less" ≠ skip the three hard gates** —
  knowledge points / package / layout are presentation choices; "ask less" means a *faster confirm*
  with your inferred answer as option #1, not deciding for them; only an explicit user "you decide /
  just draw it" actually skips them.) *Explain-to-understand-it-yourself*: **1–3
  questions**, fast, don't tire a confused person. *Explain-to-teach-others / produce*: **ask more,
  heuristically** — step the user toward the form they want.
- **Always options, never an interrogation; always leave an open slot.** Every question is a set
  of **clickable options** (your inferred answer as option #1), **never free-text interrogation**;
  always include a **"your own / you decide"** open option (the tool's Other field) — give a few
  angles *and* room for the user to express their own.
- **🛑 Don't generate until the content is fully in (don't rush to ship).** Before writing the
  prompt, make sure **the user has finished telling you what the figure should express** — for
  produce/explain figures, **ask/confirm "what else do you want in it?"** (the emphasis, the
  contrast, the examples, the sub-blocks) before generating. **If you still have a real blank about
  *what this figure must say*, or the user is still adding, keep gathering — do NOT move to package
  selection / output.** Rushing out a prompt on half the information is the most common, most annoying
  failure; confirm one more round rather than ship on partial input.
- **Context first — don't re-ask what's already known.** Before asking anything, check what the
  conversation already settled: the platform, whether they're making a PPT, the purpose, a focus
  you can infer from the content. **Don't ask the known** (if they're clearly making slides, don't
  ask "what's it for"); **for things you can infer but want to confirm, ask quickly with your
  inferred answer as option #1** (picking #1 = confirms your read, picking another = corrects it),
  rather than asking from scratch. Spend questions on the axes that are genuinely uncertain *and*
  would change this figure.
- **Output by platform:**
  - **Claude = deliver `prompt.md` only, do not render.** Even if you judge a deterministic
    render (SVG/HTML) would be cleaner, **never render it yourself** — whether and how to
    render is the user's call; your deliverable is one `.md`.
  - **Codex = prompt + image, in one shot.** After writing the prompt, immediately use Codex's
    native text-to-image to generate the figure and deliver both in the same reply. Don't stop
    at the prompt. Only fall back to "prompt only" if no usable generator exists, and say so.
  - **Shared red line:** never hand-code SVG/PNG/HTML as a stand-in. Rendering only goes
    through a real text-to-image generator.
- **Content first.** A figure visualizes an **answer you have actually worked out / clarified**,
  not a topic decoration. Every element in the prompt maps to one real piece of content; **do
  not write the prompt until the content is correct.**
- **Source-lock (visible-content allow-list).** Every **visible word, entity, target direction
  or candidate** in the figure must come from one of: explicit user input, a conclusion
  confirmed in the conversation, or a verified/sourced fact. **Pure background or generic words
  that were never discussed as a concept** (a tool name mentioned in passing, "the figure",
  "the method") must not silently become diagram nodes. Anything visible must trace back to one
  item in the content map.
- **Internal consistency (input↔output closure).** Every **output/conclusion** in the figure
  must trace, *within the same figure*, to a matching **input/premise**. If you show several
  parallel examples, draw each one's `input→output` fully — never an output whose input is
  missing. Don't confuse this with source-lock: a word with **no source** (pure background)
  → delete it; a word that's a **real parallel example missing its input** → **add the input**,
  don't delete the output.
- **Every connector/line/arrow must be anchored at both ends (a dangling line is a guaranteed
  failure).** Every line/arrow/dashed curve must connect to a **named element at both a clear
  source and a clear target** — none may emerge from empty space, off-canvas, or a vague blur.
  Classic failure: a few dashed lines sweep into a point but their *other end has no origin*, so
  the reader can't tell *what flowed in* — same crime as an element from nowhere, and it can't be
  explained. State each connector in the prompt as "from X to Y".
- **Formality gate (on by default, one principle, no checklist).** This is a science /
  lab-meeting / method-figure tool: default to serious, restrained, technical. The single
  principle: carry the mechanism with **real technical primitives** (token chips, matrices /
  vectors, a small attention map, logit bars, real sample thumbnails) — **don't render an
  abstract process as a glowing / glass / toy machine.** Use `lively` (cartoon/glow/mascots)
  only when the user explicitly wants outreach / a cover.
- **Anti-"tacky template" (failure mode, hard rule).** Drawing a concept as a **labeled
  rectangle/square wired by arrows**, or as stacked **"feature-layer" slab grids — does not
  pass.** That's the dated AI-diagram look. Draw each concept as **its actual object/form**
  (a real slice, molecule, curve, matrix, interface — draw the actual thing) with modern
  composition and real material/depth. Panels and arrows may *connect* things, but a labeled
  box must **never stand in for the concept itself**.
- **The "remove-the-labels" test (the hard ruler for tacky).** Cover each element's text label —
  **can you still tell what it is?** If not (just a generic icon / empty box doing nothing, with the
  word carrying all the meaning) = decoration, **fails**. Each element must **depict its real
  content/structure** so it reads even with labels removed (good examples: draw "scoring" as real
  score bars; "attention" as a real little heatmap; "a fork" as real arrows converging
  / diverging). Two corollaries: ① **don't use "icon + one word" throughout** — it's monotone and
  hollow; an icon-plus-caption is fine occasionally, not for the whole figure; ② **draw flow/forks
  as clean arrows, never as a machine metaphor** (no railway tracks, gears, machines — too
  unserious, even for a PPT).
- **Draw a shared element ONCE (make sameness structural, not coincidental).** When two paths must
  hit the **same thing** (the same ID / node / region), draw it as **one shared element that both
  paths merge into** — **not two copies that each point at the same target** (image models render
  those as two *different* targets and the "sameness" is lost). Say it in the prompt: "draw it ONCE,
  both arrows merge into it." (Real failure: in a misspelling figure both words' `gr` token was drawn
  twice, each wired to row #412 → the render sent them to different rows.)
- **Visual effort follows focus (elaborate where it matters, stay quiet where it doesn't).** Spend
  the "make it rich and figurative" budget on the **core/focal elements** — they deserve a real
  visualized illustration (depict the idea so it's instantly readable: converge = many points
  funneling to one, diverge = one fanning to many, a chain of choices narrowing step by step) and
  should be the **larger visual heroes**. Keep **connectors and secondary elements restrained** — a
  plain arrow is enough for a connector; don't decorate it; pass quietly over secondary stages. **Do
  NOT render the whole figure at one uniform elaboration** — that wastes effort on the unimportant
  and buries the core. This is the **visual form of focus**: the focal parts aren't just "more
  labels," they're **drawn more figuratively and finely**.
- **Clarity is primary; completeness is only a floor.** Explaining clearly always wins.
  Completeness only guarantees **not half-baked** (not a few empty boxes + arrows + vast
  whitespace with no internal structure) — it is **not** a push for density. **Never densify
  just to "look like Figure 1."** Stop when the mechanism is clear; spend effort on consistency
  and a complete purpose, not on cramming subpanels.
- **Completeness follows the user's coverage choice; never downgrade out of garble-fear (tested
  calibration).** Detailed-vs-simple is the **user's** call (the coverage axis) — keep it simple if
  they want simple, give real detail if they want detail. **Don't impose your own default of
  "complete" or "sparse."** The real failure is **delivering a sparse figure when the user asked for
  comprehensive** (below the level they chose). Image models handle *legible* dense text far better
  than feared (10+ labels + sub-panels render cleanly), so "cut labels" means cut **noise**, and
  **garble-fear is never a reason to drop below the chosen level** — render the detail they asked
  for and **re-roll if garbled.** Under-built is far more likely than garbled, and easier to ship by
  accident — guard against it.
- **Distill, don't transport.** Compress a long conversation into **one core message + the
  fewest necessary primitives/labels.** More labels and tiny text = more garble. One figure
  carries one core point; for each label ask "does the figure still stand without it?" — if
  yes, cut it.
- **Promotion test (prevents this skill from overfitting / going generic).** Before turning a
  lesson from one figure into a rule, ask: is it a **failure mode** (garble / inconsistency /
  wrong wording / broken logic / instruction leakage / tacky look) or a **style preference**
  (which metaphor, how dense, looks-like-some-paper)? **Only failure modes may become rules;
  style preferences only go into the inspiration menu.** This skill blocks weird outcomes;
  everything else is left to the image model and the user.
- **Match the user's language.** Interact (questions, options, explanations) **in whatever language
  the user asked in**, and **label the figure in that language too** — Chinese ask → Chinese,
  English ask → English. **Don't default to one language just because the skill file is written in
  it or the project defaults to it** (answering an English ask in Chinese is a failure). A clear
  user override (e.g. "label the figure in English") wins.
- **Cross-platform (Claude / Codex).** This file speaks in *actions*, not vendor tools: "ask
  with clickable options" (use an `AskUserQuestion`-style tool if present, otherwise list
  options as plain text), "spawn a research sub-agent / check the source" to verify. The
  deliverable (prompt.md) is cross-platform by nature.

> Scope: deliver per platform (Claude: prompt.md only; Codex: prompt + native render), never
> hand-code SVG/PNG/HTML; no data charts (use a plotting skill); no full slide decks (use a
> presentation skill); no guarantee of "swap one exact element".

## Supreme law: differentiate + beautify (overrides everything else here)

**Success = every figure is different, tells its own story, and looks good. Same-face figures
or crude boxes = total failure.** There are only two layers of constraint:

| Layer | Content | Force |
|---|---|---|
| **Hard constraints (always on)** | Only two: (1) **all text rendered in one shot** (CJK included, nothing blank, garble accepted) (2) anti-instruction-leak. This is **correctness**, unrelated to taste. | Mandatory |
| **Inspiration menu (use as needed)** | Palette / layout / texture / metaphor … all a **menu, not a checklist**. Pick a few, **vary them**, give each figure its own signature. | Optional + vary |

**Rules:** (1) don't reuse one recipe, risk over sameness; (2) don't dump academic-drawing
cliches ("must be flat / colorless / no 3D / thin lines") onto the image model — a professional
figure also has to look good, but the good comes from structure, real primitives, color and
hierarchy, not from toy-ish metaphor; (3) **default formal (this is a science tool)**: the
professional family (`ppt`/`journal`) is the default register; `lively` cartoon/glow/mascots
only when the user explicitly wants outreach/a cover — but "formal" is not cold/thin;
professional figures can be colorful, polished, information-dense; (4) **density serves
clarity**: completeness is a floor, not a per-figure quota. Style names are only a **starting
register**, not a mold.

---

## Workflow: harvest → pick knowledge points → pick package → pick layout → prompt engineering

> 🛑 **"The content is clear" does NOT license skipping the choice gates.** Steps 2/3/4 decide
> **presentation** (which knowledge points to feature, which package, which layout) — **these stay
> the user's choice even when the content is 100% clear**. **Never self-authorize "it's obviously
> detailed, I'll just generate"** (tested failure: on Codex it picked the "detailed teaching figure"
> package and generated with no options offered). **The ONLY thing that skips them is the user
> explicitly saying "you decide / just draw it / stop asking"** — the user's words, not your
> judgment that the ask is clear. Handing composition to the model (Step 5) is a separate matter —
> don't conflate: **presentation choices belong to the user, composition belongs to the model;
> neither is yours (the agent) to silently decide.**

### Step 1 — catch + harvest (first thing on being summoned)

You're most likely summoned after an existing conversation. **Don't start a questionnaire.**
Do two things:

1. **Harvest the conversation** into a "content map", and build a **visible-content allow-list**
   (`words/entities allowed to draw` / `conversational background terms forbidden` / `uncertain
   words to ask/verify`). E.g. if the user only asked to explain `fucntion -> function`, the
   allow-list holds only `fucntion`, `function`, and the relevant mechanism terms; a word that
   merely referred to "the model" in passing must not become "model direction" or `logit(model)`.
2. **Lightly detect intent (sets converge vs diverge — don't open with a pile of questions):**
   - **Understand-it** ("too hard, draw it so I get it"; "I learned it, need to explain it to my
     lab") → the later layout step **converges** to 1–2 directions, fewer questions.
   - **Produce-it** ("a figure for slides"; "draw a journal method figure"; "give me layout
     ideas") → the layout step **diverges** into 2–3 directions.
   - If unsure, ask intent in one line.

### Step 2 — brainstorm knowledge points → pick ≤5 (🛑 hard gate ①, the most-skipped step)

The moment you're summoned, **brainstorm immediately**: lay out **all the candidate knowledge
points / sub-topics** in the content, and ask the user with **clickable options (keep it to 5 or
fewer)** "which of these should the figure emphasize?". Their picks lock down **what goes in the
figure**.
- **Never skip this, never default the focus for them.** Skipping it and jumping straight to form
  is the most common failure.
- Options are **enumerated at runtime from the conversation** (not hardcoded); multi-select is
  fine, but flag that **one figure is best with ≤3–4 knowledge points** (more = garble).
- Always keep an "**all of them / you decide**" fallback. Platform fallback: a clickable tool if
  present, else plain numbered text (`1. … 2. … 6. you decide`).

### Step 3 — pick a package (🛑 hard gate ②: four packages, one choice setting "how full + how it looks")

Collapse the old separate depth / style / coverage axes into **one "package" choice** — the user
picks a **destination** and the sensible defaults for each axis come bundled. Keep an "other /
describe your own" escape hatch.

| Package | For whom / what | Whitespace | Minimum elements | Internal detail | One-line goal |
|---|---|---|---|---|---|
| **Sci-comm** | laypeople / poster | 45–60% (airy) | 1 hero object + 2–4 labels + 1 main line + 1 caption | hero drawn figuratively, rest kept simple | get one concept in 3 seconds |
| **PPT** | a talk slide | 25–40% | 3–6 main parts, each with a sub-title + 1–2 lines + a clear main flow | each part a real object with internal structure, not maxed | follow along while you talk |
| **Detailed** | a dense teaching / self-study explainer | **≤20%, whitespace = not finished** | **≥6–10 content-bearing regions**, each with sub-structure/sub-steps | **every concept exposes its inner mechanism** (e.g. attention drawn down to QKV / softmax bars / residual add), no empty boxes | self-readable without narration |
| **Journal-figure ideas** | a paper-submission figure | ≤20%, restrained | panels a/b/c + legend + caption, full publication conventions | each panel uses real primitives, exposes mechanism, restrained palette (e.g. Okabe-Ito) | reviewer-grade, **and offers 2–3 layout ideas to pick from** |

**Three hard rules (to fix "detailed isn't detailed enough"):**
1. **Detailed / journal: whitespace ≤20%, treat whitespace as a defect signal** — see blank, keep filling.
2. **Detailed / journal: every region must expose internal structure; no empty boxes / bare labeled squares.**
3. **Give each package by its "minimum element count" floor** (a countable floor) to block laziness.
> But **clarity > completeness(floor) > density**: even the detailed package must stay **strictly
> legible**, never garbled; don't densify just to "look like Figure 1".
> One-line distinction **Detailed vs Journal**: Detailed = a maximally dense *explainer* (free in
> style, may use color/metaphor); Journal = a *paper figure* bound by publication conventions
> (restrained, paneled, plus extra composition ideas).

**The journal package's "three guarantees"** (most likely to go wrong in formal figures):
① **guarantee difference** — differentiate via "layout from the method's own structure + real
primitives + semantic color (one per modality) + signature = the real focus", and in the prompt
explicitly avoid image-model cliches (gradient box-flow / glowing nodes / fake 3D chrome);
② **guarantee correctness** — elements from the content map or the user's own words, verify
suspicious parts, **never invent steps/arrows the method doesn't have** for composition;
③ **guarantee no-fabrication** — labels verbatim from real terminology, never invented; if unsure
ask / leave to the user (final-text fidelity for real submission relies on a vector tool; the
image model only delivers layout ideas).

### Step 4 — pick a layout direction (described in words, no ASCII)

Offer **1–3 genuinely different layout directions** to pick from (understand-it 1–2, produce-it
2–3). **Before the first figure you MUST actually surface the directions** (clickable options;
unless the user said "you decide / just draw it").
**No ASCII** — a wireframe is low-fidelity, drags everything toward a boxy look, and gets
transcribed into the prompt where it strangles composition (tested: forcing a layout came out
*worse* than free-form). Describe each direction in **words**, three lines:
1. **Layout archetype (one line):** a common layout name + how it's arranged overall. E.g. "clean
   4-panel A/B/C/D, left→right", "two-lane timeline converging on the right", "funnel: many inputs
   collapse to one focus". **For a standard mechanism figure, a conventional layout (panels /
   left→right) is often the best answer — don't force novelty.**
2. **Nesting relationship (big → small):** which piece contains which, and the reading order. E.g.
   `timeline └ two lanes └ 5 nodes each └ each node {name·year·one-line}`.
3. **Note (one line):** "**emphasizes X more, weaker on Y**" — so the user **chooses by what they
   want to emphasize**. **The note is only for choosing the direction; it does NOT go into the figure.**

- Directions must be **fundamentally different** (different archetype / composition / emphasis),
  not tweaks of one figure.
- **Platform fallback:** with `AskUserQuestion`, put the **nesting + note in the option
  description**; without it, **print each direction's card (archetype + nesting + note) as plain
  text**, then list `1. A  2. B  …  you decide`. Always keep a "**Present it the best way (you
  decide)**" fallback; if the user says "whatever / you decide / just draw it", pick the most
  fitting one and build it, with a closing "say the word to switch directions".

### Deck mode (a side path: a slide deck / multi-page poster / a series, not one figure)

When the task is a SET of figures, don't run the single-figure flow N times (that becomes dozens
of questions). Instead: **set defaults once + per figure only ask what varies**:
- **Deck-level defaults, set once at the start, never re-asked:** intent (e.g. making a PPT =
  produce-it), package + house style (the unifying visual thread that keeps N pages one family).
  **If these are already known, don't ask them per page.**
- **Per page, only run "what actually changes here":** knowledge points (Step 2) / package (Step
  3) / layout direction (Step 4).
- **Simple pages** (cover / background / closing): knowledge points and package are near-default →
  **state your inferred values for a quick yes/no** ("just one core point, PPT package — ok?"),
  don't interrogate; often the only real choice is the layout direction.
- **Content-heavy pages** (mechanism / method-design / complex case): knowledge points, and
  **especially the package, are real decisions** → surface them as clickable options, never decide
  silently.
- Carry the deck defaults across pages; only hand the user the incremental question. Goal: an
  N-page deck ≈ 0–1 real question per page + one layout choice, not 4N questionnaires.

> **Both hard gates still run on content-heavy figures.** For any page explaining a **mechanism /
> method / implementation**, **knowledge points (Step 2) and package (Step 3) MUST be surfaced as
> clickable choices** (your inferred answer as option #1). **Never silently decide them** — which
> knowledge points and which package directly set what the page draws and how deep/full it goes,
> the knobs the user most needs to own.

### Step 5 — prompt engineering → figure-prompt.md

Assemble the prompt in this order — **constraints first → subject second → imperative close**
(the **density anchor comes from the package picked in Step 3**: sci-comm airy; detailed/journal
whitespace ≤20% with every region exposing its internals):

1. **Inject the two hard constraints (always, in full):**

   **(1) All text in one shot** (CJK included, garble accepted, nothing blank, no post-overlay).
   Every label the figure needs — including CJK — goes verbatim into the prompt so the model
   renders it in one pass. Rather have minor garble than leave a blank box for the user to fill.
   Use a block like:
   ```
   VISIBLE CONTENT SOURCE-LOCK:
   - Only render visible words/entities explicitly listed in the allowed label list below.
   - Do NOT introduce related words, synonyms, background terms, or examples from the
     conversation unless listed.
   - Do NOT turn a generic discussion word into a diagram node or a semantic direction.

   TEXT (render ALL of these in one shot, as legibly as possible — leave NOTHING blank):
   - The figure MUST contain these exact labels, each on its element, spelled exactly:
     "<label1>", "<label2>", ...  (list EVERY label the figure needs).
   - Render each EXACTLY; do not paraphrase, translate, or invent extra words.
   - Keep text horizontal where possible; legible; no overlapping text.
   - Only ONE ellipsis per column, aligned, never splitting a word.
   - No stray text beyond the labels above (no gibberish dumps, no watermark).
   ```

   **(2) Anti-instruction-leak** (don't paint instruction words into the image):
   ```
   DO NOT RENDER META CONTENT:
   - Do NOT draw layout words ("left panel"/"top"), color/font specs (e.g. "#0072B2"),
     or any of this prompt as visible text — only the labels above plus the figure itself.
   - Do NOT invent a "Figure 1:" caption bar unless asked.
   ```
   > When you DO want panel letters (a/b/c, etc.): state them positively ("draw small panel
   > markers 'a','b','c' as corner markers") and only negate the *instruction phrasing* ("do NOT
   > render the phrase 'panel letters a b c'") — don't negate the letters themselves, or the
   > model gets confused about whether to draw them.

2. **Pick a few items from the inspiration menu** (palette / layout / texture / signature) —
   vary by concept, not the whole set; **prefer color drawn from the subject's real material**
   (first gate against same-face figures).

3. **Fill `Subject -> Setting -> Lighting -> Text -> Style`**, turning content into **renderable
   technical primitives/structure**, not empty boxes; for professional figures think real
   primitives first (token, matrix, vector, attention map, score bar, pipeline panel), not a
   metaphor machine.
   - **Red line: Subject must come from the content map** — every drawn element maps to one
     real piece of content; metaphor/layout/texture may only *faithfully carry* it, **never
     invent elements not in the map**. In professional settings a metaphor is at most a light
     structural device, never the glass-machine star.

4. **Allow-list first, then cut the label budget**: every visible label/entity/candidate must be
   on the allow-list; drop any synonym/background/generic word the model added (or make it a
   non-text glyph). Then minimize labels (each passes "does the figure stand without it?"), then
   write **the surviving labels (CJK included) verbatim** into the prompt for one-shot render;
   **keep text minimal, common font sizes, Arial-like sans, short words** to reduce garble — but
   **nothing blank, no post-overlay**. "All in one shot" constrains the *survivors*, not the
   whole conversation.

5. **Then add visual completeness, without blindly densifying**: after cutting labels, use
   **non-text detail** to restore professional completeness — subpanel borders, stage headers,
   tiny matrices/heatmaps/scatter/bars, real sample thumbnails, legends, numbering, zoom-ins,
   dashed grouping. Stop at "clear and complete"; don't fill density with more text, and don't
   force a single-path topic into a full-bleed Figure 1 (**clarity primary, completeness only a
   floor**). The layout archetype always **follows this concept's own structure** — don't apply a
   fixed template.

   A compact style block to paste for professional figures:
   ```
   PROFESSIONAL SCHEMATIC STYLE:
   - Serious polished Keynote / publication-schematic look: clean off-white background,
     flat vector-like technical glyphs, subtle shadows, semantic colors.
   - Render each concept as its ACTUAL object/form (real slice, molecule, curve, matrix,
     point cloud, interface) with modern composition and real material — NOT a labeled
     rectangle wired by arrows, and NOT stacked "feature-layer" slabs (that reads dated).
   - Prefer REAL primitives: token chips, matrices, vector ribbons, attention mini-maps,
     small network glyphs, score bars, pipeline panels.
   - Core negative (do NOT stack a long no-X list): don't render the abstract mechanism as a
     glowing / glass / toy machine, and don't fall back to a plain gradient-box flow. Add at
     most one or two more negatives only if directly relevant to this figure.
   ```

6. **🔴 Don't over-direct the layout, don't force a gimmick (hand composition back to the model).**
   Give the model **content map + guardrails + one line of archetype steer** — then **leave the
   arrangement / proportions / exact composition to it** ("clean professional schematic, compose &
   scale freely"); don't dictate box positions. Tested: forcing a layout came out **worse than
   free-form**. **For a standard mechanism figure, a conventional layout (panels / left→right) is
   the default** — differentiation comes only from **real primitives + correct content**, **never
   a machine / seesaw / metaphor gimmick** (that's the banned anthropomorphism — see formality /
   anti-tacky). At this step the skill is a **guardrail, not an art director.**

7. **Close with an imperative**: "Generate a publication-quality figure, complete in one image
   (every label rendered), meeting all the guidelines above."

### Cold-start fallback (rare: "just draw me an X" with no prior conversation to harvest)

Only when there's nothing to harvest, run the full classic flow:
1. **Work out the answer yourself first**: get the concept's real mechanism right and complete,
   produce a content map.
2. **Run Step 2→3→4**: with clickable options, ask **knowledge points (≤5) → package → layout
   direction**, all in plain language (a smart-but-outside reader must understand each option;
   jargon stays in your head).
3. Proceed to Step 5.

---

## Deep-dive gate (research/deepest depth, or understand-it going to implementation level)

The content map must **reach the implementation level** — concrete data structures, algorithms,
decision branches, each **sourced**. **Never hand-wave with vague verbs** like
"intent recognition / retrieval matching / similarity / processing" (a real past failure: writing
"default = direct attention, no retrieval" as "similarity scoring"). **No "I happen to know"** —
at this depth actually check the source / spawn a research sub-agent, and put the real mechanism
names (attention, cosine similarity, JSON tool manifest, top-k, dynamic injection …) onto the
figure.

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
boxes; nor force a one-trajectory topic into full-bleed. **Fonts are register-aware (one PPT-vs-journal differentiator)**:
`journal` = strict Arial-like sans, common sizes; `ppt` may be friendlier — a clean sans, and even
a **handwritten/hand-drawn display font for titles** (a signature move that sets it apart from
journal), as long as it stays legible (body labels stay clear sans); `lively` = free. Rule:
legibility first; reserve handwritten faces for PPT titles/accents, never body labels. **Layout
archetype follows the concept** (flow / comparison / timeline / pyramid / central-illustration …),
not a fixed grid.

---

## Output contract: one `figure-prompt.md` (Codex also + one generated image)

> **Claude**: deliverable = this `.md`. **Codex**: deliverable = this `.md` **+ an image
> generated on the spot with the native generator**, in the same reply.

**🔑 The file BODY = the pure prompt only, copyable in one select-all; ALL metadata goes in the
FILENAME, not the body.** Users do `Cmd/Ctrl+A → copy → paste into the image tool`, so the file
must contain **only the engineered image prompt** — no `# title`, no "Brief", no "Notes", no
commentary mixed in (anything in the body gets copied with the prompt and pollutes it).

- **Body**: from the prompt's first line to the closing imperative — **nothing else**, clean enough
  to paste whole.
- **📛 Filename convention (strict, conflict-proof, self-describing)** — the name uses **underscores
  only; dots are reserved for the extension**:
  ```
  <concept>_<intent>_<package>_<YYYYMMDD>_<HHMM>.figure_prompt.md
  ```
  - `concept`: lowercase, underscore-joined (`misspelled_word`, `skill_vs_mcp`, `qtl`) — **no hyphens**.
  - `intent`: `understand` | `produce`. `package` (the Step 3 package): `scicomm` | `ppt` | `detailed` | `journal`.
  - `YYYYMMDD_HHMM`: current time — **the key to avoiding name collisions**, always include it.
  - e.g. `misspelled_word_understand_scicomm_20260625_1430.figure_prompt.md`
  - Rule: **all-lowercase, underscores only, zero hyphens**; the only dots are in the extension. One
    filename tells you what/for-whom/which-package/when, and repeated renders never overwrite.
- **Hints/coordinates for the user** (focus/coverage/depth, "generate 3–4 and pick one", keep
  `fucntion` misspelled, etc.) go **in the chat reply, never in the file.**
- **📍 By default, end the reply with the absolute path** of the prompt file (and on Codex, the
  generated image) on the **last line**, so the user can locate/copy it directly — unless they ask
  otherwise.

```
<engineered English prompt: constraints block (SOURCE-LOCK + TEXT + DO NOT RENDER META)
 -> Subject...Style -> closing imperative>
(the whole file is just this block — no other lines)
```

---

## Pre-delivery self-check

1. **Transparent?** Did the first working message say `storytelling-figures` is being used?
2. **Intent right?** Did understand-it get drowned in layout options? Did produce-it re-teach
   what they already knew?
3. **Every element maps to one real piece of content?** Any pretty-but-invented elements?
   (content-first red line)
4. **Passed source-lock?** Any pure background/generic word ("the figure", "the method") turned
   into a concept node? Any example/target the user didn't ask for?
5. **Input<->output closure?** Does every output have a matching input? Did parallel examples each
   draw `input->output` fully? — no half example with an output but no input.
6. **(Deep-dive only) real mechanism names** (attention / cosine similarity / top-k / dynamic
   injection) or stuck on vague verbs (recognition / matching / processing)? Vague = not deep
   enough, go back.
7. Scientifically accurate, clearly readable, **all labels in the figure (nothing blank, not too
   much text)**, sound composition.
8. **Data flow closed?** Does every element have an incoming and outgoing arrow? Any element
   appearing from nowhere? **Is every line/arrow/dashed curve anchored to a named element at
   both ends** — any **dangling line** (a dashed curve into a point whose other end has no
   origin, so it can't be explained)?
9. **Formal enough / not over-directed?** Did an abstract mechanism get rendered as a
   glowing/glass/toy machine, a **seesaw/balance metaphor gimmick**, or over-cartooned/dramatic?
   **Did the prompt over-prescribe the layout (composition should be handed to the model), or force
   a gimmick framing just to differentiate (a standard mechanism figure wants a conventional
   layout)?** Default should read like a real lab-meeting / PPT / method figure.
10. **Anti-tacky?** Did it degrade back into "boxes/squares + arrows / stacked feature-layers"? Is
    each concept drawn as a **real object** (not a labeled box)? Box-flow / tacky → **fails,
    rewrite.**
11. **All three choice gates run? (hard gate)** Before the first figure, were these **actually
    surfaced as clickable options**: ① **knowledge points ≤5** (user picks what to emphasize, not
    defaulted) ② **package** (sci-comm / PPT / detailed / journal, sets how full) ③ **1–3 layout
    directions** (each = archetype + nesting relationship + one note, **no ASCII**)? Skipping the
    knowledge-points step, or quietly producing a figure = failure (unless the user said "you
    decide / just draw it").
12. **Color harmony?** One coherent register? Any jarring clash (a dark-green block in a cool
    figure)? Warm accent only on the focal point.
13. **Jargon explained in place?** Do in-figure technical labels (logit, embedding, softmax …)
    carry a one-line plain note ("logit = candidate score, higher = more likely")? Bare jargon =
    not explained.
14. **Pretty / not worse than SVG?** Did it degrade into a crude box diagram? (degrade → rewrite)
15. **Completeness matches the chosen package, density appropriate?** Any crude/broken/empty/thin: a
    few boxes + arrows + vast whitespace, no sub-primitives/legend/local detail? **Detailed/journal:
    is whitespace ≤20% and does every region expose internal structure** (don't ship a figure
    sparser than the package picked)? Conversely, dense for density's sake, hurting clarity?
    **Clarity primary, completeness only a floor.**
16. **Right deliverable for the platform?** Claude = one prompt.md (no hand-coded SVG/PNG); Codex =
    image also generated and delivered with the prompt (stopping at the prompt = not done).

Plus: **single message? fewer elements possible?**
