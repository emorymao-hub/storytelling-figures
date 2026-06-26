# storytelling-figures

**[English](#english) · [中文](#中文)**

A **Claude Code / Codex skill** that turns a conversation into a clean, paste-ready
**image-generation prompt** for scientific, conceptual and explanatory figures — and quietly keeps
the model from making the usual weird mistakes.

---

## English

You're chatting with an AI about some mechanism — how an LLM tokenizes a typo, how an agent picks a skill, your own method's pipeline. At some point you think *"this is hard to explain, let me just draw it."* This skill catches that moment: it reads the conversation you already had, engineers a single high-quality prompt for **GPT image-2 / nano-banana**, and hands it back ready to paste.

It is **not** a figure renderer and **not** a chart library. Its whole job is to write *one good prompt* and avoid the failure modes that make AI-generated figures look wrong or tacky.

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
Start a new session — it auto-discovers and triggers on intent. **For a Chinese-language skill body,** rename `SKILL.zh.md` to `SKILL.md`.

### How to use it
Don't invoke it by hand. Talk through a concept, then say *"draw what we just discussed"* / *"this is too confusing, draw it so I get it"* / *"I need a journal method figure, give me layout ideas."* It harvests the chat, brainstorms the candidate knowledge points and asks which to feature (≤5), then a package (sci-comm / PPT / detailed / journal) and 1–3 layout directions to pick from, then engineers the prompt.

**Output by platform:** on **Claude** you get one `figure-prompt.md` to paste into the image model. On **Codex** with a native text-to-image tool, you get the prompt **and** the image together. Neither hand-codes SVG/PNG.

### What's inside
```
SKILL.md       # the whole skill (English), self-contained: workflow + guardrails
SKILL.zh.md    # the same skill in Chinese (rename to SKILL.md to use it)
examples/      # two worked examples, one per intent:
               #   understand-it (skill vs MCP) · produce-it (a misspelled word)
README.md      # this file
LICENSE        # MIT
```

### Scope
Draws **mechanism / concept / process schematics**. For data charts use a plotting skill; for full slide decks use a presentation skill.

---

## 中文

你正跟 AI 聊某个机制——LLM 怎么把打错的词切 token、agent 怎么挑 skill、你自己方法的 pipeline。聊到某刻你想 *"这太难讲了，画出来吧。"* 这个 skill 就接住这一刻：回看你已经聊过的对话，工程化出一个高质量的 **GPT image-2 / nano-banana** 出图 prompt，直接给你粘。

它**不是出图器、也不是画图表的库**。它唯一的活，是写**一个好 prompt**，并挡掉那些让 AI 配图又错又俗的翻车。

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
新开会话即自动发现、按意图触发。**想用中文版规则**：把 `SKILL.zh.md` 改名成 `SKILL.md` 即可。

### 怎么用
不用手动调。聊透一个概念后随口说 *"把刚才这个画成图"* / *"太复杂了画出来理一理"* / *"我要画张 journal 方法图，给点布局灵感"*。它会收割对话、脑暴出可选知识点让你勾（≤5），再选套餐（科普 / PPT / 详细 / journal）和 1–3 个布局方向，最后工程化出 prompt。

**按平台交付**：**Claude** 端给你一个 `figure-prompt.md` 粘去出图；**Codex** 端若有原生文生图，prompt + 图一起给。两端都不手搓 SVG/PNG。

### 范围
画**机制 / 概念 / 流程示意图**。要画数据图表用绘图 skill，要整套 PPT 用演示 skill。

---

## License
MIT © 2026 emorymao-hub
