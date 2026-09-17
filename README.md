<div align="center">

**中文** · [English](./README.en.md)

# 🧰 SIDO Skills

#### 我每天在用、自己跑通过的一些 AI Skill，都开源在这里

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./LICENSE)
[![Skills](https://img.shields.io/badge/Skills-2-10B981?style=for-the-badge)](#-skills)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97706?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-Skill-10B981?style=flat-square&logo=openai&logoColor=white)
![40+ Agents](https://img.shields.io/badge/40%2B_Agents-Compatible-3B82F6?style=flat-square)

</div>

都是在自己项目里跑通了一段时间，确实省事，才搬出来开源的。没什么花活，就是几个挺实用的东西。

这里的每个 Skill 都是 Agent 能直接加载的结构化指令集，遵循 [Agent Skills](https://agentskills.io) 开放标准。Claude Code、Codex、Qoder、Kimi Code、iFlow、CodeBuddy、Cursor 等 40+ 支持该标准的 Agent 都能装。

---

## 📋 目录

| 名字 | 一句话 | 讲解 |
|---|---|---|
| ✍️ [**paper-write-skill（论文写作）**](#-paper-write-skill论文写作) | 5 步法（找-看-抄-改-修）写 AI / CV / NLP 顶会论文，按 best paper 的骨架和节奏产出 draft | [SKILL.md](./paper-write-skill/SKILL.md) |
| 🔎 [**access-sido-meet（访问 SIDO MEET）**](#-access-sido-meet访问-sido-meet) | 让 Agent 用一句话检索 / 引用 [sido-meet.github.io](https://sido-meet.github.io) 的公开内容（笔记、AI 实战、项目、术语表） | [SKILL.md](./access-sido-meet-skill/SKILL.md) |

---

## 📦 安装方式

在 Claude Code、Codex 等支持 Agent Skills 的工具里，直接说：

```
帮我安装这个 skill：https://github.com/sido-meet/sido-skills/tree/main/<skill-name>
```

把 `<skill-name>` 换成你想装的那个，比如 `paper-write-skill`、`access-sido-meet-skill`。Agent 会自己 clone 到对应目录，不用你操心路径。

你的 Agent 不支持 Skill 也没关系：把对应目录的 `SKILL.md` 全文下载下来，当成项目规则文件（或直接贴进对话）让 Agent 照着执行，效果一致。

---

## ✨ Skills

<a id="-skills"></a>

<table>
<tr><td>

### ✍️ paper-write-skill（论文写作）

> *"顶会论文不是写出来的，是抄出来的——抄的是骨架、抄的是节奏、抄的是逻辑链。"*

给 AI / CV / NLP 顶会论文写作/润色用的结构化方法论。一句话给你一份**对得上 best paper 的 draft**——不是帮你胡编数据，是逼着 Agent 按公认 4-component 公式（背景 → 不足 → 方法 → 实验）组织你的内容。

**它只解决一件事：让论文的"骨架"先对。**

骨架对了再谈修辞；骨架错了一切都是空中楼阁。CVPR / NeurIPS / ICLR 评审第一眼看的就是结构、句子间关系、section 之间的对账。这套 skill 把这一眼变成结构化检查清单。

**两种模式**

- **Mode A：从零写一段** —— 你给主题 + 卖点 + 1-2 个关键数字，skill 找 3-5 篇 best paper → 拆骨架 → 改写成你的稿子
- **Mode B：Review 已有 draft** —— 你贴一段 abstract / intro / method，skill 跑 4-component 诊断 + 跨 section 一致性检查 + 具体改写建议

**5 步心法（找-看-抄-改-修）**

| # | 步骤 | 做什么 |
|---|---|---|
| 1 | **找** | 找强相关顶会 best paper / highlight（WebSearch 或你贴的 abstract） |
| 2 | **看** | 看结构 / 语法 / 句子间关系，标出每句的 component 角色 |
| 3 | **抄** | 抄骨架、抄句式、抄连接词——不要抄原话，抄的是 flow |
| 4 | **改** | 把抄来的骨架换成你的实际内容（背景 / 不足 / 方法 / 实验） |
| 5 | **修** | GPT 自检 + 人工 check 数字 / 引用 / limitation（GPT 会编，必须自己核） |

**内置资源**

- `references/sample-abstracts.md`：5 篇 text-to-3D 顶会 abstract（SAR3D / SLAT / TAR3D / MAR-3D / VAR-3D 改写范例），覆盖 4-component 全部形态
- `references/section-templates.md`：abstract / intro / method / experiments / related work / conclusion / rebuttal 的逐句骨架模板
- `workspace/iteration-1/`：第 1 轮 eval 结果（with-skill 10/10 vs without-skill 7/10，覆盖率 +30%，latency +14.6s）

**怎么触发**

```
帮我写一篇 NeurIPS 2026 的 abstract，主题是基于 diffusion 的 text-to-3D
我准备投 CVPR，帮我 review 一下我的 introduction 写得有没有问题
这个 ECCV highlight paper 的 method 部分我看不太懂，能帮我解读一下吗
投稿 NeurIPS 的 rebuttal 怎么写？reviewer 问了我们 dataset 太小的问题
AI 顶会 paper 的 method section 一般怎么写？有什么固定的段落结构
```

覆盖场景：abstract / introduction / related work / method / experiments / conclusion / rebuttal 全部 section，**不**覆盖：纯技术讨论、代码 review、读书笔记、非学术写作。

→ [SKILL.md](./paper-write-skill/SKILL.md)

</td></tr>
</table>

<table>
<tr><td>

### 🔎 access-sido-meet（访问 SIDO MEET）

> *"AI 工程笔记、LLM / Agent 实战、Text-to-SQL、项目存档、书签收藏、术语表——让 Agent 用一句话就能查。"*

让支持 SKILL.md 的 Agent 用最自然的中文检索 / 阅读 / 总结 / 引用 [sido-meet.github.io](https://sido-meet.github.io) 的公开内容。无需 API Key、无需配置 MCP server。

**它能做什么**

- 跨 7 类内容搜索（article 长文 / note 笔记 / ai AI 栏目 / bookmark 书签 / thought 短思考 / term 术语 / project 项目）
- 按内容类型优先级匹配提问（"调研一个 Text-to-SQL 实战" → 优先 `note`；"介绍我的项目" → 优先 `project`）
- 落地到具体页面引用，每条结论附 canonical URL
- 区分本人写作 vs AI 栏目披露内容
- 找不到匹配内容时，明确声明而不是编

**搜索 workflow**

```
1. 拉 search.json 做宽召回
2. 按 title / summary / content / tags / categories / content_type 评分
3. 按提问类型挑 content_type
4. 需要时再 fetch 完整页面
5. 输出时附 canonical URL
```

**怎么触发**

```
查一下 SIDO MEET 上关于 agent memory 的笔记
SIDO 有哪些 Text-to-SQL 相关的实战
介绍一下 SIDO MEET 术语表里"few-shot"这个词
sido-meet 上有哪些项目
SIDO MEET 最近最火的 5 篇文章
```

回答遵循的规则：**用提问者语言**（中文提问默认中文答）、**有引用才说**、**没找到就明说**、**简答题用 search.json 摘要，长答才 fetch 全文**。

→ [SKILL.md](./access-sido-meet-skill/SKILL.md) · [SIDO MEET 站点](https://sido-meet.github.io) · [Skill 公开指南](https://sido-meet.github.io/skill/)

</td></tr>
</table>

---

## 🌟 关于

这里是 [SIDO MEET](https://sido-meet.github.io) 配套的 skill 仓库。SIDO MEET 是一个记录 AI 工程笔记、LLM / Agent 实战、Text-to-SQL、项目、书签、术语的个人站点；这个仓库里的 skill 都是为了让 Agent 更好地用上站点里的内容、更好地帮人把活干完。

这些 skill 都是我自己每天在用的，开源出来如果对你有帮助，给个 ⭐ 就行。有问题或建议，欢迎在 Issues / Discussions 里说一声。

---

<div align="center">

[MIT License](./LICENSE) · 自由使用 / 修改 / 再分发

Made by [@sido-meet](https://github.com/sido-meet)

</div>