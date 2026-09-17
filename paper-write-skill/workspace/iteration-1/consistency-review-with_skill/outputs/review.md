# Consistency Review — DiffuScene-AR (CVPR 2026 Submission)

> Review mode: Mode B (Review existing draft) per `paper-write-skill`.
> Methodology: 找-看-抄-改-修 → here we execute **看 (Look)** + **改 (Modify)** in diagnostic form.
> Scope: Abstract, Introduction (body only), Method §3.1–§3.5.
> Goal: surface every inconsistency across the three sections before submission.

---

## TL;DR — Severity Triage

| # | Issue | Severity | Sections |
|---|---|---|---|
| 1 | Training dataset name and scope disagree | **Critical** | Abstract vs Method §3.5 |
| 2 | Evaluation benchmark name disagrees | **Critical** | Abstract vs Introduction |
| 3 | FID improvement percentage differs (12.4% vs 17.8%) | **Critical** | Abstract vs Introduction |
| 4 | Speedup factor differs (3× vs 4.2×) | **Critical** | Abstract vs Introduction |
| 5 | Promised module "layout-aware scene tokenizer" is absent from Method | **Critical** | Introduction vs Method §3 |
| 6 | "Lightweight" diffusion claim unsupported by training config | Moderate | Abstract vs Method §3.4 |
| 7 | Method section numbering skips §2 (Related Work) | Minor (structural) | Method header |
| 8 | Speedup comparison target differs ("diffusion-only" vs "prior work") | Moderate | Abstract vs Introduction |

---

## 1. Per-Section 4-Component Check

### 1.1 Abstract

| # | Component | Present? | Evidence (quoted) |
|---|---|---|---|
| 1 | Background | ✓ | "Existing 3D scene generation methods either rely on pure autoregressive transformers, which struggle with geometric detail, or pure diffusion models, which are slow at scene-level layout." |
| 2 | Gap | ✓ | "To address this trade-off..." (implicit gap: the trade-off itself) |
| 3 | Method | ✓ | "DiffuScene-AR uses autoregressive prediction to model scene-level layout, then refines individual object geometries with a lightweight diffusion module." |
| 4 | Experiment | ✓ | "DiffuScene-AR achieves a 12.4% improvement in FID over the strongest baseline and runs 3× faster than diffusion-only methods." |

**Verdict:** Abstract is internally well-formed. All 4 components present in correct order. But see §2 for cross-section inconsistencies.

---

### 1.2 Introduction (body only)

| # | Component (per `section-templates.md`) | Present? | Evidence (quoted) |
|---|---|---|---|
| 1 | Big background (1–2 sentences) | ✓ | "Recent advances in generative models have enabled remarkable progress in 3D content creation." |
| 2 | Sub-background (sub-area progress) | ✓ | "two dominant paradigms have emerged: autoregressive transformers, which excel at compositional layout but produce blurry object geometries, and diffusion models, which produce high-fidelity shapes but struggle with scene-level coherence." |
| 3 | Gap / Limitation | ✓ | "However, existing methods typically commit to one paradigm, leaving a clear gap: no approach effectively combines the strengths of both." |
| 4 | Method idea (one sentence) | ✓ | "we propose DiffuScene-AR, a hybrid framework that leverages autoregressive modeling for scene layout and diffusion for object-level refinement." |
| 5 | Method breakdown (3–4 bullets) | ✓ | Three bullets — but **Bullet 2 names a module that does not exist in Method** (see §2.5). |
| 6 | Experiment one-liner | ✓ | "where our method achieves a 17.8% improvement in scene-level FID and a 4.2× speedup over prior work." |
| 7 | Section roadmap | ✗ | Missing entirely. (Optional but standard for CVPR.) |

**Verdict:** Structure complete, but **the numbers/benchmark/module do not match the Abstract or the Method** (detailed below).

---

### 1.3 Method (§3.1–§3.5)

| # | Component (per `section-templates.md`) | Present? | Evidence (quoted) |
|---|---|---|---|
| 1 | Problem formulation (input/output/constraints) | ✓ | "Given a text prompt $T$... our goal is to generate a 3D scene $\mathcal{S} = \{o_1, o_2, \ldots, o_n\}$..." |
| 2 | Core insight (one sentence) | ✗ | Missing a single-sentence "Our key insight is..." statement. The framework description in §3.2 is purely procedural. |
| 3 | Overall framework (figure + paragraph) | ✓ (text only) | "DiffuScene-AR consists of two stages..." |
| 4 | Module 1 (with formula) | ✓ | §3.3 Scene Layout Transformer + autoregressive next-object formula |
| 5 | Module 2 (with formula) | ✓ | §3.4 Object Shape Diffusion (DDPM/DDIM) — **no formula here** |
| 6 | Training objective + loss | ✗ | No loss function defined anywhere. §3.5 lists hyperparameters only. |
| 7 | Implementation details | Partial | §3.5 lists steps/batch/lr, but training data is **inconsistent with Abstract** (see §2.1). |

