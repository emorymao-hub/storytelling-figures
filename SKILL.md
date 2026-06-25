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
  vague / unreadable → treat as unclear and ask.** *Explain-to-understand-it-yourself*: **1–3
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
  *what this figure must say*, or the user is still adding, keep gathering — do NOT move to Step
  2.5 / output.** Rushing out a prompt on half the information is the most common, most annoying
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
  content/structure** so it reads even with labels removed (good examples: draw "direction choice"
  as a few real ASCII sketch rows; "scoring" as real score bars; "a fork" as real arrows converging
  / diverging). Two corollaries: ① **don't use "icon + one word" throughout** — it's monotone and
  hollow; an icon-plus-caption is fine occasionally, not for the whole figure; ② **draw flow/forks
  as clean arrows, never as a machine metaphor** (no railway tracks, gears, machines — too
  unserious, even for a PPT).
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

## Workflow: detect intent → two paths → prompt engineering

### Step 1 — catch + harvest + detect intent (first thing on being summoned)

You're most likely summoned after an existing conversation. **Don't start a questionnaire.**
Do two things:

1. **Harvest the conversation** into a "content map": what the user **asked about repeatedly**
   = their focus; how **deep** it went = their depth. Most of this is already in the chat —
   just take it.
   - At the same time build a **visible-content allow-list**: `words/entities allowed to draw`,
     `conversational background terms forbidden`, `uncertain words to ask/verify`. E.g. if the
     user only asked to explain `fucntion -> function`, the allow-list holds only `fucntion`,
     `function`, and the relevant mechanism terms; a word that merely referred to "the model"
     in passing must not become "model direction" or `logit(model)`.
2. **Detect intent** (decides the whole path):

| | **Understand-it** | **Produce-it** |
|---|---|---|
| Signals | "too hard, draw it so I get it"; "I learned it, need to explain it to my advisor / lab" | "I need a figure for slides"; "I designed a method, draw a journal figure"; "give me layout ideas" |
| What they want | **Clarity** (get it themselves / explain to others) | **Beauty + design ideas** (goes straight into a deliverable) |
| Where content comes from | grew out of the chat, **maybe still stuck/incomplete** | **their own, complete, they know it better than you** |

