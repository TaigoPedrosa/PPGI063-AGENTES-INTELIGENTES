# Experiments - World Model Benchmarks

> **Note on scope.** Unlike the *present* team's experiments (which the team ran themselves), the Futuro folder contains no original experiment. This page is a **digest of the experiments and benchmark results reported inside the papers** in `future/pdfs/`. Every number below is quoted from the source paper, with the file named. Nothing here was generated or estimated by us. Where a paper doesn't report something, it is left out.
>
> Same format as "Experiments - Sub-Agents": methodology, metrics tables, qualitative analysis, conclusion.

---

## 1) Hierarchical Planning (HWM): flat vs hierarchical planning

*Source:* [Zhang et al. · Hierarchical Planning (HWM)](pdfs/zhang-hierarchical-planning-hwm.pdf)

**Methodology.** HWM is a training-free hierarchical MPC layer dropped on top of three *existing* latent world models. Each model is evaluated with its native flat (single-level) planner vs. with the HWM two-level hierarchy. Success = reaching the goal from a single goal image.

**Headline results (success rate, flat → hierarchical):**

| Backbone | Task | Flat | Hierarchical | Δ |
| --- | --- | --- | --- | --- |
| VJEPA2-AC | Franka pick-&-place (no oracle subgoals) | 0% | 70% | **+70** |
| VJEPA2-AC | Open/close drawer | 30% | 70% | +40 |
| DINO-WM | Push-T | 17% | 61% | +44 |
| PLDM | Diverse Maze (unseen layouts) | 44% | 83% | +39 |

**Push-T by horizon (DINO-WM backbone):**

| Planner | d=25 | d=50 | d=75 |
| --- | --- | --- | --- |
| DINO-WM (flat) | 84% | 55% | 17% |
| DINO-WM (hierarchy) | **89%** | **78%** | **61%** |

**Diverse Maze by difficulty (PLDM backbone):**

| Planner | easy | medium | hard |
| --- | --- | --- | --- |
| PLDM (flat) | 100% | 63% | 44% |
| PLDM (hierarchy) | 100% | **95%** | **83%** |

**Notable:** HWM outperforms VLA baselines trained on **~77× more** robot interaction data, and uses **3–4× less** planning-time compute than flat planners (one high-level step replaces up to 16 autoregressive low-level steps).

---

## 2) Temporal Straightening: latent-geometry regularization for planning

*Source:* [Wang et al. · Temporal Straightening](pdfs/wang-temporal-straightening.pdf)

**Methodology.** A curvature regularizer is added during world-model training; goal-reaching success is measured with a cheap gradient-descent (GD) planner across 4 environments (Wall, PointMaze-UMaze, PointMaze-Medium, PushT), 50 test samples, 3 seeds. The "Straight?" column marks whether temporal straightening was applied (yes/no).

**Goal-reaching success (GD planner), Open-loop / MPC:**

| Encoder setup | Straight? | Wall | UMaze | Medium | PushT |
| --- | --- | --- | --- | --- | --- |
| DINOv2 patch + proj (14×14×8) | no | 80.0 / 90.7 | 44.0 / 81.3 | 72.0 / 96.7 | 70.0 / 78.7 |
| DINOv2 patch + proj (14×14×8) | yes | **90.7 / 100.0** | **94.0 / 100.0** | 82.7 / 98.7 | 77.3 / 85.3 |
| ResNet from scratch (14×14×8) | no | 1.3 / 6.7 | 14.7 / 66.0 | 18.7 / 57.3 | 71.3 / 70.7 |
| ResNet from scratch (14×14×8) | yes | **84.7 / 100.0** | **64.7 / 98.7** | **80.7 / 99.3** | 70.7 / 91.3 |

**Reported gains:** open-loop success +20–60%, MPC +20–30%; UMaze open-loop rises from 44% → 94% (with projector) and 14.7% → 64.7% (ResNet from scratch). Straightening narrows the gap between cheap GD planning and expensive CEM search.

**Qualitative:** the straightened model's Euclidean-distance map closely matches ground-truth geodesic (A* search) distance, despite being trained only on suboptimal trajectories.

---

## 3) LeWorldModel: stable end-to-end JEPA from pixels

*Source:* [Maes et al. · LeWorldModel](pdfs/maes-leworldmodel-jepa.pdf)

**Methodology.** ~15M-param JEPA trained end-to-end from pixels with a 2-term loss (prediction + SIGReg). Evaluated on Push-T, OGBench-Cube, Two-Room, Reacher against DINO-WM, PLDM, and behavior-cloning/IQL baselines via MPC + CEM.

**Planning success rate (selected):**

| Env | LeWM | DINO-WM | PLDM |
| --- | --- | --- | --- |
| Push-T | **96** | 74 (92 w/ prop) | 78 |
| Reacher | **86** | 79 | 78 |
| OGBench-Cube | 74 | **86** | 65 |
| Two-Room | 87 | **100** | 97 |

