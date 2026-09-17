# Consistency Review — DiffuScene-AR (CVPR 2026 draft)

This review checks structural soundness and cross-section consistency across the Abstract, Introduction, and Method sections of the draft. The Method section is numbered 3.1–3.5 (it appears Section 2 — Related Work — has been omitted from the draft as provided; this itself is a structural flag, see below).

---

## 1. Summary of Findings

The three sections disagree on at least four load-bearing facts that any reviewer will spot immediately: the **evaluation benchmark**, the **FID improvement number**, the **speedup factor**, and the **training datasets**. There are also two smaller consistency issues around the diffusion module ("lightweight" in the Abstract vs. 1000-step DDPM in §3.4) and the role of ScanNet (evaluation dataset in the Abstract, training dataset in §3.5). On top of this, the Abstract's training claim (ProcTHOR-10K) and §3.5's training claim (ProcTHOR + ScanNet) do not match.

Concrete counts:
- 4 numerical / naming mismatches between Abstract ↔ Introduction
- 3 dataset mismatches between Abstract ↔ Introduction ↔ Method
- 1 wording mismatch ("lightweight" vs. 1000-step DDPM)
- 1 structural omission (missing Section 2)
- Several minor grammar / clarity issues (see §4)

The biggest single risk is the **FID improvement / speedup / benchmark triple mismatch** — these are the headline numbers of the paper and they currently contradict each other between the Abstract and Introduction. This must be fixed before submission.

---

## 2. Structure Check

### 2.1 Abstract
The Abstract is one long paragraph. For CVPR, this is acceptable but it does slightly over-concentrate information. It currently contains:
- (a) the problem framing ("Existing 3D scene generation methods..."),
- (b) the proposed approach,
- (c) the training data,
- (d) the benchmark + two headline numbers.

A common CVPR pattern is to keep this dense, but consider whether the training-data clause ("We train our model on ProcTHOR-10K, a curated subset of ProcTHOR containing 10K procedurally generated indoor scenes.") belongs here or in the Method. As-is it commits the Abstract to a specific training set that §3.5 then contradicts.

The Abstract does not mention the dataset scale (10K scenes) anywhere else in the draft — only §3.5 mentions ProcTHOR, and it does so without the "10K" qualifier.

### 2.2 Introduction
The Introduction provides only the body (paragraphs on the two paradigms + a contribution bullet list). As supplied it is missing:
- an opening / "teaser" paragraph that motivates 3D scene generation,
- a paragraph on related work or positioning vs. prior art,
- a final paragraph previewing the Method structure.

For CVPR, an Introduction is typically 4–6 paragraphs. The current draft is closer to 2 effective paragraphs (one prose paragraph + one contribution list). This is structurally thin and would likely trigger a "motivation is shallow" reviewer comment.

Also, **the section numbering jumps from 1 to 3**. Either Section 2 (Related Work) needs to be added, or this draft is excerpted from a longer paper — but as provided, the paper would submit with §1 → §3 which is invalid.

### 2.3 Method
The Method is organized into 3.1–3.5 with a logical flow (problem → framework → layout module → diffusion module → training). Structurally this is fine. Two issues:
- §3.5 "Training Setup" sits at the end of the Method, but it reads like an experiments/training-config paragraph. Many CVPR methods put this in the Experiments section. As-is it is acceptable, but readers may expect to see the loss formulation and training-objective details here, not just step counts and optimizer.
- The Method never introduces the **ProcTHOR-10K subset**, the **CLIP text encoder**, or the **12-layer transformer** that are claimed in the Abstract or implicitly assumed. If these are part of the paper they should be at least named in §3.

---

## 3. Cross-Section Consistency

This is the core of the review. I list each inconsistency with the exact quoted lines.

### 3.1 Benchmark — three different names
- Abstract: *"Experiments on the **ScanNet** benchmark demonstrate that DiffuScene-AR achieves..."*
- Introduction: *"we conduct extensive experiments on the **Structured3D** benchmark, where our method achieves..."*
- Method §3.5: *"We train on a combination of **ProcTHOR and ScanNet** datasets."*