> If unsure, **ask intent in one line** ("Do you want to first understand it, or do you already
> get it and want a figure with layout ideas?") — not a triple questionnaire.

### Step 2A — understand-it: fill content → converge to one clear figure

- **Content**: mostly harvested; **fill/verify the part they got stuck on** — this is where
  content-first research is most valuable (don't let the figure paper over what they didn't
  grasp). For implementation depth, use the **Deep-dive gate** below.
- **One message**: the **core point that resolves the confusion** (a few sentences ok; each is
  a verb-bearing conclusion, not a topic).
- **🔴 Axis gate: focus / depth / style MUST be surfaced as options (same tier as the direction
  choice, not a suggestion).** Before output, **always present these axes** (your inferred answer as
  #1); **never decide them silently.**
  - **The direction choice (the Step 2.5 ASCII pick) does NOT replace surfacing the axes** — offering
    only the direction while silently fixing focus/depth/style = **failure** (real case: on GPT it
    only popped the ASCII direction and skipped all the axes). **Surface both.**
  - **Platform fallback (critical):** use a clickable-option tool if available; **if not, list the
    options as plain numbered text** (`1. … 2. … 3. … 4. your own`) — **do not skip asking just
    because you can't render fancy cards.** Every platform (Claude / Codex / other) must surface them.
  - Understand-it may **bundle into one or two quick screens** (focus+depth on one, style on another);
    only collapse to "direction only" when the user clearly signals "I'm lost / keep it simple / you
    decide."
- **Style**: default `ppt` (clean, clear, whitespace, professional — can be polished and
  colorful, but **no cartoon, no glow, no mascots**). For deep/complex content dial toward
  `journal` (a denser method figure); **density follows clarity — if mid-density explains it,
  don't make it full-bleed** (ppt<->journal is a continuous knob, don't agonize over a tier).
  Use `lively` only when the user explicitly wants an outreach/cover vibe.
- **Stance = converge**: aim for **one figure you get at a glance**. Still run Step 2.5 for that
  *one* direction choice, but don't pile on extra questionnaires for someone already confused.

### Step 2B — produce-it: take their content → diverge into layout ideas

- **Content**: theirs — **don't re-teach, don't re-research** (they know it better). But run a
  **light consistency check** — if you spot a clear contradiction, a suspicious point, or a key
  deep mechanism, **verify / flag it**; don't draw something wrong.
- **Style**: default `ppt` (sparse, projection-ready) or `journal` (dense, submission/method
  figure).
- **Stance = diverge, but read the ask:**
  - They explicitly say "give me **ideas / options / a few layouts**" → offer **2–3 layout
    archetypes / signature directions** (one line each + an optional ASCII sketch) to pick from,
    then build the chosen one out.
  - They say "**just draw it / turn the above into a figure**" → **don't stop to make them
    choose**: pick the most fitting default layout and **write the prompt directly**, with a
    closing line "say the word if you want a different layout". **One fewer round-trip > one more
    choice.**
- focus / depth are usually clear to them; follow what they give.

**🧭 Produce-it thinking chain (a process system — not making aesthetic decisions for the user)**
We can't give scene-specific drawing advice for arbitrary figures, but we can give a process of
**asking the right questions**: each step is a **clickable choice**, each maps to exactly one
downstream decision, each lets the user defer with "you decide". **Options are generated at
runtime from the user's own content** (except Q1, which is fixed) — never hardcoded. Understand-it
runs a trimmed version (≤3 questions; focus is usually already harvested, so jump to Q3 / Step
2.5); produce-it runs the full chain:

```
Q1 What's this figure for?      ──▶ sets style family + density
   Fixed options: "Paper figure" (→ journal, dense) / "PPT / talk" (→ ppt, sparse, 16:9) /
   "Sci-comm poster" (→ lively); plus free-text (the clickable tool's Other field).
Q2 What to emphasize?           ──▶ sets focus + signature
   Options ENUMERATED at runtime from the user's pipeline nodes: emphasize <core/novel module> /
   <input·data> / <output·result> / spread evenly + "you decide".
Q3 How much content to cover?   ──▶ sets coverage (→ density follows; never ask "how dense")
   Just the core / core + key supporting / comprehensive (more branches + detail) + "you decide".
Q4 How deep to emphasize?       ──▶ sets depth
   Overview (big-picture, no formulas) / Key mechanism (how the core works, no low-level) /
   Full implementation (formulas · dimensions · boundary conditions) + "you decide".
 → Step 2.5 direction choice (2–3 visual directions, each with an ASCII sketch — pick one)
 → Step 3 prompt engineering
```
> **Information density = coverage × depth — don't ask "how dense" (the user can't judge that),
> ask "how much content to cover" (which they can).** More coverage + more depth → more density,
> as a result. For explain-to-teach, run these as heuristic clickable steps with a "your own" slot;
> for explain-to-understand, trim to 1–3 questions by tone.
> Each question swaps exactly one decision (Q1→register, Q2→focus, Q3→depth), so "produce-it may
> ask more" is a **chain that converges**, not loose chit-chat. The user can bail with "you decide
> / just draw it" at any point and you pick the best. These are **presentation questions, not
> content questions** — don't use them to re-teach the method they know better than you.

**Produce-it · journal "three guarantees" (the three things most likely to go wrong in formal
figures — handle as one step)**
- **Guarantee difference**: a formal figure **can't differentiate via robots/glow/metaphor**.
  Only four legit levers — (1) pick the layout from **this method's own structure** (two-lane /
  radial / nested …, not a default "left-to-right box flow"); (2) use **this method's real
  primitives** (real samples, real molecules, real data structures — inherently unique);
  (3) semantic color **bound to method parts** (one color per modality); (4) signature = the
  method's **real visual focus** (the fusion point / fork / bottleneck). And in the prompt
  **explicitly avoid the AI sci-infographic cliches**: generic gradient box-flow, glowing nodes,
  fake 3D chrome, neon outlines.
- **Guarantee correctness**: elements and structure **from the content map or the user's own
  words**; light consistency check, verify suspicious/deep parts; **never invent steps/arrows
  the method doesn't have** just for composition.