- **+18%** success over PLDM on Push-T; LeWM (pixels only) beats DINO-WM *even when DINO-WM is given proprioception*.
- **Speed:** full planning **0.98s vs 47s** for DINO-WM (~48× faster); fixed-FLOPs Push-T success 90 vs 13.
- **Physics test:** violation-of-expectation surprise spikes are significant for *physical* perturbations (object teleport, p < 0.01) but weak for *visual* ones (color change), the latent encodes dynamics, not appearance.

---

## 4) V-JEPA 2.1: dense features via the Dense Predictive Loss

*Source:* [Mur-Labadia et al. · V-JEPA 2.1](pdfs/murlabadia-v-jepa-2-1.pdf)

**Methodology.** Self-supervised video/image encoder (ViT-B 80M → ViT-G 2B) used as a **frozen** backbone across dense, global, and world-modeling tasks. Key ablation: supervise *all* tokens (Dense Predictive Loss) instead of only masked ones.

**Frozen-encoder SOTA highlights (ViT-G):**

| Task | Metric | V-JEPA 2.1 | Prior reference |
| --- | --- | --- | --- |
| Ego4D STA v2 | mAP | **7.71** | 5.67 (STAformer) |
| EPIC-KITCHENS-100 anticip. | Recall@5 (Action) | **40.8** | 39.7 (V-JEPA 2) |
| NYUv2 depth | RMSE ↓ | **0.307** | 0.309 (DINOv3 ViT-7B) |
| Something-Something-v2 | Top-1 | **77.7** | 77.3 (V-JEPA 2) |
| ADE20K seg. | mIoU | 47.9 | 55.9 (DINOv3, still ahead) |

**Ablation, building up the model (IN1K / SSv2 / NYU RMSE / ADE20K mIoU):**

| Step | IN1K | SSv2 | NYU↓ | ADE20K |
| --- | --- | --- | --- | --- |
| V-JEPA 2 baseline | 82.2 | 72.8 | 0.682 | 22.2 |
| + Context (dense) loss | 72.6 | 62.5 | 0.474 | 33.8 |
| + Deep self-supervision | 80.8 | 72.1 | 0.463 | 38.6 |
| + VisionMix data | 81.6 | 72.6 | 0.418 | 40.8 |
| + Multi-modal tokenizer | 81.6 | 72.6 | 0.415 | 41.4 |
| + Model scaling (ViT-G) | 84.8 | 76.1 | 0.365 | 47.1 |
| + Cool-down (final) | **85.5** | **77.7** | **0.307** | **47.9** |

**Robotics:** +20% grasping over V-JEPA 2 (grasp 60% → 80% at horizon 8); navigation planning 10.6s vs 103.2s for NWM (10× faster).

---

## 5) Beyond Language Modeling: controlled multimodal pretraining

*Source:* [Tong et al. · Beyond Language Modeling](pdfs/tong-beyond-language-modeling.pdf)

**Methodology.** Decoder-only Transformer trained *from scratch* (no LLM init, to avoid confounds) under Transfusion; controlled comparison of visual representations, data mixtures, and MoE design. ~1T tokens (520B text + 520B multimodal).

- **Visual representation:** SigLIP 2 (RAE) beats VAEs (SD-VAE, FLUX.1) on VQA, DPGBench, GenEval, VQA after multimodal pretraining 40.3 (SigLIP 2) vs 30.1 (SD-VAE).
- **Data synergy:** 20B VQA + 80B heterogeneous tokens (37.9 avg VQA) beats 100B VQA-only (35.7), better with 5× *less* in-domain data.
- **World modeling (NWM):** pure video gives the biggest planning gain (ATE 1.974 → 1.410 at 50B tokens); capability emerges from general video pretraining, not domain data.
- **Scaling law:** dense vision exponent (data-side) 0.63 vs language 0.53, vision is far more data-hungry; MoE narrows the exponent gap from 0.10 → 0.05.

---

## 6) Cross-paper pattern

| Theme | Evidence across papers |
| --- | --- |
| **Latent planning beats pixel generation for speed** | LeWM 48× faster than DINO-WM; V-JEPA 2.1 10× faster than NWM; HWM 3–4× less compute |
| **Long-horizon is the wall** | Push-T drops 84%→17% with horizon (HWM, flat); every paper flags error accumulation |
| **Structure or geometry fixes long horizons** | HWM (hierarchy): Push-T d=75 17%→61%; Temporal Straightening (geometry): UMaze 44%→94% |
| **Small + simple can win** | LeWM 15M params beats foundation-model baselines on Push-T; distilled V-JEPA 2.1 ViT-L ≈ ViT-G |
| **Data-hungry vision** | Tong et al. scaling laws; MoE as the mitigation |

## Conclusion

The literature converges on one experimental story: **planning in a learned latent space is dramatically cheaper than generating pixels**, and the dominant failure mode is **long-horizon error accumulation**. Two independent fixes both work in the benchmarks, improving the **geometry** of the latent space (Temporal Straightening) and imposing **hierarchical structure** on the search (HWM), and the most striking single result is flat-planner Franka pick-and-place going from **0% → 70%** purely by adding a training-free planning hierarchy.
