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
A model can already write an image prompt. The catch is **aiming it** — the things you *can't* get by prompting an image-generation AI yourself, no matter how you phrase it:

| What you get | By yourself | With the skill |
| :-- | :-- | :-- |
| **🎯 Hits your target** | ❌ a vague prompt → a random draw, rarely the figure you pictured | ✅ pins the few choices that narrow it to *your* figure — which points (≤5), how full, what to emphasize |
| **🔒 No made-up / stray content** | ❌ invents steps, slips a passing background word in as a concept | ✅ a word allow-list (source-lock) — only what's on it gets drawn, nothing invented |
| **📐 Logically consistent** | ❌ lines from nowhere, content warped to make the layout look nice | ✅ input↔output closure, no dangling lines, content not bent for looks |

**Honest about the ceiling.** It **raises the hit-rate and tells you what to check — it doesn't guarantee** the model obeys; you still generate a few and pick the best. *Composition* (metaphor, palette, layout) is deliberately left to the model — forcing it tested worse. It also pushes every concept to read even with its text label removed (more visual, less labelling) and keeps labels legible — helpful, but a careful prompter can ask for those too, and dense CJK still garbles. The skill is a **guardrail, not a template**.

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
模型本来就会写出图 prompt，难的是**瞄准**——那些你自己怎么对图片生成 AI 提示都**拿不到**的东西：

| 你能得到 | 自己提示 | 装上 skill |
| :-- | :-- | :-- |
| **🎯 打得中你的目标** | ❌ 含糊一句 → 随机抽，很少是你脑中那张 | ✅ 钉住那几个把输出收窄到*你那张图*的选择——画哪几个点（≤5）、画多满、强调什么 |
| **🔒 不臆造、不串词** | ❌ 编步骤、把路过的背景词当概念塞进图 | ✅ 设一份用词白名单（source-lock）——只画名单里的，不乱写 |
| **📐 逻辑自洽** | ❌ 线从空白处飘来、为布局好看把内容拧失真 | ✅ 输入↔输出守恒、线不悬空、内容不为好看变形 |

**丑话说前头。** 它**把命中率拉高、告诉你该查什么——但不保证**模型一定照做；你仍是生成几张挑一张。*构图*（隐喻、配色、布局）**故意**交给模型——硬锁实测更差。它还会**提高形象化**（抹掉文字注释也认得出）、并尽量让标签清晰——有帮助，但懂行的人自己也能要求，且密集中文仍会糊。skill 是**护栏，不是模板**。

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

## Example — *skill vs MCP* · 范例

**Just ask in plain words / 直接用大白话问:**
> *"Help me draw a diagram explaining the difference between a skill and an MCP."*
> *"帮我画个图解释 skill 和 MCP 的区别。"*

You don't invoke anything or write any prompt yourself. The agent **recognizes the intent and invokes the skill automatically**, then asks you a few quick clickable choices and hands back the finished prompt (on Codex, the image too).

你不用手动调用、也不用自己写 prompt。**agent 会自动识别意图、调起这个 skill**，弹几个可点选择让你定，再把做好的 prompt 交给你（Codex 端连图一起给）。

---

## License
MIT © 2026 emorymao-hub
