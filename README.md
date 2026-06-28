# storytelling-figures

**v6.0** · **[English](#english) · [中文](#中文)**

A **Claude Code / Codex skill** that turns a conversation into a clean, paste-ready
**image-generation prompt** for scientific, conceptual and explanatory figures — and quietly keeps
the model from making the usual weird mistakes.

---

## English

You're chatting with an AI about some mechanism — how an LLM tokenizes a typo, how an agent picks a skill, your own method's pipeline. At some point you think *"this is hard to explain, let me just draw it."* This skill catches that moment: it reads the conversation you already had, engineers a single high-quality prompt for **GPT image-2 / nano-banana**, and hands it back ready to paste.

It is **not** a figure renderer and **not** a chart library. Its whole job is to write *one good prompt* and avoid the failure modes that make AI-generated figures look wrong or tacky.

### Highlights
A model can already write an image prompt. The three things it **can't reliably do on its own — so the skill enforces them:**

| Guarantee | A model on its own tends to… | With the skill — enforced |
| :-- | :-- | :-- |
| **🔒 Honest content** | invent steps, slip a background word ("the model") in as a concept, draw lines from nowhere | a source-lock allow-list, a correct-first mechanism check, and input↔output closure |
| **🎨 No templated look** | fall back to the same boxes-and-arrows and stacked "feature-layer" slabs | two hard rules plus an inspiration menu; every concept drawn as its real object (the remove-the-labels test) |
| **🔤 Readable labels** | garble or drop CJK / dense text, and leak instruction words into the image | every label baked in once, register-aware fonts, nothing dropped or leaked |

### What it actually does for you
The model can already write an image prompt. What it can't reliably do on its own is avoid these:
- **Gibberish / dropped labels** — CJK and dense text garble. The skill bakes every label in once, with explicit "render exactly" instructions.
- **Made-up / leaked content** — a background word from the chat ("the model") sneaks in as a diagram concept. A *source-lock* allow-list blocks that.
- **Logical inconsistency** — an output appears with no matching input; a dangling line arrives from nowhere. An *input↔output closure* check forbids it.
- **Tacky, dated look** — stacked "feature-layer" slabs, everything a labeled box wired by arrows. Hard rule + a *remove-the-labels test*: draw each concept as the **real object**, not a box.
- **Wrong visual effort** — it spends the rich, figurative detail on the *core* and keeps connectors and secondary parts quiet (visual effort follows focus).
- **Font / layout traps** — register-aware fonts, one-ellipsis-per-column, no instruction text leaking into the image.

Everything else — the metaphor, palette, composition, how dense — is left to you (via clickable, heuristic option prompts) and the image model. The skill stays a **guardrail, not a template**.

### Install
Pure markdown — no dependencies, no API keys.
```bash
# Claude Code
git clone https://github.com/emorymao-hub/storytelling-figures.git ~/.claude/skills/storytelling-figures
# Codex
git clone https://github.com/emorymao-hub/storytelling-figures.git ~/.codex/skills/storytelling-figures
```
Start a new session — it auto-discovers and triggers on intent. **It answers in the language you ask in** — English question → English, Chinese question → Chinese.

### How to use it
Don't invoke it by hand. Talk through a concept, then say *"draw what we just discussed"* / *"this is too confusing, draw it so I get it"* / *"I need a journal method figure, give me layout ideas."* It harvests the chat, brainstorms the candidate knowledge points and asks which to feature (≤5), then a package (sci-comm / PPT / detailed / journal) and what to emphasize (comparison / at-a-glance / walk-through / mechanism), then engineers the prompt — leaving the composition to the image model.

> **Example ask:** *"Help me draw a diagram to explain the difference between a skill and an MCP."*
> → it harvests the distinction, runs the three gates (knowledge points · package · emphasis), and hands back one paste-ready prompt.

**Output by platform:** on **Claude** you get one `figure-prompt.md` to paste into the image model. On **Codex** with a native text-to-image tool, you get the prompt **and** the image together. Neither hand-codes SVG/PNG.

### What's inside
```
SKILL.md       # the whole skill, self-contained: workflow + guardrails (replies in your question's language)
README.md      # this file
LICENSE        # MIT
```

### Scope
Draws **mechanism / concept / process schematics**. For data charts use a plotting skill; for full slide decks use a presentation skill.

---

## 中文

你正跟 AI 聊某个机制——LLM 怎么把打错的词切 token、agent 怎么挑 skill、你自己方法的 pipeline。聊到某刻你想 *"这太难讲了，画出来吧。"* 这个 skill 就接住这一刻：回看你已经聊过的对话，工程化出一个高质量的 **GPT image-2 / nano-banana** 出图 prompt，直接给你粘。

它**不是出图器、也不是画图表的库**。它唯一的活，是写**一个好 prompt**，并挡掉那些让 AI 配图又错又俗的翻车。

### 亮点
模型本来就会写出图 prompt；它**自己稳不住、得靠 skill 兜住的就这三件事：**

| 保证 | 模型自己出图，常翻车成… | 装上 skill，强制做到 |
| :-- | :-- | :-- |
| **🔒 内容不跑偏** | 臆造步骤、把背景词（如泛指的"模型"）误当概念、画出没来源的悬空线 | 可见内容白名单（source-lock）、先对后画核实机制、输入↔输出守恒 |
| **🎨 不千图一面** | 退回千篇一律的"方框＋箭头"、堆叠"特征层"网格 | 仅两条硬规则＋一份灵感菜单；每个概念画成它真实的对象（盖住标签也认得出） |
| **🔤 标签画得清** | 中文／密集文字糊成乱码或漏字、把 prompt 指令词漏进画面 | 每个标签一次画全、按场景选字体、不糊不漏不外泄 |

### 它到底替你干了啥
模型本来就会写出图 prompt。它自己搞不定的，是稳定避开这些坑：
- **乱码 / 掉标签**——中文和密集文字易糊。skill 把每个标签逐字写进、并下"照写清楚"的硬指令。
- **臆造 / 串词**——对话里的背景词（如泛指的"模型"）混进图当概念节点。**可见内容白名单（source-lock）**挡住。
- **逻辑不自洽**——有输出却没配对输入；一条线从空白处飘来（悬空线）。**输入↔输出守恒**禁止。
- **俗气、过时**——堆叠"特征层"、通篇贴标签方框+箭头。硬规则 +「**去标签测试**」：每个概念画成**真实对象**，不是盒子。
- **力气花错地方**——把精致、形象化的预算花在**核心**上，连接线和配角沉住（视觉详略跟着 focus 走）。
- **字体/版式坑**——字体按 register 分、省略号每列一处、指令词不外泄进图。

其余——隐喻、配色、构图、画多密——都通过**可点选项、启发式**交给你和图模型。skill 始终是**护栏，不是模板**。

### 安装
纯 markdown，无依赖、不要 key。
```bash
# Claude Code
git clone https://github.com/emorymao-hub/storytelling-figures.git ~/.claude/skills/storytelling-figures
# Codex
git clone https://github.com/emorymao-hub/storytelling-figures.git ~/.codex/skills/storytelling-figures
```
新开会话即自动发现、按意图触发。**问什么语言就答什么语言**——问英文给英文、问中文回中文，跟装的是哪个文件无关。

### 怎么用
不用手动调。聊透一个概念后随口说 *"把刚才这个画成图"* / *"太复杂了画出来理一理"* / *"我要画张 journal 方法图，给点布局灵感"*。它会收割对话、脑暴出可选知识点让你勾（≤5），再选套餐（科普 / PPT / 详细 / journal）和侧重点（侧重比较 / 秒懂 / 讲解 / 机理），最后工程化出 prompt——构图交给出图模型发挥。

> **提问示例：** *"帮我画个图解释 skill 和 MCP 的区别。"*
> → 它收割出二者的真实区别，走三道闸（知识点 · 套餐 · 强调），交回一个可直接粘的 prompt。

**按平台交付**：**Claude** 端给你一个 `figure-prompt.md` 粘去出图；**Codex** 端若有原生文生图，prompt + 图一起给。两端都不手搓 SVG/PNG。

### 范围
画**机制 / 概念 / 流程示意图**。要画数据图表用绘图 skill，要整套 PPT 用演示 skill。

---

## Example — *skill vs MCP*, end to end · 范例

**The ask / 提问:** *"Help me draw a diagram explaining the difference between a skill and an MCP."*
· *"帮我画个图解释 skill 和 MCP 的区别。"*

**The three gates it surfaces / 它弹出的三道闸:**

| Gate / 闸 | For this figure / 这张图 |
| :-- | :-- |
| ① Knowledge points · 知识点 | what each **is** (a context doc vs a callable server) · how it reaches the model (progressive disclosure vs a tool-call protocol) · what it changes (**knows** vs **does**) |
| ② Package · 套餐 | **PPT** — a talk slide / 报告幻灯 |
| ③ Emphasis · 强调 | **comparison** — foreground the one real difference / 侧重比较 |

**The engineered prompt it hands back / 交回的出图 prompt:**

```text
A clean, professional 16:9 comparison schematic contrasting a "Skill" and an "MCP" for an AI agent. Off-white background, polished Keynote / method-figure look, flat vector-like technical glyphs, soft shadows. Two columns sharing one center, one calm base color per side (slate for Skill, teal for MCP) and a single warm amber accent on the key difference. No cartoon, no glowing/glass machines, no concepts drawn as plain labeled boxes wired by arrows.

VISIBLE CONTENT SOURCE-LOCK:
- Only render the words/entities listed in TEXT below. Do NOT add synonyms, product names, or extra examples.

TEXT (render ALL in one shot, leave NOTHING blank; spell each EXACTLY; English; legible Arial-like sans; horizontal; no overlap; no watermark; no "Figure 1:" bar):
- "Skill", "MCP", "the model"
- "SKILL.md", "tool manifest"
- "instructions / know-how", "running server + tools"
- "loaded into context", "called over a protocol"
- "progressive disclosure", "JSON-RPC tool call"
- "changes what the model KNOWS", "changes what the model can DO"

DO NOT RENDER META CONTENT:
- Do NOT draw layout words ("left"/"column"), color specs, or any of this prompt as visible text — only the labels above plus the figure.

SUBJECT & LAYOUT (two columns sharing ONE center "the model", drawn once):
- LEFT — Skill: a real markdown document labeled "SKILL.md" (visible heading + bullet lines) that unfolds in stages ("progressive disclosure") and flows as text INTO the model's context panel ("loaded into context"); tag the column "instructions / know-how" and "changes what the model KNOWS".
- RIGHT — MCP: a real server with a small "tool manifest" (rows of tool names); the model sends a "JSON-RPC tool call" arrow to the server, which runs and returns a result chip ("called over a protocol"); tag the column "running server + tools" and "changes what the model can DO".
- CENTER: one shared "the model" drawn ONCE between the columns — Skill feeds its context, MCP exchanges call/result with it; both sides' arrows connect to this single shared element.

STYLE & COMPLETENESS:
- Draw each side as its ACTUAL object — Skill as a real document with readable headings/bullets, MCP as a real server with a tool list and a request→response round-trip — NOT two identical labeled rectangles. Medium density, every label legible, the "knows vs does" contrast obvious.

Generate a publication-quality 16:9 comparison figure with the "knows vs does" contrast as the focal point.
```

> **Correct-first:** the figure says *Skill = instructions loaded into context (changes what the model **knows**)* vs *MCP = a running server of tools called over a protocol (changes what the model can **do**)* — each drawn as its real object (a document vs a server with a tool manifest), the shared "the model" drawn once, every label baked in. That's the skill's guardrails doing their job, not a template.
> **先对后画：** 图讲的是 *Skill＝注入上下文的说明书（改变模型"知道"什么）* vs *MCP＝按协议调用的工具服务器（改变模型"能做"什么）*——各画成真实对象（文档 vs 带工具清单的服务器），共享的"the model"只画一个，每个标签都画进图。这就是 skill 的护栏在起作用，而不是套模板。

---

## License
MIT © 2026 emorymao-hub
