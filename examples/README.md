# Examples

Two worked examples, one per **intent** the skill detects from how you ask:

| File | Intent | When | What the skill does |
|---|---|---|---|
| [`understand_skill_vs_mcp.md`](understand_skill_vs_mcp.md) | **understand-it** | you want to *grasp / explain* a concept | works the real content out first, asks a little, **converges** to one clear figure |
| [`produce_misspelled_word.md`](produce_misspelled_word.md) | **produce-it** | you already know it and are *making a deliverable* | doesn't re-teach you, **diverges** — offers layout directions, then engineers a polished prompt |

Each file shows the **plain-language ask** that triggered the skill and the **engineered image prompt** it produced. The deliverable on Claude is always just that prompt (you paste it into GPT image-2 / nano-banana); on Codex you also get the rendered image.