- **Guarantee no-fabrication**: labels **verbatim from real terminology, never invent/approximate
  spelling**; if unsure, **ask or leave it to the user** — don't let the image model bluff;
  minimize labels (fewer = less garble, fewer errors). **Honest positioning**: final-text
  fidelity for real submission does **not** rely on the image model — it delivers **layout /
  composition ideas**; treat rendered text as placeholder and finalize real labels in a vector
  tool.

### Step 2.5 — direction brainstorm (the big choice, on by default)

Before drawing, give the user **one high-leverage choice**: brainstorm **2–3 genuinely different
visual directions** to pick from, then build out the chosen one. This step fixes "the result
didn't match what I wanted / it looks tacky".
> **This is a hard gate, not a suggestion: before the first figure you MUST actually surface the
> 2–3 directions as clickable options and wait for a pick before writing the prompt.** Quietly
> producing a figure with no choice ever offered = failure (unless the user said "you decide /
> just draw it"). In practice this step is the most commonly skipped — don't skip it.
- **This is a real brainstorm — not three skeletal ASCII lines.** Each direction must be something
  the user can genuinely evaluate and choose between, with at least: ① **one sentence on the core
  presentation approach** (which real object/metaphor carries the concept, the overall composition,
  the signature, what it emphasizes); ② **a line or two on "why it fits / how it differs from the
  others"**; ③ **a legible** ASCII sketch (a supplement, not decoration). **Directions must be
  fundamentally different** (different motif / composition / emphasis) — genuinely distinct design
  concepts, not tweaks of one figure. **Don't let a crude ASCII stand in for a real description of
  the direction.**
  > **🎨 ASCII sketch craft (an unreadable sketch is worse than none — it MUST be legible):**
  > ① **Label every shape with a short word; never leave a bare `●`/`□`** — a bare shape means
  >    nothing.
  > ② **Align columns** (monospace grid); show flow with `→ ↓ ↘`; draw forks/merges explicitly.
  > ③ Consistent shape semantics: `[ ]`=module/panel, `( )`=station/node, `●`=data point,
  >    `▢`=vector, `═/─`=track/connector.
  > ④ **Show only this direction's distinguishing idea (its signature)**, not every detail — a
  >    sketch is not the final figure; keep it ≤6 lines.
  > ⑤ **One plain-language note underneath** stating the selling point / how it differs from the
  >    others.
  > Bad (illegible): `● ──▶ ●   ● ──▶ ●` (bare dots, unknown meaning, no note).
  > Good:
  > ```
  > top   noisy-in  ─▶[②attn+MLP]─▶ ●H_L(noisy)   ┐
  > bottom clean-in  ─▶[②attn+MLP]─▶ ●H_L(clean)  ┘→ same semantic region
  > selling point: each dot is its own track's terminus; origin = its colored track; no dangling line
  > ```
- Offer it with **clickable options** (use an `AskUserQuestion`-style tool if present, else plain
  text). **If the tool has a preview field, put the ASCII sketch in the preview** so the directions
  compare side by side.
- **Understand-it gets this too, but just this one choice** — don't turn it back into a
  questionnaire; produce-it is already divergent, so this fits naturally.
- No direction may be the "labeled box + arrows / stacked feature-layers" style (see
  "anti-tacky template") — offer genuinely modern directions that draw the concept as a **real
  object**.
- **Always include a fallback option "Present it the best way (I'm not sure — you decide)"**:
  lets a user who can't follow or doesn't want to choose hand it to you.
- If the user says "whatever / you decide / just draw it" or picks the fallback, pick the most
  fitting direction and build it, with a closing "say the word to switch directions".

### Step 2.6 — Deck mode (a set of figures: a slide deck / multi-page poster / a series, not one figure)

When the task is a SET of figures, don't run the single-figure flow N times (that becomes dozens
of questions). Instead: **set defaults once + per figure only ask what varies**:
- **Deck-level defaults, set once at the start, never re-asked:** intent (e.g. making a PPT =
  produce-it), style family + density, and a house style (the unifying visual thread that keeps N
  pages one family). **If these are already known, don't ask them per page.**
