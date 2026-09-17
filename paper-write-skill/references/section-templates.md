# Section Templates (按论文 section 拆解的 4-component)

`SKILL.md` 里 abstract 的 4-component 是 `[背景 → 不足 → 方法 → 实验]`。其他 section 的 4-component 不同。下表是各 section 的"骨架"，skill 在 Mode A 时直接套这个骨架，再填用户内容。

---

## 1. Abstract

| # | 组件 | 句式 |
|---|---|---|
| 1 | 背景 | "X has demonstrated success across Y." |
| 2 | 不足 | "Despite / However, X to Y remains Z." |
| 3 | 方法 | "This paper introduces M, which ..." |
| 4 | 实验 | "Experiments show M surpasses ... in both X and Y." |

参考范例：`sample-abstracts.md` 5 篇。

---

## 2. Introduction

| # | 组件 | 内容 | 句式 |
|---|---|---|---|
| 1 | 大背景 | 领域大趋势（1-2 句） | "In recent years, X has attracted increasing attention due to its Y." |
| 2 | 子背景 | 用户子领域的进展 | "Within this context, several works have explored Z." |
| 3 | 不足 / gap | 现有方法的 limitation（1-3 个） | "However, existing methods suffer from / fail to address ..." |
| 4 | 本文 idea 一句话 | 核心 insight | "In this paper, we propose M, a novel framework that ..." |
| 5 | 方法拆解（3-4 个 bullet） | 每个 bullet = 一个贡献 | "• First, we ...; • Second, ...; • Furthermore, ..." |
| 6 | 实验一句话 | 主要结果 | "Extensive experiments on X benchmark demonstrate that M outperforms Y by Z%." |
| 7 | 章节安排（可选） | 本文结构 | "The rest of this paper is organized as follows. Section 2 introduces ... Section 3 ..." |

**关键**：第 5 步的方法拆解 bullet list 是 introduction 的灵魂——评审第一眼会扫这里。建议 3-4 条，每条 = 一个独立贡献点，用动名词开头（"developing X" / "introducing Y" / "demonstrating Z"）。

---

## 3. Related Work

| # | 组件 | 内容 |
|---|---|---|
| 1 | 分类法 | 把相关工作分 2-4 类（一段话或一句话） |
| 2 | 第一类 | 概述 + 局限 |
| 3 | 第二类 | 概述 + 局限 |
| 4 | 第三类 | 概述 + 局限 |
| 5 | 本文定位 | 跟每一类的差异 + 本文同时做 X 和 Y 的优势 |

**句式模板**：

```
Existing works on X can be broadly divided into three categories:
A-based methods, B-based methods, and C-based methods.

A-based methods [cite, cite] typically ... . Despite their success,
these methods are limited by ... .

B-based methods [cite, cite] ... .

C-based methods [cite, cite] ... .

In contrast to these prior works, our approach simultaneously
addresses X and Y by ... .
```

**禁忌**：
- 不要逐篇列（"Smith et al. [1] proposed X. Lee et al. [2] proposed Y."）→ 顶会 related work 是**主题式**综述，不是 bibliography
- 不要漏掉最近 1 年的强相关工作
- 不要回避跟自己方法最相似的工作——主动对比、承认其优点、说清差异

---

## 4. Method

| # | 组件 | 内容 | 长度 |
|---|---|---|---|
| 1 | 问题形式化 | 输入 X、输出 Y、约束 | 0.5-1 段 |
| 2 | 核心 idea 一句话 | "Our key insight is ..." | 1 句 |
| 3 | 整体框架 | 一个图 + 1 段文字描述 pipeline | 1 段 |
| 4 | 模块 1 | 关键模块细节 | 1-2 段 + 公式 |
| 5 | 模块 2 | 关键模块细节 | 1-2 段 + 公式 |
| 6 | 训练目标 | loss function + 优化方法 | 1 段 + 公式 |
| 7 | 实现细节 | 数据集 / 训练配置 | 半段 |

**句式模板**：

```
## 3. Method

### 3.1 Problem Formulation
We denote the input as X = {x_1, ..., x_n} and the output as Y.
Our goal is to learn a mapping f: X → Y that minimizes ...

### 3.2 Overall Framework
Fig. 2 illustrates the overall pipeline of our method, which
consists of three stages: (1) ..., (2) ..., and (3) ... .

### 3.3 Module Name
The first stage aims to ... . Specifically, we ... .
Formally, given ..., we compute:
    z = f(x; θ)
where θ denotes the learnable parameters.

### 3.4 Training Objective
We train the model by minimizing:
    L = L_recon + λ L_aux
where L_recon is the reconstruction loss and L_aux is the auxiliary loss.
```

**关键**：
- 先画框架图（Fig. N），再写细节——文字描述要能让人**不读图也能懂**
- 每个模块独立小节（3.1, 3.2, 3.3...）便于索引
- 公式编号连续、不跳号
- 不要塞过多 ablation 进 method——ablation 进 experiments

