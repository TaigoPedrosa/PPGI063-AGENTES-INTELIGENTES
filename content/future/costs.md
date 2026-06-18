# Costs - Compute & Efficiency

> **Note on scope.** The Futuro folder has no billing/consumption data of its own (unlike the *present* team's "Tokens & IA 2026" cost page). So this page reframes "cost" as the **compute, training, and inference efficiency reported inside the papers** in `future/pdfs/`, the economic story of world models. Every figure is quoted from its source paper; nothing is estimated by us.
>
> Same spirit as the present team's "Costs" page: where the money/compute goes, and how each approach repasses cost.

---

## 1) The headline: latent planning is cheap, pixel generation is expensive

The single biggest cost lever in this literature is **where you predict**. Predicting in a compact latent space is orders of magnitude cheaper than generating pixels or video frames.

| System | Full planning time | vs. baseline | Source |
| --- | --- | --- | --- |
| **LeWorldModel** | **0.98 s** | ~**48× faster** than DINO-WM (47 s) | [Maes et al. · LeWorldModel](pdfs/maes-leworldmodel-jepa.pdf) |
| **V-JEPA 2.1** (navigation) | **10.6 s** | **10× faster** than NWM (103.2 s) | [Mur-Labadia et al. · V-JEPA 2.1](pdfs/murlabadia-v-jepa-2-1.pdf) |
| **V-JEPA 2** (per action) | 16 s | vs ~4 min for Cosmos (generative) | [Presentation 2, slide 10](pdfs/presentation-2-jepa-world-models.pdf) |
| **HWM** | n/a | **3–4× less** compute/step than flat planning | [Zhang et al. · Hierarchical Planning (HWM)](pdfs/zhang-hierarchical-planning-hwm.pdf) |

LeWorldModel also encodes **~200× fewer tokens** than DINO-WM, and a high-level HWM step replaces **up to 16** autoregressive low-level steps.

---

## 2) Model size: small models are competitive

| Model | Params | Note |
| --- | --- | --- |
| LeWorldModel | **~15M** (5M encoder + 10M predictor) | trainable on a single GPU in a few hours; beats foundation-model baselines on Push-T |
| V-JEPA 2.1 ViT-B | 80M | distilled |
| V-JEPA 2.1 ViT-L | 300M | distilled; nearly closes the gap to ViT-G |
| V-JEPA 2 | 1.2B | ViT-g encoder + predictor (Presentation 2) |
| V-JEPA 2.1 ViT-G | 2B | flagship |
| Tong et al. (default) | 2.3B total / **1.5B active** | MoE: total capacity decoupled from active compute |

**Takeaway:** LeWorldModel makes the strongest cost argument, a **15M-param** model matching or beating models built on encoders like DINOv2 (trained on ~124M images, ~2 orders of magnitude more data).

---

## 3) Training cost & data scale

- **V-JEPA 2.1:** trained on **VisionMix163M**: LVD-142M (142M images) + YT-1B (~1.6M hours of video) + SSv2 + HowTo100M + Kinetics. Batch 2,304 images / 128 video clips; 135k iterations + 12k cool-down. The multi-modal tokenizer removes the image→16-frame duplication of V-JEPA 2, explicitly to cut compute. *(GPU-hours / total FLOPs: not reported.)*
- **V-JEPA 2 (presentation):** Stage 1 on **1M+ hours** internet video (no actions); Stage 2 fine-tuned on **62 h** of DROID robot data (Franka Panda).
- **HWM:** world models trained on **~130 h** of unlabeled real-robot data (DROID + RoboSet). Outperforms VLA baselines pretrained on **~77× more** robot data, i.e. far better return on data.
- **Tong et al.:** ~1T tokens (520B text + 520B multimodal); IsoFLOP budgets ~6×10¹⁸ → 10²¹ FLOPs. Key cost finding: **vision is far more data-hungry than language** (demands 14× more data at 100B params, 51× more at 1T); **MoE** narrows the data-demand exponent gap from 0.10 → 0.05, i.e. better scaling economics.

---

## 4) Hyperparameter / tuning cost

LeWorldModel reframes a hidden cost, **tuning**:

- Reduces loss hyperparameters from **six → one** (only λ for SIGReg).
- Hyperparameter search in **O(log n)** (bisection) vs PLDM's **O(n⁶)**.
- No stop-gradient, EMA target encoder, or frozen pretrained encoder needed.

This is a real operational saving: fewer knobs, cheaper search, more stable training.

---

## 5) The deployment-cost frontier: agents on the edge

*Source:* [Molinari & Ciravegna · Pervasive Distributed Agentic GenAI](pdfs/molinari-pervasive-agentic-genai.pdf) (survey). When agents leave the data center, cost becomes **energy, memory, and latency** on constrained devices.

**Energy:**
- ~**0.1 J/token per billion params** → a 7B model ≈ 0.7 J/token.
- A fully charged iPhone (~50 kJ) sustains a 7B model in conversation for **< 2 hours** at 10 tok/s; every 64 tokens drains ~0.2% battery.

**Memory footprint (Llama-2 7B):**

| Precision | Weights | + KV cache (4k ctx) |
| --- | --- | --- |
| FP (full) | ~28 GB | +2 GB |
| 8-bit | ~7 GB | +2 GB |
| 4-bit | ~3.5 GB | +2 GB |

Typical phones have 6–12 GB RAM → a real obstacle.

**Cost-reduction toolkit (cited):** small language models (SLMs), quantization (8-/4-bit), pruning, split inference (EdgeShard), federated learning, KV-cache compression/swapping, dynamic early-exit (GREEN-CODE), and event-triggered planning (low-power sensors gate the expensive LLM).

**Cloud vs edge trade-off:** cloud = scalable but high latency, privacy, and bandwidth cost for streaming sensor/multimodal data; edge = low latency/bandwidth but adds device/network management and local-security cost.

---

## 6) Comparison table: cost dimension by approach

| Dimension | Winner | Why |
| --- | --- | --- |
| **Inference / planning speed** | LeWorldModel | 48× faster than DINO-WM; latent rollouts, ~200× fewer tokens |
| **Training simplicity** | LeWorldModel | 1 loss hyperparameter, single GPU, no heuristics |
| **Data efficiency (robotics)** | HWM | beats baselines trained on 77× more data |
| **Planning-compute per step** | HWM | 3–4× less than flat planners |
| **Scaling economics (multimodal)** | MoE (Tong et al.) | decouples active compute from capacity; narrows vision–language gap |
| **Deployment energy/memory** | (open problem) | edge devices still bottlenecked, Molinari survey |

## Conclusion

In this literature, "cost" is **compute and energy, not dollars**, and the consistent finding is that **predicting in latent space is the cheapest path**: LeWorldModel plans **48× faster** with a **15M-param** model and a single hyperparameter, V-JEPA 2.1 plans **10× faster** than a generative baseline, and HWM extracts more from **77× less** robot data at **3–4× lower** planning compute. The remaining cost frontier is **deployment**: running these agents continuously on edge hardware is still bounded by **energy** (~0.7 J/token for 7B) and **memory** (tens of GB), which is exactly what the Molinari survey flags as the open challenge for *pervasive* agentic AI.

> See `Experiments` for the accuracy side of these trade-offs, and `Articles · World Models` for per-paper context.
