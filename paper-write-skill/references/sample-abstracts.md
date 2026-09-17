# Sample Abstracts (内置参考)

下面 5 篇 abstract 覆盖 4-component 公式的标准范例 + 一篇改写范例。skill 在 Step 2 / Step 3 直接复用这里的句式模板。

---

## Abstract 1: SAR3D (背景→不足→方法→实验)

> Autoregressive models have demonstrated remarkable success across various fields, from large language models (LLMs) to large multimodal models (LMMs) and 2D content generation, moving closer to artificial general intelligence (AGI). Despite these advances, applying autoregressive approaches to 3D object generation and understanding remains largely unexplored. This paper introduces Scale AutoRegressive 3D (SAR3D), a novel framework that leverages a multi-scale 3D vector-quantized variational autoencoder (VQVAE) to tokenize 3D objects for efficient autoregressive generation and detailed understanding. By predicting the next scale in a multi-scale latent representation instead of the next single token, SAR3D reduces generation time significantly, achieving fast 3D object generation in just 0.82 seconds on an A6000 GPU. Additionally, given the tokens enriched with hierarchical 3D-aware information, we finetune a pretrained LLM on them, enabling multimodal comprehension of 3D content. Our experiments show that SAR3D surpasses current 3D generation methods in both speed and quality and allows LLMs to interpret and caption 3D models comprehensively.

**4-component 拆解**：

| # | 组件 | 内容 |
|---|---|---|
| 1 | 背景 | Autoregressive models 在 LLM / LMM / 2D 生成的成功 |
| 2 | 不足 | autoregressive 在 3D 生成上未充分探索 |
| 3 | 方法 | SAR3D：multi-scale 3D VQVAE + next-scale prediction |
| 4 | 实验 | 0.82 秒生成 + 超过 SOTA + LLM 能 caption |

**句式类型标记**：

- 导入句：`Autoregressive models have demonstrated X across Y.`
- 过渡句：`Despite these advances, applying X to Y remains Y.`
- 提出方法句：`This paper introduces Z, a novel framework that ...`
- 机制句：`By predicting X instead of Y, Z reduces ...`
- 扩展句：`Additionally, given X, we ...`
- 结果句：`Our experiments show that Z surpasses ... in both X and Y.`

---

## Abstract 2: SLAT (结构最完整范例)

> We introduce a novel 3D generation method for versatile and high-quality 3D asset creation. The cornerstone is a unified Structured LATent (SLAT) representation which allows decoding to different output formats, such as Radiance Fields, 3D Gaussians, and meshes. This is achieved by integrating a sparsely-populated 3D grid with dense multiview visual features extracted from a powerful vision foundation model, comprehensively capturing both structural (geometry) and textural (appearance) information while maintaining flexibility during decoding. We employ rectified flow transformers tailored for SLAT as our 3D generation models and train models with up to 2 billion parameters on a large 3D asset dataset of 500K diverse objects. Our model generates high-quality results with text or image conditions, significantly surpassing existing methods, including recent ones at similar scales. We showcase flexible output format selection and local 3D editing capabilities which were not offered by previous models.

**4-component 拆解**：

| # | 组件 | 内容 |
|---|---|---|
| 1 | 背景 | 直接进入方法（隐含了"3D asset creation 需要 flexible output"） |
| 2 | 不足 | 隐含——之前不能同时支持多种输出格式 |
| 3 | 方法 | SLAT 表示 + rectified flow transformers + 2B 参数 + 500K 数据 |
| 4 | 实验 | text/image 条件生成 + 超过 SOTA + 灵活输出 + local editing |

**特点**：背景 + 不足被压缩到一个长句里，方法 + 实验展开充分。适合写"已经熟悉领域、直接亮方法"的 abstract。

---

## Abstract 3: TAR3D (范式迁移型)

> We present TAR3D, a novel framework that consists of a 3D-aware Vector Quantized-Variational AutoEncoder (VQ-VAE) and a Generative Pre-trained Transformer (GPT) to generate high-quality 3D assets. The core insight of this work is to migrate the multimodal unification and promising learning capabilities of the next-token prediction paradigm to conditional 3D object generation. To achieve this, the 3D VQ-VAE first encodes a wide range of 3D shapes into a compact triplane latent space and utilizes a set of discrete representations from a trainable codebook to reconstruct fine-grained geometries under the supervision of query point occupancy. Then, the 3D GPT, equipped with a custom triplane position embedding called TriPE, predicts the codebook index sequence with prefilling prompt tokens in an autoregressive manner so that the composition of 3D geometries can be modeled part by part. Extensive experiments on several public 3D datasets demonstrate that TAR3D can achieve superior generation quality over existing methods in text-to-3D and image-to-3D tasks.

**4-component 拆解**：

| # | 组件 | 内容 |
|---|---|---|
| 1 | 背景 | 隐含——"next-token prediction 在 LLM 上的成功" |
| 2 | 不足 | 隐含——"3D 缺少这种 paradigm" |
| 3 | 方法 | 把 next-token prediction 迁移到 3D：VQ-VAE + GPT + TriPE |
| 4 | 实验 | 多个 public dataset 上 text-to-3D / image-to-3D SOTA |