---

## 5. Experiments

| # | 组件 | 内容 |
|---|---|---|
| 1 | Experimental Setup | dataset / metric / baseline / implementation |
| 2 | Main Results | 主表（Table 1）+ 关键观察 |
| 3 | Ablation Study | 各模块贡献（Table 2-3） |
| 4 | Qualitative Results | 可视化（Fig. N）+ 简短文字 |
| 5 | Limitation & Future Work | 诚实地说不足 |

**句式模板**：

```
## 4. Experiments

### 4.1 Experimental Setup
**Datasets.** We evaluate on three benchmarks: X [ref], Y [ref],
and Z [ref]. X provides ..., Y provides ... .
**Metrics.** We adopt two standard metrics: ... and ... .
**Baselines.** We compare against 5 recent state-of-the-art methods,
including A [ref], B [ref], C [ref], D [ref], and E [ref].
**Implementation.** We implement ... in PyTorch and train on
N A100 GPUs for K hours. ... .

### 4.2 Main Results
Table 1 summarizes the main results. Our method consistently
outperforms all baselines across all metrics. Specifically,
we achieve +X.X% improvement over the strongest baseline on
the Y benchmark.

### 4.3 Ablation Study
We conduct ablations to validate each component. As shown in
Table 2, removing module A causes the largest drop (-Y.Y%), which
suggests that A is the most critical component.

### 4.4 Qualitative Results
Fig. 4 visualizes ... . Our method produces sharper details and
more coherent structure compared to the baselines.

### 4.5 Limitation
Our method has two main limitations. First, ... . Second, ... .
We leave these for future work.
```

**关键**：
- Main result 表要有 **statistical significance**（标准差 / p-value / 多 seed）
- Ablation 要有"去掉哪个模块掉多少"的硬数字，不要 soft 描述
- Limitation 一定要有——评审最爱在这里刁难
- 不要 cherry-pick 视觉效果——选代表性 + 失败案例各 1-2 个

---

## 6. Conclusion

| # | 组件 | 长度 |
|---|---|---|
| 1 | 一句话总结本文做了什么 | 1-2 句 |
| 2 | 核心数字 / 贡献 | 1-2 句 |
| 3 | Limitation + Future Work | 1-2 句 |
| 4 | Broader impact（可选） | 1 句 |

**句式模板**：

```
In this paper, we presented M, a novel framework for X.
By integrating A, B, and C, our method achieves state-of-the-art
performance on Y benchmark, outperforming the strongest baseline
by Z%. We believe M opens up new possibilities for ...
```

**禁忌**：
- 不要在 conclusion 里引入新概念
- 不要罗列 abstract 已经说过的内容（除非换个说法）
- 不要写"future work we will explore many directions"——要具体

---

## 7. Rebuttal / Author Response

rebuttal 不是论文 section，但 skill 也覆盖：

| # | 组件 | 句式 |
|---|---|---|
| 1 | 感谢 + 总结评审意见 | "We thank all reviewers for their constructive comments." |
| 2 | 逐条回应（Q1, Q2, ...） | "Q1: [评审原问题]  → 回应 + 证据（实验 / 引用 / 新加的表 / 段落）" |
| 3 | 改动的具体位置 | "We have added new experiments in Section 4.3 (Page X, Table Y)." |
| 4 | 再次感谢 | "We hope these clarifications address the reviewers' concerns." |

**句式模板**：

```
**Q[X]: [Reviewer's concern in one line]**

We thank the reviewer for raising this important point. We agree
that ... . To address this concern, we have:

1. [动作 1]：具体改动（"added a new ablation in Table X"）
2. [动作 2]：补充说明（"clarified in Section Y, Page Z"）
3. [动作 3]：新实验（"conducted additional experiments on dataset D"）

请见 Fig./Table X 中的新结果。
```

**关键**：rebuttal 要**具体到章节、页码、表格编号**，不要泛泛"我们会改进"。

---

## 总结：通用 4-component 变形

| Section | 4-component |
|---|---|
| Abstract | 背景 + 不足 + 方法 + 实验 |
| Introduction | 背景 + gap + 本文 idea + 方法拆解 + 实验 |
| Related Work | 分类法 + 逐类评述 + 本文定位 |
| Method | 问题形式化 + 核心 idea + 整体框架 + 模块细节 |
| Experiments | setup + main result + ablation + 可视化 + limitation |
| Conclusion | 做了什么 + 核心数字 + limitation |
| Rebuttal | 感谢 + 逐条回应 + 改动位置 |

**一句话**：每个 section 都是"先告诉读者**为什么**（背景/不足），再告诉读者**做了什么**（方法/实验/对比），最后告诉读者**这样做的结果**（数字 / 表 / 图）"。