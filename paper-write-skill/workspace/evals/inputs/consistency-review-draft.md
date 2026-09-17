# Fake Paper Draft — DiffuScene-AR

This is a synthetic paper draft used as input for the consistency-review eval. It has intentional inconsistencies across sections that a good review should catch.

---

## Abstract

> We present DiffuScene-AR, a novel hybrid framework for 3D scene generation that combines autoregressive language modeling with diffusion-based refinement. Existing 3D scene generation methods either rely on pure autoregressive transformers, which struggle with geometric detail, or pure diffusion models, which are slow at scene-level layout. To address this trade-off, DiffuScene-AR uses autoregressive prediction to model scene-level layout, then refines individual object geometries with a lightweight diffusion module. We train our model on ProcTHOR-10K, a curated subset of ProcTHOR containing 10K procedurally generated indoor scenes. Experiments on the ScanNet benchmark demonstrate that DiffuScene-AR achieves a 12.4% improvement in FID over the strongest baseline and runs 3× faster than diffusion-only methods.

---

## 1. Introduction

Recent advances in generative models have enabled remarkable progress in 3D content creation. Within this context, two dominant paradigms have emerged: autoregressive transformers, which excel at compositional layout but produce blurry object geometries, and diffusion models, which produce high-fidelity shapes but struggle with scene-level coherence. However, existing methods typically commit to one paradigm, leaving a clear gap: no approach effectively combines the strengths of both.

In this paper, we propose DiffuScene-AR, a hybrid framework that leverages autoregressive modeling for scene layout and diffusion for object-level refinement. Our contributions are threefold:

- **First**, we introduce a novel hybrid paradigm that unifies autoregressive scene layout prediction with diffusion-based shape refinement in a single framework.
- **Second**, we develop a layout-aware scene tokenizer that captures both room-level structure and object-level context.
- **Third**, we conduct extensive experiments on the Structured3D benchmark, where our method achieves a 17.8% improvement in scene-level FID and a 4.2× speedup over prior work.

---

## 3. Method

### 3.1 Problem Formulation

Given a text prompt $T$ describing a desired indoor scene, our goal is to generate a 3D scene $\mathcal{S} = \{o_1, o_2, \ldots, o_n\}$ consisting of $n$ object instances with their categories, positions, and shapes. Each object $o_i$ is represented as a category label $c_i \in \mathcal{C}$, a 3D bounding box $b_i \in \mathbb{R}^6$, and a latent shape code $z_i \in \mathbb{R}^{256}$.

### 3.2 Overall Framework

DiffuScene-AR consists of two stages. In the first stage, an autoregressive transformer predicts the scene layout by generating a sequence of object tokens. In the second stage, a diffusion model refines each object's shape code conditioned on the predicted layout.

### 3.3 Scene Layout Transformer

The first stage aims to predict the scene layout autoregressively. Specifically, we encode the text prompt using a pretrained CLIP text encoder, then feed the embeddings into a 12-layer transformer with cross-attention to the text features. We compute the next-object distribution as:

$$p(o_t | o_{<t}, T) = \text{softmax}(W_o \cdot h_t)$$

where $h_t$ is the hidden state at step $t$ and $W_o$ is the output projection.

### 3.4 Object Shape Diffusion

We employ a 3D diffusion model to refine each object's geometry. We adopt the standard DDPM formulation with 1000 denoising steps during training and 50 steps during inference via DDIM sampling.

### 3.5 Training Setup

We train on a combination of ProcTHOR and ScanNet datasets. The autoregressive transformer is trained for 100K steps with batch size 64, and the diffusion module is trained for 200K steps with batch size 32. We use the AdamW optimizer with learning rate $1 \times 10^{-4}$.