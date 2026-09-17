# Paper-Write-Skill

A 5-step paper writing methodology (找-看-抄-改-修 / find-look-copy-modify-polish) for AI / CV / NLP academic papers, packaged as a Claude Skill.

## What it does

When the user is writing, drafting, polishing, or reviewing any section of an academic paper, this skill applies a structured methodology that mirrors how experienced researchers read and imitate top-venue papers:

| Step | Action |
|---|---|
| **1. 找 (Find)** | Locate best papers in the user's sub-area (top venues: CVPR / ECCV / NeurIPS / ICML / ICLR / KDD / ACL / EMNLP / NAACL / AAAI / IJCAI / T-PAMI / IJCV) |
| **2. 看 (Look)** | Analyze each reference on three axes: structure / grammar / sentence-by-sentence connections |
| **3. 抄 (Copy)** | Borrow the structure, grammar patterns, and flow from whichever reference is strongest |
| **4. 改 (Modify)** | Swap the reference's content for the user's actual content, keeping the borrowed skeleton |
| **5. 修 (Polish)** | GPT pass + manual check (the user must verify numbers / citations / limitations — the model cannot) |

The skill works for **any** paper section — abstract, introduction, related work, method, experiments, conclusion, rebuttal — not just abstracts.

## Repository structure

```
paper-write-skill/
├── README.md                            # this file
├── SKILL.md                             # main file: 5-step methodology + trigger list + two modes
└── references/
    ├── sample-abstracts.md              # 5 sample abstracts (4 standards + 1 rewrite demo)
    └── section-templates.md             # 4-component breakdown for each paper section
```

## Installation

### Option A: Direct install (Claude Code / Cowork)

Copy this folder into your Claude skills directory:

```
# macOS / Linux
~/.claude/skills/paper-write-skill/

# Windows
%USERPROFILE%\.claude\skills\paper-write-skill\
```

Restart Claude. The skill appears in `available_skills` and triggers on academic-writing requests.

### Option B: Install from `.skill` package

A pre-packaged `.skill` file is provided in the parent project directory:

```
paper-write-skill.skill
```

Double-click it, or install via:

```bash
# from a Claude Code session
/install-skill path/to/paper-write-skill.skill
```

## Usage

The skill triggers automatically when the user mentions:

- Writing / drafting / polishing / reviewing any paper section (abstract, intro, method, experiments, related work, conclusion, rebuttal)
- Top venues: CVPR / ECCV / NeurIPS / ICML / ICLR / KDD / ACL / EMNLP / NAACL / AAAI / IJCAI / T-PAMI / IJCV / best paper / oral / highlight
- 投稿 / camera-ready / 审稿回复

When triggered, the skill first asks whether to operate in:

- **Mode A — Write from scratch**: user provides topic + selling points, skill produces a draft
- **Mode B — Review existing draft**: user pastes a draft, skill produces a diagnostic checklist + rewrites

## Two modes, one methodology

| Mode | Input | Output |
|---|---|---|
| A (Write) | Topic + selling points + 1-2 key numbers | A draft that imitates the structure / grammar / flow of current best papers |
| B (Review) | Pasted draft (any section) | Diagnostic checklist (structure / grammar / flow / consistency) + concrete rewrite suggestions |

In both modes, the 4-component skeleton applies:

```
[背景 / background] + [你的卖点 / selling point] + [实现细节（粗糙）/ method outline] + [实验结果 / experiment]
```

## Customization

The reference abstracts in `references/sample-abstracts.md` are intentionally generic — they cover text-to-3D generation but the structure / grammar patterns transfer to any AI / CV / NLP subfield. To specialize for a different area:

1. Replace `references/sample-abstracts.md` with 4-5 abstracts from your target venue / subfield
2. Keep the 4-component structure intact (background / gap / method / experiment)
3. Keep the analytical annotations that mark each sentence's structural role

To add more section templates:

1. Append to `references/section-templates.md`
2. Follow the same per-section table format (4-component + sentence templates)

## Repackaging

If you edit the skill, repackage the `.skill` file with the skill-creator's `package_skill.py`:

```bash
cd path/to/skill-creator
PYTHONUTF8=1 PYTHONIOENCODING=utf-8 PYTHONPATH=. python scripts/package_skill.py \
  /path/to/paper-write-skill
```

## License

MIT (or specify your own before publishing).

## Contributing

This skill came out of a personal paper-writing workflow and may not generalize to all subfields. If you adapt it for, say, security / systems / theory papers, please:

1. Update the venue list in `SKILL.md` if needed
2. Add subfield-specific reference abstracts to `references/sample-abstracts.md`
3. Open a PR