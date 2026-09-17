---
name: paper-write-skill
description: A 5-step paper writing methodology (找-看-抄-改-修 / find-look-copy-modify-polish) for AI / CV / NLP academic papers. Use this skill whenever the user is writing, drafting, polishing, or reviewing any section of an academic paper — abstract, introduction, related work, method, experiments, conclusion — or any time they mention CVPR, ECCV, NeurIPS, ICML, ICLR, KDD, ACL, EMNLP, NAACL, AAAI, IJCAI, T-PAMI, IJCV, best paper, oral, highlight, rebuttal, camera-ready, or 投稿. The skill operates in two modes — "write from scratch" (producing a draft that imitates the structure, grammar, and sentence flow of current best papers) and "review existing draft" (returning a diagnostic checklist plus concrete rewrites for structure / grammar / sentence-connection / consistency issues). Always trigger on academic-writing requests even when the user does not explicitly ask for a "paper writing skill", because the methodology is broadly applicable and the user usually wants this exact 5-step workflow applied.
---

# Paper Write Skill

## 核心方法论：找 → 看 → 抄 → 改 → 修

这是一套 **AI / CV / NLP 顶会论文**写作与润色的工作流，以 abstract 为示例提炼，但同样的逻辑适用于论文的**任意 section**——abstract、introduction、related work、method、experiments、conclusion 都按这 5 步走。

| 步骤 | 含义 | 工具/输入 |
|---|---|---|
| **1. 找** | 找强相关顶会论文 / best paper | WebSearch 或用户提供 |
| **2. 看** | 看结构、看语法、看句子间关系 | 用户提供的 abstract / section 文本 |
| **3. 抄** | 抄结构、抄语法、抄句子关系 | "谁火就用谁的 flow" |
| **4. 改** | 在模仿基础上改写 | 用户自己的内容 |
| **5. 修** | GPT + 人工 check | 终稿 |

---

## 何时触发（Trigger 列表）

满足**任一**即触发：

- 用户说"帮我写 abstract / intro / method / experiments / conclusion / rebuttal"
- 用户粘贴一段论文草稿，让 Claude 润色 / 改写 / 评审 / 找问题
- 用户提到顶会：CVPR / ECCV / NeurIPS / ICML / ICLR / KDD / ACL / EMNLP / NAACL / AAAI / IJCAI / T-PAMI / IJCV / best paper / oral / highlight
- 用户提到投稿、camera-ready、rebuttal、审稿意见回复
- 用户讨论 paper structure / paper flow / paper grammar
- 用户问"这一段写得怎么样" / "怎么写好 introduction"

**不要**触发的情况：纯技术问题讨论、代码 review、读书笔记、非学术写作（博客、新闻稿、产品文案）。

---

## 两种模式（先问清楚，再开工）

启动 skill 后第一件事是判断走哪条路。如果用户没说清，**先问一句**：

```
你这次是 (A) 从零写一段，还是 (B) 让我 review 你已经写好的稿子？
```

### Mode A：从零写一段

输入：用户给主题 + 关键卖点（方法名、核心 idea、关键数字）。
流程：找 → 看 → 抄 → 改 → 修，全程产出最终段落。

### Mode B：Review 已有 draft

输入：用户粘贴的 draft（abstract / intro / method / 任意 section）。
流程：跳到「看 + 改 + 修」——直接做诊断，输出问题清单 + 具体改写建议。

---

## Step 1：找（Find）

### 先问用户，再决定要不要搜

不要一上来就 WebSearch。优先问用户：

```
你需要我联网找最近的 best paper 吗？
- 你已经有 reference papers（请贴 1-3 篇 abstract / 全文）
- 你想我搜（告诉我会议 + 年份 + 子领域，如 "CVPR 2025 text-to-3D"）
- 你想先用我内置的范例 abstract（见 references/sample-abstracts.md）
```

如果用户说"搜"，再 WebSearch。重点搜：

- 最近 1-2 年的 CVPR / ECCV / NeurIPS / ICLR / ICML best paper、highlight、oral
- 子领域关键词（text-to-3D、diffusion、agent、reasoning、alignment…）
- 找 3-5 篇**结构最像**用户主题的 abstract，提取共性

### 如果不搜

直接读 `references/sample-abstracts.md`，那里有 4 篇预置范例（SAR3D / SLAT / TAR3D / MAR-3D）+ 1 篇改写范例（VAR-3D），覆盖 4-component 公式。

---

## Step 2：看（Look）

拿到 reference abstracts 之后，按三个维度分析每一篇：

### 2.1 看结构

每篇 abstract / section 都按下面这个 4-component 骨架展开：

```
[1] 背景 + 已有方法的不足
[2] 你的卖点（核心 insight / 关键 idea）
[3] 实现细节（粗略即可，不要全展开方法细节）
[4] 实验结果（数字 / dataset / 相比 SOTA 的提升）
```

判断 reference 的 4 个 component 顺序是否一致、是否齐全。

### 2.2 看语法

摘出代表性句式，分类记录：

- **领域导入句**："Autoregressive models have demonstrated X across Y."
- **过渡句**："Despite these advances, applying X to Y remains Y."
- **提出方法句**："This paper introduces Z, a novel framework that ..."
- **数字 / 结果句**："Our experiments show that Z surpasses ... in both X and Y."
- **结尾句**："Extensive experiments demonstrate that Z achieves ..."