- **Per page, only run "what actually changes here":** focus / depth / direction (Step 2.5).
- **Simple pages** (cover / background / closing): focus and depth are near-default → **state your
  inferred values for a quick yes/no** ("focus = balanced, depth = overview — ok?"), don't
  interrogate; often the only real choice is the layout direction.
- **Content-heavy pages** (mechanism / method-design / complex case): focus, and **especially
  depth, are real decisions** → see "depth is a required choice" below; surface them as clickable
  options.
- Carry the deck defaults across pages; only hand the user the incremental question. Goal: an
  N-page deck ≈ 0–1 real question per page + one direction choice, not 4N questionnaires.

> **Depth is a required choice on content-heavy figures (hard).** For any figure explaining a
> **mechanism / method / implementation**, **depth MUST be surfaced as a clickable choice** (e.g.
> `Overview (big picture, no formulas) / Key mechanism (how the core works) / Full implementation
> (formulas, dimensions, boundaries)`, with your inferred level as option #1). **Never silently
> decide depth** — it directly sets how deep the figure goes and is the knob the user most needs
> to own. Same for focus: if inferable, confirm with "inferred = option #1", but **don't silently
> skip it**.

### Step 3 — prompt engineering → figure-prompt.md (both paths converge here)

Assemble the prompt in this order — **constraints first → subject second → imperative close**:

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

6. **Close with an imperative**: "Generate a publication-quality figure, complete in one image
   (every label rendered), meeting all the guidelines above."

### Cold-start fallback (rare: "just draw me an X" with no prior conversation to harvest)

Only when there's nothing to harvest, run the full classic flow:
1. **Work out the answer yourself first**: get the concept's real mechanism right and complete,
   produce a content map.
2. **Lock the one message** → ask `focus` (multi-select, detail distribution) / `depth`
   (single) / `style` (single) with clickable options, all in plain language (a smart-but-outside
   reader must understand each option; jargon stays in your head).
3. Proceed to Step 3.

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
- **Metadata into the filename**: encode concept + intent + style, e.g.
  `skill-vs-mcp.understand-ppt.figure-prompt.md` / `qtl.understand-journal.figure-prompt.md`
  (`<concept>.<intent>-<style>.figure-prompt.md`).
- **Hints/coordinates for the user** (focus/coverage/depth, "generate 3–4 and pick one", keep
  `fucntion` misspelled, etc.) go **in the chat reply, never in the file.**

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
9. **Formal enough?** Did an abstract mechanism get rendered as a glowing/glass/toy machine, or
   over-cartooned/dramatic? Default should read like a real lab-meeting / PPT / method figure.
10. **Anti-tacky?** Did it degrade back into "boxes/squares + arrows / stacked feature-layers"? Is
    each concept drawn as a **real object** (not a labeled box)? Box-flow / tacky → **fails,
    rewrite.**
11. **Offered the direction choice? (hard gate)** Before the first figure, were Step 2.5's 2–3
    directions **actually surfaced as clickable options** for the user to pick, **each with an
    ASCII sketch** so they compare without rendering? Quietly producing a figure with no choice
    ever offered = failure (unless the user said "you decide / just draw it").
12. **Color harmony?** One coherent register? Any jarring clash (a dark-green block in a cool
    figure)? Warm accent only on the focal point.
13. **Jargon explained in place?** Do in-figure technical labels (logit, embedding, softmax …)
    carry a one-line plain note ("logit = candidate score, higher = more likely")? Bare jargon =
    not explained.
14. **Pretty / not worse than SVG?** Did it degrade into a crude box diagram? (degrade → rewrite)
15. **Complete enough (floor) and density appropriate?** Any crude/broken/empty/thin: a few boxes
    + arrows + vast whitespace, no sub-primitives/legend/local detail? Conversely, dense for
    density's sake, hurting clarity? **Clarity primary, completeness only a floor.**
16. **Right deliverable for the platform?** Claude = one prompt.md (no hand-coded SVG/PNG); Codex =
    image also generated and delivered with the prompt (stopping at the prompt = not done).

Plus: **single message? fewer elements possible?**