The Abstract and Introduction disagree on the evaluation benchmark. §3.5 introduces ScanNet as a *training* dataset, which contradicts its role as an *evaluation* benchmark in the Abstract. The Introduction's "Structured3D" appears in no other section.

**Suggested fix:** pick one evaluation benchmark (likely ScanNet, since it is most commonly used for 3D scene generation FID) and use it consistently across the Abstract, Introduction, and Experiments. If Structured3D is also used, list it explicitly in both Abstract and Introduction. If ScanNet is used as both training and evaluation, that is unusual and must be called out (and §3.5 must clarify whether train/test splits are disjoint).

### 3.2 FID improvement — 12.4% vs. 17.8%
- Abstract: *"DiffuScene-AR achieves a **12.4% improvement in FID** over the strongest baseline"*
- Introduction: *"our method achieves a **17.8% improvement in scene-level FID** and a 4.2× speedup"*

These numbers cannot both be true unless they refer to different baselines or different settings. Neither section qualifies them with the comparison partner ("over the strongest baseline" in the Abstract, "over prior work" in the Introduction — different phrasings, no shared baseline).

**Suggested fix:** decide on a single headline FID number and a single comparison partner, and use the same phrasing in both sections. If both numbers are valid (e.g., 12.4% over the strongest single baseline and 17.8% averaged across multiple baselines), say so explicitly in both places.

### 3.3 Speedup — 3× vs. 4.2×
- Abstract: *"runs **3× faster** than diffusion-only methods"*
- Introduction: *"a **4.2× speedup** over prior work"*

Same problem as §3.2. Also note the comparison class differs: "diffusion-only methods" (Abstract) vs. "prior work" (Introduction, broader).

**Suggested fix:** one number, one comparison class. If both are true, justify in one sentence in the Introduction ("3× over diffusion-only baselines, 4.2× over the best prior autoregressive baseline") and trim the Abstract accordingly.

### 3.4 Training dataset — ProcTHOR-10K vs. ProcTHOR + ScanNet
- Abstract: *"We train our model on **ProcTHOR-10K**, a curated subset of ProcTHOR containing 10K procedurally generated indoor scenes."*
- Method §3.5: *"We train on a combination of **ProcTHOR and ScanNet** datasets."*

The Abstract commits to a single curated subset called ProcTHOR-10K. §3.5 introduces a *combination* of two datasets and does not mention the 10K subset. The Abstract's "10K" figure is also not used elsewhere.

**Suggested fix:** state the training set consistently in all sections. If the model is trained on the union of ProcTHOR and ScanNet, say so in the Abstract. If a 10K curated subset is used, name it in §3.5. (Note: training on ScanNet for FID evaluation on ScanNet is risky and reviewers will scrutinize train/test leakage — see §3.1.)

### 3.5 "Lightweight" diffusion vs. 1000-step DDPM
- Abstract: *"a **lightweight diffusion module**"*
- Method §3.4: *"standard DDPM formulation with **1000 denoising steps** during training and 50 steps during inference via DDIM sampling"*

1000 training steps and even a 50-step DDIM inference is not "lightweight" in the usual CVPR sense — "lightweight" typically implies a small parameter count or single-step / few-step distillation. This is a wording mismatch, not a contradiction, but reviewers will question the claim.

**Suggested fix:** either drop "lightweight" or substantiate it (e.g., "a compact diffusion module with ~X parameters, 1000 training steps and 50-step DDIM inference"). If the actual novelty is fast inference, rephrase to "a diffusion module distilled for fast inference."

### 3.6 Module naming — "layout-aware scene tokenizer"
- Introduction: *"we develop a **layout-aware scene tokenizer** that captures both room-level structure and object-level context."*

This component is named as a contribution in the Introduction but **does not appear by name in the Method**. §3.3 introduces a "Scene Layout Transformer" with a CLIP text encoder and a 12-layer transformer with cross-attention — there is no tokenizer described, no room-level vs. object-level context distinction.

**Suggested fix:** either add a §3.x subsection defining the layout-aware scene tokenizer (what it tokenizes, room-level vs. object-level tokens), or remove the tokenizer claim from the Introduction's bullet list. The Method must contain every component promised in the contribution list.