每类至少摘 2-3 个原句模板。

### 2.3 看句子间关系

abstract / section 内部句子不是平铺的，而是有 **chain of logic**：

```
背景(A) → 不足(B) → 本文针对 B 提 C → C 的关键 idea(D) → D 的实现粗描(E) → 实验验证(F)
```

判断 reference 的句子是怎么从 A 推到 F 的。常见连接词：Despite、However、Specifically、To address、Additionally、Moreover、Extensive experiments。

---

## Step 3：抄（Copy）

> 引用一句原笔记的话：**抄别人的 flow，谁火就用谁的，懂？**

这一步是把 reference 的"骨架"原样拿来填用户的内容。

具体操作：

1. **抄结构**：固定 [背景→不足→方法→实验] 四段顺序不变
2. **抄语法**：每个 component 用 reference 里出现过的句式类型（导入句 / 过渡句 / 提出方法句…）
3. **抄句子关系**：用 reference 用过的逻辑链（Despite→However→Specifically→To address→Additionally→Experiments）

不要抄原话。要抄的是**结构和连接逻辑**。

---

## Step 4：改（Modify）

把 Step 3 抄出来的骨架，换成用户的实际内容：

| 组件 | 替换为 |
|---|---|
| 背景 | 用户研究领域 + 已有工作 |
| 不足 | 用户发现的痛点 / limitation |
| 方法 | 用户的方法名 + 核心 insight |
| 实现 | 一两个关键模块名即可，**不要写公式** |
| 实验 | 用户的关键数字 + 对比 SOTA |

**反例**：直接复制 reference 里的具体名词（VQ-VAE、TriPE、SLAT…）→ 必须替换成用户的术语。

改写范例见 `references/sample-abstracts.md` 里的 **VAR-3D**——它就是从 SAR3D 改出来的（同领域、同结构、不同术语、不同 insight）。

---

## Step 5：修（Polish）

GPT + 人工 check 两遍：

### GPT 第一遍（auto-polish）

让 GPT 自查：

- 4 个 component 是否齐全、顺序对不对
- 是否有语法错误（主谓一致、冠词、时态）
- 句子间连接是否流畅
- 是否有冗余 / 重复表达

### 人工 check 第二遍（用户必做）

提醒用户检查：

- 数字 / dataset / baseline 名称是否真实（**GPT 会编**）
- 缩写第一次出现是否拼全
- 引用是否齐全
- 是否漏掉关键 limitation / future work
- 论文整体的一致性（method 里说的事 abstract 里有没有提）

---

## 详细章节模板

不同 section 的 4-component 拆解略有不同。详见 `references/section-templates.md`：

- Abstract：背景+不足+方法+实验
- Introduction：背景→动机→本文贡献→章节安排
- Method：问题形式化→核心 idea→模块拆解→训练 loss
- Experiments：setup→main result→ablation→可视化→limitation
- Related Work：分类法 → 逐类对比 → 本文定位
- Conclusion：核心贡献一句话 + limitation + future work

---

## 输出格式

### Mode A 输出（write）

```
## [Section 标题]

[Section 内容]

---
### 本次参考的 best paper
- [paper 1 标题 / 会议 / 年份 / 一句话结构]
- [paper 2 ...]

### 4-component 对照
| 组件 | 你的稿子 | 参考稿子 |
|---|---|---|
| 背景 | ... | ... |
| 不足 | ... | ... |
| 方法 | ... | ... |
| 实验 | ... | ... |
```

### Mode B 输出（review）

```
## 诊断清单

### 结构 (structure)
- [✓/✗] 4-component 齐全
- [✓/✗] 顺序正确
- [建议]

### 语法 (grammar)
- [逐句标注]

### 句子间关系 (flow)
- [chain of logic 检查]

### 一致性 (consistency)
- [method / abstract 对照]
- [figure / table 对照]

## 改写建议
- 句 N: 原句 → 建议改写
- ...
```

---

## 关键原则

1. **不要替用户做学术判断**——数字、引用、limitation 永远是用户的责任
2. **GPT 会编具体细节**——遇到"具体某篇论文用某方法得到某数字"必须让用户确认
3. **句子间连接 > 单句语法**——顶会评审看的是 flow 不是 vocabulary
4. **结构稳定 > 修辞新颖**——抄稳的 flow 比剑走偏锋安全
5. **AI 顶会写作 ≠ 文学写作**——句子要"重"、要"实"，避免感性词

---

## 依赖 / 工具

- **WebSearch**：Step 1 检索顶会 best paper（用户确认后才用）
- **WebFetch**：抓取 best paper abstract 详情
- **Read**：读取用户提供的 draft / reference 文本

不依赖 docx / pptx / xlsx / pdf skill——本 skill 只产生 markdown 文本，由用户自行粘贴到论文。

---

## 参考资料

- `references/sample-abstracts.md`：4 篇预置 reference abstracts（结构 / 语法 / 句子关系范例）+ 1 篇改写范例
- `references/section-templates.md`：abstract / intro / method / experiments / related work / conclusion 的 4-component 拆解

当用户提供具体 reference 时再读相应 section 模板；默认 abstract 模板已内置在本文件 Step 2。