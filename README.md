# storytelling-figures

**v6.0** · **[English](#english) · [中文](#中文)**

A **Claude Code / Codex skill** that turns a conversation into a clean, paste-ready
**image-generation prompt** for scientific, conceptual and explanatory figures — and quietly keeps
the model from making the usual weird mistakes.

---

## English

You're chatting with an AI about some mechanism — how an LLM tokenizes a typo, how an agent picks a skill, your own method's pipeline. At some point you think *"this is hard to explain, let me just draw it."* This skill catches that moment: it reads the conversation you already had, engineers a single high-quality prompt for **GPT image-2 / nano-banana**, and hands it back ready to paste.

It is **not** a figure renderer and **not** a chart library. Its whole job is to write *one good prompt* and avoid the failure modes that make AI-generated figures look wrong or tacky.

### What you can't get by vibe-prompting
A model can already write an image prompt. The catch is **aiming**. Vibe-prompt image-2 yourself and it's a lottery: a vague prompt under-constrains the model, so you get a random draw that rarely lands on the figure you pictured — and drifts on depth, fidelity and style. The one thing this skill does that a casual prompt **structurally can't** is **aim it** — and aiming is two things:

- **🎯 It pins your target.** It pulls out the few decisions that actually narrow the output to *your* figure — which points to feature (≤5), how full (the package), what to emphasize — the constraints vibe-prompting skips, which is exactly why vibe is a lottery.
- **🔒 It locks the logic a diffusion model ignores.** Image-2 doesn't reason about consistency: it invents steps, slips a passing background word ("the model") in as a concept, draws lines from nowhere, warps content to make the layout look nice. The skill front-loads the constraints that forbid each — a source-lock allow-list, a correct-first mechanism check, input↔output closure, no dangling lines — plus a checklist of what to verify.

**Honest about the ceiling.** This **raises the hit-rate and tells you what to check; it does not guarantee** image-2 obeys — you still generate a few and pick the best. And everything *composition* — metaphor, palette, layout — is deliberately left to the model: tested, forcing the layout comes out worse than letting it compose. The skill stays a **guardrail, not a template**.

**Also, but not magic.** It draws each concept as a real object instead of a labeled box and varies every figure to dodge the AI "same-face" look, and it bakes each label in once with short strings to cut CJK garble. These help — but a careful prompter could ask for them too, and image-2 still can't render dense CJK cleanly, so labels are *minimized, not guaranteed*.

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

### vibe 自己调 image-2 拿不到的
模型本来就会写出图 prompt，难的是**瞄准**。你自己 vibe 一句喂给 image-2，基本是摇奖：提示词太含糊、约束不住模型，出来的是一把随机抽，很少正好是你脑子里那张——还会在**详略、保真、风格**上漂移。这个 skill 唯一一件 vibe **结构上做不到**的事，就是**帮你瞄准**——而瞄准是两件事：

- **🎯 钉住你的目标。** 它逼出那几个真正能把输出收窄到*你那张图*的决定——画哪几个点（≤5）、画多满（套餐）、强调什么——正是 vibe 跳过的约束，也正是 vibe 变摇奖的原因。
- **🔒 锁死扩散模型不管的逻辑。** image-2 不做一致性推理：它编步骤、把路过的背景词（泛指的"模型"）当概念塞进去、画出没来源的线、为了布局好看把内容拧失真。skill 把禁止这些的约束**前置**进 prompt——可见内容白名单（source-lock）、先对后画、输入↔输出守恒、线不悬空——外加一份"该检查啥"的清单。

**对天花板说实话。** 它**把命中率拉高、告诉你该查什么；但不保证** image-2 一定照做——你仍是生成几张挑一张。而所有*构图*——隐喻、配色、布局——是**故意**留给模型的：实测硬锁布局比放手更差。skill 始终是**护栏，不是模板**。

**还有，但不是魔法。** 它把每个概念画成真实对象而不是贴标签的方框、并让每张图都不一样以躲开 AI"同脸"；也把每个标签烤一次、尽量用短词来压中文乱码。这些有用——但懂行的人自己也能要求，而且 image-2 仍渲染不好密集中文，所以标签是*尽量少烤、不是保证完美*。

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
