# paper-write-skill workspace

评估 / 迭代用的开发材料。克隆仓库后会看到 skill 的实际验证数据。

## 目录

| 文件 / 目录 | 说明 |
|---|---|
| `evals/` | Eval 测试集（输入 + prompts） |
| `evals/evals.json` | 所有 eval 用例（当前只有 1 个：consistency-review） |
| `evals/inputs/consistency-review-draft.md` | 测试用 draft（CVPR 投稿模拟，含5 个故意埋的不一致） |
| `iteration-1/` | 第 1 轮 eval 结果 |
| `iteration-1/consistency-review-with_skill/` | 用 skill 跑出来的结果 |
| `iteration-1/consistency-review-without_skill/` | baseline（不调用 skill）跑出来的结果 |
| `iteration-1/review.html` | 两份输出的可视化对比（用 skill-creator 的 generate_review.py 生成） |
| `iteration-1/benchmark.json` | 量化对比（with_skill 10/10 vs without_skill 7/10） |
| `iteration-1/desc-opt-report.html` | Description 优化报告（Windows 环境受限，run_loop 跑不全） |
| `trigger-eval-queries.json` | 触发评估 20 条查询（10 should-trigger + 10 should-not-trigger） |

## 验证结论

第 1 轮 eval（consistency-review 任务）：

| | with-skill | without-skill |
|---|---|---|
| Pass rate | **10/10** | **7/10** |
| 时长 | 77s | 62s |

with-skill 多覆盖的 3 项：①显式 4-component 诊断表、② camera-ready checklist、③显式引用 paper-write-skill 方法论。

## 如何重跑

需要 skill-creator 在 PATH 里：

```bash
# 重新跑 with-skill 和 baseline（2 个 agent 并行）
cd ../../..  # 回到 Claude skills 目录的同级
# 复制 evals/inputs/ 给 agent，prompt 让它读 .claude/skills/paper-write-skill/SKILL.md
# 然后跑 generate_review.py 重新生成 review.html
```

## 限制

- `iteration-N/` 是累积历史，不要删除
- 下一轮 eval 应该建 `iteration-2/`，不要覆盖 `iteration-1/`
- 描述优化在 Windows 下 `subprocess` 找不到 `claude.cmd`，report 仅作记录参考