**Verdict:** Major missing pieces — no formal loss, no "key insight" sentence, and the data claim contradicts Abstract. Also: §3.4 lacks any formula (only the abstract formulation is described).

**Section header note:** Numbering jumps from §1 (Introduction) to §3 (Method). Either §2 (Related Work) is missing, or the heading should be renumbered. Standard CVPR papers need a Related Work section.

---

## 2. Cross-Section Consistency — Critical Findings

### 2.1 Training Dataset: Three different stories

| Section | What it says |
|---|---|
| Abstract | "We train our model on **ProcTHOR-10K, a curated subset of ProcTHOR containing 10K procedurally generated indoor scenes**." |
| Introduction | Does not mention training data. |
| Method §3.5 | "We train on **a combination of ProcTHOR and ScanNet datasets**." |

Three different versions of "where we trained":

- Abstract restricts to a *curated 10K-subset* (ProcTHOR-10K, single dataset).
- Method §3.5 uses the *full* ProcTHOR **plus** ScanNet (two-dataset combination, no mention of -10K subset or 10K scenes).
- Abstract also says ScanNet is the **evaluation** benchmark — Method §3.5 uses ScanNet for **training**, which is at best ambiguous (potential data leakage if ScanNet is also the eval set).

**Recommended rewrite (pick one and stick to it):**

> **Option A — single-dataset training:** Abstract → "We train our model on **ProcTHOR-10K**, a curated 10K-scene subset of ProcTHOR." Method §3.5 → "We train on **ProcTHOR-10K**, consisting of 10K procedurally generated indoor scenes from ProcTHOR."

> **Option B — multi-dataset training:** Abstract → "We train our model on a combination of **ProcTHOR and ScanNet**." Method §3.5 → "We train on ProcTHOR and ScanNet (excluding eval scenes to avoid leakage)."

Whichever is chosen must also be **consistent with the eval benchmark** (see §2.2).

---

### 2.2 Evaluation Benchmark: ScanNet vs Structured3D

| Section | What it says |
|---|---|
| Abstract | "Experiments on the **ScanNet** benchmark demonstrate..." |
| Introduction bullet 3 | "we conduct extensive experiments on the **Structured3D** benchmark, where our method achieves a 17.8% improvement in scene-level FID..." |

These are two genuinely different benchmarks with different scene types, object taxonomies, and FID evaluation protocols. A reviewer will spot this in seconds.

**Recommended rewrite:** Pick one benchmark and use it in **all** places where eval is mentioned. If multiple benchmarks are intended, the Abstract must list **both** (e.g., "on ScanNet and Structured3D benchmarks"); the Introduction bullet should then aggregate numbers rather than citing only one.

If the team only benchmarked on **Structured3D**, the Abstract needs revising. Suggested form:

> "Experiments on the **Structured3D** benchmark demonstrate that DiffuScene-AR achieves a 17.8% improvement in scene-level FID over the strongest baseline and runs 4.2× faster than diffusion-only methods."

---

### 2.3 FID Improvement: 12.4% vs 17.8%

| Section | What it says |
|---|---|
| Abstract | "12.4% improvement in FID" |
| Introduction | "17.8% improvement in scene-level FID" |

A 5.4-point gap between Abstract and Introduction on the headline number. The Introduction adds a "scene-level" qualifier that does not exist in the Abstract — this hints at two different metrics computed against two different baselines, which is not acceptable for a CVPR paper.

**Recommended rewrite:** Align both sections on a single number. If both are real (e.g., 12.4% overall FID, 17.8% scene-level FID), the Abstract must explicitly distinguish:

> "DiffuScene-AR achieves a **12.4% overall FID improvement and a 17.8% scene-level FID improvement** over the strongest baseline."

If only one number is real, pick that one and delete the other.

---

### 2.4 Speedup Factor: 3× vs 4.2×

| Section | What it says |
|---|---|
| Abstract | "runs **3× faster than diffusion-only methods**" |
| Introduction | "a **4.2× speedup over prior work**" |