**特点**：把"把 XX 领域的成功范式搬到 YY 领域"作为核心 insight。适合"我做的工作是借鉴 / 迁移"的 abstract。

---

## Abstract 4: MAR-3D (挑战-方案型)

> Recent advances in auto-regressive transformers have revolutionized generative modeling across different domains, from language processing to visual generation, demonstrating remarkable capabilities. However, applying these advances to 3D generation presents three key challenges: the unordered nature of 3D data conflicts with sequential next token prediction paradigm, conventional vector quantization approaches incur substantial compression loss when applied to 3D meshes, and the lack of efficient scaling strategies for higher resolution latent prediction. To address these challenges, we introduce MAR-3D, which integrates a pyramid variational autoencoder with a cascaded masked auto-regressive transformer (Cascaded MAR) for progressive latent upscaling in the continuous space. Our architecture employs random masking during training and autoregressive denoising in random order during inference, naturally accommodating the unordered property of 3D latent tokens. Additionally, we propose a cascaded training strategy with condition augmentation that enables efficiently up-scale the latent token resolution with fast convergence. Extensive experiments demonstrate that MAR-3D not only achieves superior performance and generalization capabilities compared to existing methods but also exhibits enhanced scaling capabilities compared to joint distribution modeling approaches (e.g., diffusion transformers).

**4-component 拆解**：

| # | 组件 | 内容 |
|---|---|---|
| 1 | 背景 | AR transformer 在多领域成功 |
| 2 | 不足 | 3 个明确 challenge（有序 vs 无序 / 压缩损失 / 缺乏 scaling） |
| 3 | 方法 | MAR-3D：pyramid VAE + cascaded MAR + 随机 masking + 条件增强训练 |
| 4 | 实验 | 优于 SOTA + 优于 diffusion（joint distribution modeling） |

**特点**：用 "However, X presents N key challenges" 一次性列 3 个 limitation，然后 "To address these" 一次性回应。适合方法要解决**多个明确问题**的 abstract。

---

## Abstract 5: VAR-3D (改写范例——从 SAR3D 派生)

> Recent advances in auto-regressive transformers have achieved remarkable success in generative modeling. However, text-to-3D generation remains challenging, primarily due to bottlenecks in learning discrete 3D representations. Specifically, existing approaches often suffer from information loss during encoding, causing representational distortion before the quantization process. This effect is further amplified by vector quantization, ultimately degrading the geometric coherence of text-conditioned 3D shapes. Moreover, the conventional two-stage training paradigm induces an objective mismatch between reconstruction and text-conditioned auto-regressive generation. To address these issues, we propose View-aware Auto Regressive 3D (VAR-3D), which integrates a view-aware 3D Vector Quantized-Variational AutoEncoder (VQ-VAE) to convert the complex geometric structure of 3D models into discrete tokens. Additionally, we introduce a rendering-supervised training strategy that couples discrete token prediction with visual reconstruction, encouraging the generative process to better preserve visual fidelity and structural consistency relative to the input text. Experiments demonstrate that VAR-3D significantly outperforms existing methods in both generation quality and text-3D alignment.

**4-component 拆解**：

| # | 组件 | 内容 |
|---|---|---|
| 1 | 背景 | AR transformer 在生成上成功 |
| 2 | 不足 | 3 个 limitation（信息损失 / 几何一致性差 / 两阶段目标 mismatch） |
| 3 | 方法 | VAR-3D：view-aware VQ-VAE + rendering-supervised 训练 |
| 4 | 实验 | 优于 SOTA in generation quality + text-3D alignment |

**改写要点（vs SAR3D）**：

- 同样的背景范式（AR success → 3D 挑战）
- 不同的具体 limitation（information loss / geometric coherence / objective mismatch）
- 不同的方法名（VAR-3D vs SAR3D）
- 不同的训练策略（rendering-supervised vs multi-scale next-scale）
- 同样的结果句式（"surpasses / outperforms in both X and Y"）

**可复用模板**：

```
Recent advances in X have achieved remarkable success in Y.
However, Z remains challenging, primarily due to bottlenecks in W.
Specifically, existing approaches often suffer from A, causing B
before C. This effect is further amplified by D, ultimately
degrading E. Moreover, the conventional F paradigm induces G.
To address these issues, we propose M, which integrates ...
Additionally, we introduce N. Experiments demonstrate that M
significantly outperforms existing methods in both X and Y.
```

---

## 总结：4-component 公式

```
[背景] 已有方法在 X 领域成功
[不足] Despite/However, 在 Y 子问题上还差 / 没解决
[方法] This paper introduces M, which ...
[实验] Experiments show M surpasses ... in both X and Y.
```

中文口诀：**背景 + 你的卖点 + 实现细节（粗糙）+ 实验结果**。

"实现细节" 不要写公式、不要列公式编号——abstract 层面只要 1-2 个关键模块名 + 一个量化加速 / 提升数字即可。