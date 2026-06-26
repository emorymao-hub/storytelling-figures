# Example — produce-it: "Why a misspelled word is still understood"

**Intent: produce-it** (you already know the content and are making a deliverable).

### The plain-language ask
> I'm making a figure for my talk on why an agent still understands a misspelled word — the
> tokens get different vocabulary IDs, but attention in latent space pulls the wrong feature onto
> the right meaning. Give me a clean, good-looking figure for the slide.

### What the skill does here
- **Doesn't re-teach** you the mechanism (you clearly know it) — it does a light consistency check
  and takes the nodes from your own words.
- **Produce-it path**: **diverges** — brainstorms the knowledge points to feature (≤5), then a
  package (here: detailed) and 2–3 layout directions for you to pick, then engineers the prompt.
- Keeps the misspelling intentionally wrong, draws real objects, anchors every line, and makes the
  latent-space transformation the focal point.

### The engineered image prompt
```
A clean, publication-quality technical schematic, 16:9 landscape, explaining why a misspelled word is still understood: its tokens get arbitrary vocabulary IDs and a "wrong" starting feature, and an attention-like operation in latent space pulls that feature to the correct meaning. Off-white background, polished method-figure look, flat vector-like glyphs, soft shadows, restrained cool palette (slate / indigo / teal) with ONE warm amber accent reserved for the misspelled path and the correction. Left-to-right flow ending in a latent-space map. NO machine metaphors, NO cartoon, NO glow.

VISIBLE CONTENT SOURCE-LOCK:
- Only render visible words/entities explicitly listed in the TEXT list below.
- Do NOT introduce related words, synonyms, model names, or extra examples.
- Keep "graammar" misspelled exactly (double a); do NOT auto-correct it.

TEXT (render ALL in one shot, leave NOTHING blank; spell each EXACTLY; English only; horizontal; legible Arial-like sans; no overlap; no gibberish; no watermark; no "Figure 1:" bar):
- "graammar"
- "subword tokens"
- "gr", "aa", "mmar"
- "vocabulary IDs"
- "#412", "#90", "#7"
- "row number = an address, not meaning"
- "wrong feature at input"
- "attention + context"
- "pulled toward the right meaning"
- "latent space"
- "meaning region"
- "right feature = grammar"

DO NOT RENDER META CONTENT:
- Do NOT draw layout words ("left"/"hero"), color/font specs, or any of this prompt as visible text — only the labels above plus the figure.

SUBJECT AND LAYOUT (left → right; the latent-space transformation is the focal hero, drawn largest; every connector is a plain anchored arrow — no dangling line):
- Left: the misspelled word "graammar" (amber) breaks into rounded subword token chips labeled "subword tokens": "gr", "aa", "mmar". Thin arrows run from the chips to a small numbered "vocabulary IDs" table showing plain row numbers "#412", "#90", "#7" (keep them looking arbitrary). A small note: "row number = an address, not meaning".
- Center-left: those IDs map to a starting point dropped into the latent-space map at a spot clearly OUTSIDE the target cluster, tagged "wrong feature at input".
- Center-right (THE hero, largest): an attention-like operation drawn as a few curved attention arcs + a context cue acting on that point, tagged "attention + context"; a single amber trajectory curves FROM the "wrong feature at input" point and is "pulled toward the right meaning" (label the trajectory) INTO the cluster.
- Right: a clean 2D "latent space" panel drawn as a soft contour map / point cloud with a shaded cluster labeled "meaning region"; the trajectory lands inside it at a point labeled "right feature = grammar".

STYLE & COMPLETENESS:
- Render each concept as its ACTUAL object: token chips as real chips, the ID table as a real numbered matrix, attention as real curved arcs, latent space as a real contour/point-cloud map with a converging trajectory — NOT labeled rectangles wired by arrows, NOT stacked feature-layer slabs. Clarity first, medium density, real internal detail in each region, no empty boxes.

Generate a publication-quality figure with the latent-space "wrong feature → pulled by attention → right feature" transformation as the focal hero.
```