Both numbers and the comparison target differ. "Diffusion-only methods" is a specific baseline category; "prior work" is vague — CVPR reviewers will flag the latter as weasel wording.

**Recommended rewrite:** Use the same factor and target everywhere. Suggested unified sentence:

> "DiffuScene-AR runs **4.2× faster than diffusion-only baselines** while improving scene-level FID by 17.8%."

(The 50-step DDIM inference in §3.4 supports a multi-fold speedup; 4.2× is plausibly the more defensible measurement. Verify against your actual wall-clock data.)

---

### 2.5 "Layout-aware scene tokenizer" — Phantom Module

| Section | What it says |
|---|---|
| Introduction bullet 2 | "we develop a **layout-aware scene tokenizer** that captures both room-level structure and object-level context." |
| Method | **No tokenizer anywhere.** §3.3 is titled "**Scene Layout Transformer**" — a different concept (a transformer is not a tokenizer; a tokenizer would typically refer to a VQ-VAE / discrete-codebook module). |

A "tokenizer" in 3D scene generation normally implies a discrete latent / VQ-VAE encoder. The Method as written has:

- A CLIP text encoder (text side)
- A 12-layer transformer for layout (token side)
- A 3D diffusion model for shape (output side)

There is no scene-level tokenization module. The Intro promises a contribution that the Method does not deliver.

**Recommended rewrite (three options, pick one):**

> **A. Drop the second bullet from the Introduction** if no scene tokenizer exists. Replace with a real contribution that matches §3.3, e.g.:
> *"Second, we introduce a **layout-aware scene transformer** that conditions object-level predictions on room-level context via cross-attention."*

> **B. Add the tokenizer to the Method** (requires new content — a §3.4 on "Scene Tokenizer" with VQ codebook mechanics). Out of scope for this review.

> **C. Rename the Method module** if a tokenizer was intended but misnamed. Then align both sections on "Scene Layout Tokenizer".

The reviewers who download the code will check that the second contribution is real — option A is the lowest-risk fix.

---

### 2.6 "Lightweight" Diffusion — Unsupported Claim

| Section | What it says |
|---|---|
| Abstract | "refines individual object geometries with a **lightweight diffusion module**." |
| Method §3.4 | "We adopt the **standard DDPM formulation with 1000 denoising steps during training and 50 steps during inference** via DDIM sampling." |

"Lightweight" implies either (a) small parameter count, (b) few inference steps, or (c) low FLOPs. The Method gives none of (a)–(c):

- 1000 training steps + 50 inference steps is the **standard DDPM/DDIM** recipe — not lightweight by itself.
- No parameter count, no FLOPs, no wall-clock per-object latency is given.

**Recommended rewrite:**

> **A. Soften the Abstract:** "with a **compact diffusion module**" (less loaded than "lightweight").

> **B. Justify in Method §3.4:** Add a sentence — e.g., "The diffusion module uses **M.X M parameters** and runs in **Y ms per object** on an A100, ~Z× faster than the autoregressive layout stage." Then the term becomes defensible.

> **C. Drop the adjective:** "refines individual object geometries with a **diffusion module**."

---

### 2.7 Speedup Comparison Target

| Section | Phrasing |
|---|---|
| Abstract | "**3× faster than diffusion-only methods**" (specific baseline class) |
| Introduction | "**4.2× speedup over prior work**" (vague) |

Even setting aside the factor (§2.4), the **target of comparison** differs. "Prior work" is too soft for a CVPR bullet. Unify on a precise target.

**Recommended rewrite:** "...a **4.2× speedup over diffusion-only baselines**." Keep the same target ("diffusion-only") across sections.

---

## 3. Sentence-Connection / Flow Quality

### 3.1 Abstract — Flow

Logical chain holds within the Abstract:

```
[Background: AR vs diffusion trade-off]
   → [Gap: combine the two]
   → [Method: AR layout + diffusion refinement]
   → [Experiment: 12.4% FID + 3× on ScanNet]
```

The connectors are good: "Existing", "To address", "Experiments on". No flow issues **internal to the Abstract**. The problems are all cross-section.

### 3.2 Introduction — Flow

Logical chain:

```
[Recent advances → generative models → 3D content]
   → [two paradigms: AR (blurry objects) vs diffusion (incoherent scenes)]
   → [gap: no hybrid]
   → [propose DiffuScene-AR]
   → [three bullets]
```

Within the Intro, flow is good. The "threefold" enumeration is clean and the bullets parallel in form (verb-led).