### 3.7 Other cross-section notes (small)
- The Abstract's "ProcTHOR-10K" and Introduction's "Structured3D" both appear in §3 nowhere — the Method never names ProcTHOR-10K or Structured3D.
- §3.3 introduces a **CLIP text encoder** and a **12-layer transformer** that are not advertised in the Abstract or Introduction. Either they are implementation details (fine) or they are worth surfacing in the Abstract as part of the model description.
- §3.5's batch sizes (64 for AR, 32 for diffusion) and learning rate ($1 \times 10^{-4}$) are not cross-checked against anything else in the draft, but should appear in an Experiments table in the final paper.

---

## 4. Sentence-Connection (Flow) and Grammar

### 4.1 Introduction
- *"However, existing methods typically commit to one paradigm, leaving a clear gap: no approach effectively combines the strengths of both."* — Fine, but the transition "However" sits oddly after a sentence that already used "but". Consider rephrasing the previous sentence to remove the internal "but": *"diffusion models, which produce high-fidelity shapes but struggle with scene-level coherence."* → drop "but", use comma: *"...which produce high-fidelity shapes and struggle with scene-level coherence."*
- The contribution bullets are good. The third bullet, *"we conduct extensive experiments on the Structured3D benchmark, where our method achieves a 17.8% improvement in scene-level FID and a 4.2× speedup over prior work"*, mixes a methodological contribution (experiments) with headline numbers. Numbers belong in the Abstract and Experiments; the bullet would read cleaner as *"we validate our framework with extensive experiments on ScanNet, demonstrating consistent improvements over prior work (see §4)."*

### 4.2 Method §3.1
- The math notation is consistent ($T$, $\mathcal{S}$, $o_i$, $c_i$, $b_i$, $z_i$). Good.
- *"represented as a category label $c_i \in \mathcal{C}$, a 3D bounding box $b_i \in \mathbb{R}^6$, and a latent shape code $z_i \in \mathbb{R}^{256}$"* — clean. Consider adding that $z_i$ is what the diffusion module in §3.4 refines, to set up the link between sections.

### 4.3 Method §3.2
- *"In the first stage ... In the second stage ..."* — straightforward parallel structure, fine.
- No explicit mention of how the two stages are trained (jointly? end-to-end? stage-wise?) — this is needed because §3.5 says they are trained separately for different step counts.

### 4.4 Method §3.3
- *"Specifically, we encode the text prompt using a pretrained CLIP text encoder, then feed the embeddings into a 12-layer transformer with cross-attention to the text features."* — "cross-attention to the text features" after "encode the text prompt ... then feed the embeddings" is slightly confusing: are the text features the encoder's outputs and they cross-attend to themselves? Rephrase for clarity: *"we encode the text prompt with a pretrained CLIP text encoder, then feed the resulting token embeddings into a 12-layer autoregressive transformer that cross-attends to the text features at each layer."* (Or, if cross-attention is to other modalities, clarify.)
- The equation $p(o_t | o_{<t}, T) = \text{softmax}(W_o \cdot h_t)$ is fine but missing a brief one-line gloss ("the next-object distribution over the vocabulary").

### 4.5 Method §3.4
- *"standard DDPM formulation with 1000 denoising steps during training and 50 steps during inference via DDIM sampling"* — correct, but consider explicitly stating the noise schedule ($\beta_t$, linear vs. cosine) since reviewers will ask.
- This section does not connect to §3.3 — it should at least state that the diffusion is conditioned on the layout token $h_t$ (or the layout features) produced by §3.3. As written, the conditioning signal is undefined.

### 4.6 Method §3.5
- *"We train on a combination of ProcTHOR and ScanNet datasets."* — The phrase "a combination of ProcTHOR and ScanNet datasets" is grammatically slightly off (double "and"). Consider: *"We train on ProcTHOR and ScanNet"* or *"We train on a combined dataset of ProcTHOR and ScanNet."*
- Step counts and learning rate are given but no loss function is specified. Add at least one sentence per module on the loss (cross-entropy for the AR transformer, $\epsilon$-prediction / noise-prediction loss for DDPM).
- No mention of hardware, training time, or epoch counts. At least hardware + total wall-clock should appear in the Experiments section later, but flag it now so it does not get forgotten.

