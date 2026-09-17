# Paper-Write-Skill Roadmap

当前 SKILL.md 描述的是 **v1.0** 的能力。本文件列出下一轮迭代（v2.0 / v2.1 / v3.0）的方向，按**用户实际使用中的痛点**驱动。

> 每个方向背后都有一次 `workspace/iteration-N/` 的评估数据支撑——roadmap 不是猜测，是被 `iteration-1/` 验证完的现状里挤出来的下一刀。

---

## v2.0 — Traceability（段落级血缘追踪）

### 当前痛点

Mode A 写稿、Mode B review 的产出都只有最终稿一份。用户读到一段"写得好"的句子，想知道"AI 是从谁那里学来的、学的是哪个 component"，没有溯源通道；想判断"这段是 AI 真在模仿，还是 AI 自己发挥"，也只能凭直觉猜。

这种不透明导致两层问题：
- **好的没法复用**——用户想"再给我来一段同样的"，不知道该让 AI 模仿哪一篇
- **差的没法修**——AI 编的细节藏在稿子里，等评审抓出来才知道

### 目标

每个生成段落附带**血缘三元组**：
- **上游参考**：哪一篇 best paper（标题 / 会议 / 年份）
- **具体段落**：参考的是该论文 abstract / intro / method / experiments 的哪一段
- **句法角色**：导入句 / 过渡句 / 提出方法句 / 机制句 / 结果句中的哪一类

Mode A 写稿时按段标注；Mode B review 时若产出改写段也保留同一血缘标注。

### 预期价值

- "AI 在模仿谁"从盲猜变成可核查链路
- 把"AI 真的在模仿"和"AI 自由发挥"的段落分离开，用户可定向决定保留 / 替换
- 复用更准：用户说"再写一段同样的"，Agent 可直接定位到那篇参考再派生

### 评估方式

`workspace/iteration-2/` 跑一轮 "段落溯源准确率" 评测——给定一段 AI 输出，判断其血缘标注是否与人工标注一致。目标：≥ 80% 段落级一致。

---

## v2.0 — Trigger Robustness（触发鲁棒性）

### 当前痛点

口头触发（"帮我润色"、"review 这段"）在长对话后、或上下文已切换时，偶尔被 Agent 漏掉——尤其当用户消息不带明确"paper" / "abstract" / "顶会"等强触发词时。

`workspace/iteration-1/` 的 trigger-eval-queries.json 测了 20 条 case（10 should-trigger + 10 should-not-trigger），但这只测了**离线查询**——真实会话中的多轮上下文切换还没测。

### 目标

把触发从"严格关键词 + 用户必须点名"升级到两层：

1. **隐式意图推断**：从最近几条消息判断是否处于"学术写作上下文"
   - 显式信号：粘 draft、说投稿 / 顶会名、列会议名、讨论 section 结构
   - 隐式信号：上一条消息触发了 skill，本轮"这段再改一下"自动延续
2. **会话级状态保持**：本窗口内一旦触发过一次，后续"下一个 bullet 重写"、"这段再 polish"自动仍在 skill 模式内，不再要求重新点名

### 预期价值

- 把"每次都得提醒 Agent 走 skill"压到"Agent 自己记得"
- 减少用户在长流程中的提醒成本（"润色 → 再润色 → 再 polish"三连不再需要每轮点名）

### 评估方式

`workspace/iteration-2/` 加一轮"会话级触发"评测——模拟 5 轮真实会话，前一轮触发了 skill，后 4 轮看是否漏触发。目标：5/5 全部命中。

---

## v3.0 — Self-Review Pass（AI 主动自审）

### 当前痛点

用户早期会逐字读稿，后期养成"瞄一眼符合意图就过"的习惯——结果细看之下不少低层毛病（数字跨 section 不一致、缩写首次出现未拼全、引用对不上号、4-component 缺一个、关键 limitation 漏提）漏到交付后，等真实评审来抓。

`workspace/iteration-1/benchmark.json` 的 with-skill 评测已能抓住**用户主动要求 review 时**的大部分问题，但**用户没要求 review 时**的自检还没覆盖。

### 目标

在 Mode A 写稿输出前、Mode B 诊断输出后，**强制插入一轮系统化自检**——按一份固定 checklist 跑：

- **数字一致性**：跨 section 的所有数字（FID / 提升百分比 / 速度倍数）口径一致
- **缩写展开**：所有缩写第一次出现时是否拼全（DDPM / FID / TriPE…）
- **引用对应**：claim 后是否跟了引用、引用编号是否连续
- **4-component 完整性**：按 `references/section-templates.md` 检查每个 section 是否齐全
- **关键句法角色**：导入 / 过渡 / 方法 / 结果 这四类是否每类至少出现一次
- **与参考 best paper 的差异度**：太像 = 抄（需重写）、太不像 = 没学到（需回到 step 2）

自检不通过的项**先内部修订一轮**，再交给用户。

### 预期价值

- 匹配用户"瞄一眼就过"的真实审阅节奏
- 把"靠人审 / 靠用户事后挑毛病"前移到"AI 自审 + 用户只做意图确认"
- 减少交付后被评审抓的低级 bug 数量（迭代-2 评估会量化）

### 评估方式

`workspace/iteration-3/` 跑一轮"自检覆盖率"评测——给 5 篇故意埋 bug 的 draft，看 self-review pass 能抓出多少。目标：抓出率 ≥ 90%。

---

## 版本节奏

| 版本 | 范围 | 触发条件 |
|---|---|---|
| v1.0 | 当前 SKILL.md | 已有 |
| v2.0 | Traceability + Trigger Robustness | iteration-2 eval 通过 |
| v3.0 | Self-Review Pass | iteration-3 eval 通过 |
| v3.x | 后续 | 收集到的真实用户痛点驱动 |

---

## 不在 roadmap 上的东西

为了避免 scope creep，下列**显式不做**：

- 不替用户做学术判断（数字真不真、引用对不对、limitation 写没写——永远是用户的责任）
- 不引入 docx / pptx / xlsx / pdf skill（本 skill 只产 markdown，由用户自己粘贴）
- 不改 5 步心法（找-看-抄-改-修）的骨架——这是核心方法论，不是产品功能
- 不接外部 LLM API（WebSearch / WebFetch / Read 已足够）