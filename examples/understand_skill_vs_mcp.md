# Example — understand-it: "Skill vs MCP"

**Intent: understand-it** (you want to grasp / explain a concept).

### The plain-language ask
> Help me draw a diagram to explain the difference between a skill and an MCP.

### What the skill does here
- **Content-first**: works out the real distinction before drawing — a *skill* is a markdown
  instruction file loaded into the agent's context (no server, no new tools); an *MCP* is a
  running server that exposes new tools to the agent over a protocol.
- **Understand-it path**: brainstorms the knowledge points to feature, picks a package, offers a
  layout direction, then **converges** to one clear comparison figure — it doesn't pile on options.
- Delivers the engineered prompt below (Claude: prompt only; Codex: prompt + image).

### The engineered image prompt
```
A clean, publication-quality explanatory schematic, 16:9 landscape, comparing two concepts side by side across a single thin vertical divider: the LEFT half explains a "Skill", the RIGHT half explains an "MCP". One coherent off-white background and one design language so the two halves read as one figure. Flat vector-like technical glyphs, soft shadows, restrained cool palette with ONE warm amber accent. NO machine metaphors, NO cartoon, NO glow.

VISIBLE CONTENT SOURCE-LOCK:
- Only render visible words/entities explicitly listed in the TEXT list below.
- Do NOT introduce related words, synonyms, model names, or extra examples.

TEXT (render ALL in one shot, leave NOTHING blank; spell each EXACTLY; English only; horizontal; legible; no overlap; no gibberish; no watermark; no "Figure 1:" bar):
- Two half-headers: "Skill", "MCP"
- Left ("Skill") labels: "SKILL.md", "name + description loaded first", "full instructions loaded on demand", "into the agent's context", "know-how: HOW to do the task", "no server"
- Right ("MCP") labels: "MCP server", "tools exposed over a protocol", "the agent calls a tool", "runs code, returns a result", "API", "Database", "Files", "power to ACT on the world"
- One bottom caption spanning the width: "Skill = expertise loaded INTO the agent   ·   MCP = a connection wired OUT to systems"

DO NOT RENDER META CONTENT:
- Do NOT draw layout words ("left half"/"divider"), color/font specs, or any of this prompt as visible text — only the labels above plus the figure.

LEFT HALF — "Skill" (expertise absorbed INTO the agent):
- Draw a real open SKILL.md document with markdown-like lines: a small highlighted top chip "name + description loaded first", and below it body lines "full instructions loaded on demand" fading in (progressive disclosure). An arrow carries the page "into the agent's context" (a small agent glyph). Tags "know-how: HOW to do the task" and "no server". Every line connects named source to named target — no dangling line.

RIGHT HALF — "MCP" (power to act, wired OUT):
- The same agent connected by a cable doing "the agent calls a tool" to an "MCP server" box labeled "tools exposed over a protocol" and "runs code, returns a result"; from the server, branches fan out to real external systems "API", "Database", "Files". Tag "power to ACT on the world".

STYLE & COMPLETENESS:
- Serious polished method-figure look; render each concept as its ACTUAL object (a real document, a real server with cabled tools, real external-system icons) — NOT labeled rectangles wired by arrows. Medium density, clarity first, no empty boxes, no large blank areas.

Generate a publication-quality side-by-side comparison meeting all the guidelines above.
```