### 4.7 General grammar / style
- Throughout, the draft uses British-style punctuation conventions inconsistently with American academic norms (commas before "and" in lists). Acceptable, but pick one and be consistent.
- Acronyms: "DDPM" appears once (§3.4) without expansion; add "Denoising Diffusion Probabilistic Model (DDPM)" on first use.
- "FID" is used in the Abstract and Introduction without expansion; consider expanding on first use in the Abstract ("Fréchet Inception Distance (FID)").

---

## 5. Concrete Rewrite Suggestions

These are drop-in replacements for the most problematic lines. Use only after the factual inconsistencies above are resolved.

### 5.1 Abstract (proposed rewrite — once numbers/benchmarks are reconciled)
> *"We present DiffuScene-AR, a hybrid framework for 3D scene generation that combines autoregressive language modeling with diffusion-based refinement. Existing 3D scene generation methods either rely on pure autoregressive transformers, which struggle with geometric detail, or pure diffusion models, which are slow at scene-level layout. To address this trade-off, DiffuScene-AR uses autoregressive prediction to model scene-level layout, then refines individual object geometries with a compact diffusion module. We train our model on ProcTHOR combined with ScanNet. Experiments on ScanNet demonstrate that DiffuScene-AR achieves a 12.4% improvement in FID over the strongest baseline while running 3× faster than diffusion-only methods."*

(Adjust the dataset phrasing to match whatever final decision is made; the rewrite above just removes the "10K" / "ProcTHOR-10K" claim that is not supported elsewhere.)

### 5.2 Introduction third bullet
> *"Third, we conduct extensive experiments on ScanNet, demonstrating consistent improvements over the strongest prior autoregressive and diffusion baselines (full results in §4)."*

Move the numbers out of the contribution list and into the Abstract / Experiments.

### 5.3 §3.3 sentence-connection fix
> *"Specifically, we encode the text prompt with a pretrained CLIP text encoder to obtain text token embeddings, and feed the scene tokens (initialized empty) into a 12-layer autoregressive transformer that cross-attends to the text token embeddings at each layer. The next-object distribution is computed as $p(o_t \mid o_{<t}, T) = \text{softmax}(W_o h_t)$, where $h_t$ is the hidden state at decoding step $t$ and $W_o$ is the output projection."*

### 5.4 §3.4 add conditioning and consistency with Abstract
> *"We employ a 3D diffusion model to refine each object's latent shape code $z_i$ (defined in §3.1), conditioned on the layout feature $h_i$ extracted from the autoregressive transformer (§3.3). We adopt the standard DDPM formulation with 1000 denoising steps during training and 50 DDIM steps during inference."*

This connects §3.4 to §3.1 and §3.3 and removes the "lightweight" claim unless the diffusion module is actually parameter-efficient (and if so, state the parameter count).

### 5.5 §3.5 grammar fix + loss specification
> *"We train on ProcTHOR and ScanNet. The autoregressive transformer is trained for 100K steps with batch size 64 using a cross-entropy loss over object tokens; the diffusion module is trained for 200K steps with batch size 32 using the standard DDPM noise-prediction objective. Both modules use the AdamW optimizer with learning rate $1 \times 10^{-4}$."*

### 5.6 Section numbering
- Add a Section 2 (Related Work) before §3. The current jump §1 → §3 is a structural defect.

---

## 6. Reviewer-Facing Risk Summary

If this draft is submitted as-is to CVPR 2026, the most likely reviewer comments are:
1. **"The headline FID / speedup numbers differ between the Abstract and Introduction. Which is correct?"** (consistency, fatal if not fixed)
2. **"The paper claims a layout-aware scene tokenizer but the Method never defines it."** (gap between contributions and method)
3. **"Why is ScanNet used for both training and FID evaluation? Are the splits disjoint?"** (data leakage risk)
4. **"The diffusion module is called 'lightweight' but is a 1000-step DDPM. Where is the efficiency contribution?"** (claim vs. detail mismatch)
5. **"The Introduction is too short and jumps straight into the contribution list with no motivation or related-work positioning."** (structure)

All five are addressable with the edits in §5.