**One weakness:** the bullets do not all share the same grammatical structure.

- Bullet 1: "**we introduce** a novel hybrid paradigm..."
- Bullet 2: "**we develop** a layout-aware scene tokenizer..."
- Bullet 3: "**we conduct** extensive experiments..."

These are consistent (all "we + verb"). No issue here. But Bullet 3 mixes a *contribution* (experiments) with contributions 1 and 2 (method components). Convention at CVPR is for the last bullet to also be a method contribution (e.g., a loss, a training strategy, or an inference trick), not "we did experiments". Experiments are a fact-of-paper, not a contribution.

**Suggested rewrite of bullet 3:**

> "**Third**, we introduce a **two-stage training schedule** (200K diffusion steps + 100K autoregressive steps) and demonstrate its effectiveness on the Structured3D benchmark, where our method achieves a 17.8% improvement in scene-level FID..."

Or move the experimental claim out of the contribution list and into a separate sentence after the bullets, the way CVPR papers usually do.

### 3.3 Method — Flow

Within Method, the flow from §3.1 → §3.5 is logical: **problem → framework → module 1 → module 2 → training**. No internal breaks.

However:

- §3.4 has no formula despite the `section-templates.md` template expecting one for each module. Add at least the DDPM forward / reverse process or the DDIM update.
- §3.5 reads as implementation only — no loss function appears anywhere. A "loss" component is missing from the 4-component skeleton (see §1.3 row 6).

---

## 4. Grammar / Style Issues

### 4.1 Minor grammar

| Location | Issue | Suggested fix |
|---|---|---|
| Abstract, "**a 12.4% improvement in FID over the strongest baseline and runs 3× faster than diffusion-only methods**" | Comma before "and runs" would help readability across such a long compound. | "...achieves a 12.4% improvement in FID over the strongest baseline, and runs 3× faster..." |
| Method §3.1, "Each object $o_i$ is represented as a category label $c_i \in \mathcal{C}$, a 3D bounding box $b_i \in \mathbb{R}^6$, and a latent shape code $z_i \in \mathbb{R}^{256}$." | Article missing before "latent shape code". | "...and **a** latent shape code..." |
| Method §3.3, "we encode the text prompt using a pretrained CLIP text encoder, then feed the embeddings" | Missing conjunction. | "...a pretrained CLIP text encoder**, and then** feed the embeddings..." |
| Method §3.5, "The autoregressive transformer is trained for 100K steps with batch size 64, and the diffusion module is trained for 200K steps with batch size 32." | "100K" / "200K" — clarify whether this is iterations or epochs. CVPR convention is "iterations". | "...trained for **100K iterations** with batch size 64..." |
| Method §3.5, "We use the AdamW optimizer with learning rate $1 \times 10^{-4}$." | Singular "rate" for one model; OK, but mixing with two modules above creates ambiguity. | Specify: "**Both stages** use the AdamW optimizer with learning rate $1 \times 10^{-4}$." |

### 4.2 Article / preposition nits

- Method §3.1: "$\mathcal{S} = \{o_1, o_2, \ldots, o_n\}$ consisting of $n$ object instances" → use singular for one set: "**a set** of $n$ object instances" or "**a collection**".
- Method §3.4: "1000 denoising steps **during** training and 50 steps **during** inference via DDIM sampling" → "and **50** steps **at** inference" or "**50 inference steps** via DDIM sampling."

These are cosmetic; reviewers will not fail the paper on them, but they cost you 2 minutes per nit.

---

## 5. Critical Rewrites (consolidated)

### 5.1 Abstract — proposed revision (assumes Option B in §2.1 and Structured3D as the unified benchmark)

> We present DiffuScene-AR, a novel hybrid framework for 3D scene generation that combines autoregressive language modeling with diffusion-based refinement. Existing 3D scene generation methods either rely on pure autoregressive transformers, which struggle with geometric detail, or pure diffusion models, which are slow at scene-level layout. To address this trade-off, DiffuScene-AR uses autoregressive prediction to model scene-level layout, then refines individual object geometries with a compact diffusion module. We train DiffuScene-AR on ProcTHOR-10K, a curated 10K-scene subset of ProcTHOR. Experiments on the Structured3D benchmark demonstrate that DiffuScene-AR achieves a 17.8% improvement in scene-level FID over the strongest baseline and runs 4.2× faster than diffusion-only methods.

(Reconcile training-data wording and unify FID/speedup numbers with the Introduction.)

### 5.2 Introduction — proposed revision (with bullet 3 rephrased and the phantom tokenizer replaced)

> Recent advances in generative models have enabled remarkable progress in 3D content creation. Within this context, two dominant paradigms have emerged: autoregressive transformers, which excel at compositional layout but produce blurry object geometries, and diffusion models, which produce high-fidelity shapes but struggle with scene-level coherence. However, existing methods typically commit to one paradigm, leaving a clear gap: no approach effectively combines the strengths of both.
>
> In this paper, we propose DiffuScene-AR, a hybrid framework that leverages autoregressive modeling for scene layout and diffusion for object-level refinement. Our contributions are threefold:
>
> - **First**, we introduce a novel hybrid paradigm that unifies autoregressive scene layout prediction with diffusion-based shape refinement in a single framework.
> - **Second**, we design a layout-aware scene transformer that conditions per-object predictions on room-level context via cross-attention.
> - **Third**, we propose a decoupled two-stage training schedule that stabilizes joint optimization of the autoregressive and diffusion modules.
>
> Extensive experiments on the Structured3D benchmark demonstrate that DiffuScene-AR outperforms the strongest baseline by 17.8% in scene-level FID and runs 4.2× faster than diffusion-only methods.

(Bullet 2 swapped to "scene transformer" — matches §3.3. Bullet 3 reframed as a real contribution. The aggregate claim kept on a separate sentence after the bullets.)

### 5.3 Method §3.5 — proposed revision

> ### 3.5 Training Setup
>
> We train DiffuScene-AR on **ProcTHOR-10K**, a curated 10K-scene subset of the ProcTHOR dataset. We use the AdamW optimizer with learning rate $1 \times 10^{-4}$ throughout. The autoregressive layout transformer (Section 3.3) is trained for 100K iterations with batch size 64; the diffusion shape module (Section 3.4) is trained for 200K iterations with batch size 32, using 1000 denoising steps during training and 50 DDIM steps at inference. All experiments are evaluated on the **Structured3D** benchmark, which is held out from training to ensure no data leakage.

(Reconciles data claim with Abstract; reconciles eval benchmark with Abstract/Intro; surfaces training/inference efficiency.)

---

## 6. Checklist — Verifiable Fixes for Camera-Ready

Before resubmitting, confirm each item below by re-reading Abstract, Intro, and Method in order:

- [ ] Same **training-dataset** name appears in Abstract + Method (no contradiction).
- [ ] Same **evaluation-benchmark** name appears in Abstract + Intro + (where mentioned) Method.
- [ ] Same **FID improvement %** in Abstract + Intro (single number, or both numbers with explicit "overall" vs "scene-level" split).
- [ ] Same **speedup factor** in Abstract + Intro.
- [ ] Same **comparison target** for speedup ("diffusion-only baselines", not "prior work").
- [ ] Every **module named in the Introduction bullets** has a corresponding Method subsection.
- [ ] Every Method subsection is **referenced from the Introduction** (either explicitly or implied by the layout).
- [ ] Every **adjective used to sell the method** ("lightweight", "novel", "efficient") is supported by a concrete number somewhere in the paper.
- [ ] Section numbering is **continuous** (no jump from §1 to §3).
- [ ] Section roadmap sentence **present** at end of Introduction.
- [ ] **Loss function** appears in Method.
- [ ] **Key insight sentence** ("Our key insight is...") appears in Method §3.2.

---

## 7. Summary for the Author

The draft reads well at sentence level — both Abstract and Introduction follow the standard 4-component skeleton, and the prose is mostly grammatical. However, the cross-section consistency is broken in five concrete ways that any reviewer will catch in the first pass:

1. Training data description in Abstract vs §3.5 disagrees (ProcTHOR-10K vs ProcTHOR+ScanNet; eval vs train).
2. Evaluation benchmark differs (ScanNet in Abstract vs Structured3D in Intro).
3. Headline FID number differs (12.4% vs 17.8%).
4. Speedup factor differs (3× vs 4.2×), and the comparison target shifts from specific ("diffusion-only methods") to vague ("prior work").
5. The Introduction lists a "layout-aware scene tokenizer" as a contribution that does not exist anywhere in the Method section.

Fix the five cross-section mismatches first — they are mechanically verifiable. Then decide whether to add a missing tokenizer to Method, or to drop the bullet from the Introduction. The other items (lightweight adjective, missing Related Work section, missing loss function, section numbering gap) are smaller but still visible to careful reviewers.
