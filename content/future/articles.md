# Articles - World Models & Agentic AI

> Team: *Intelligent Agents in the Future* (Athus Vasconcellos, José Janio de Castro Junior, Aylla de Melo Lino).
> Source PDFs live in `future/pdfs/`.

---

### A Path Towards Autonomous Machine Intelligence

[LeCun · A Path Towards Autonomous Machine Intelligence](pdfs/lecun-autonomous-machine-intelligence.pdf)

Yann LeCun (NYU / Meta-FAIR). Position paper, OpenReview, 2022 (v0.9.2). The conceptual backbone of the whole "world models" program.

- **Cognitive architecture:** proposes a modular, fully differentiable agent made of six modules, **configurator, perception, world model, cost (intrinsic + critic), short-term memory, and actor**, where a scalar "energy" cost can be back-propagated through the world model and actor for planning.
- **World model as centerpiece:** a single, dynamically configurable predictive engine that estimates missing world state and predicts plausible futures, sharing knowledge across tasks (reasoning by analogy) rather than one model per situation.
- **JEPA (Joint-Embedding Predictive Architecture):** the paper's signature idea, predict the *representation* of the future in an abstract latent space instead of reconstructing raw pixels, letting the encoder discard unpredictable detail.
- **Non-contrastive self-supervision:** argues against contrastive methods (curse of dimensionality) in favor of regularized methods like **VICReg** and **Barlow Twins**.
- **Two reasoning modes:** Mode-1 (reactive, Kahneman "System 1") vs Mode-2 (deliberate planning via the world model = model-predictive control, "System 2"), with hierarchical planning (**H-JEPA**) that *learns* its own intermediate sub-goal vocabulary.
- **Thesis:** "scaling is not enough" and "reward is not enough", human-like learning efficiency comes from a predictive world model trained by self-supervision, not from bigger LLMs or pure RL.
- **Note:** a vision/position paper, no experiments or numbers of its own.

---

### Beyond Language Modeling: An Exploration of Multimodal Pretraining

[Tong et al. · Beyond Language Modeling](pdfs/tong-beyond-language-modeling.pdf)

Tong, Fan, Nguyen et al. (Meta-FAIR / NYU; advisors include LeCun, Saining Xie). arXiv:2603.03276, Mar 2026.

- **Core thesis:** text is a "lossy compression of reality" and high-quality text data is running out, so vision should be treated as a *first-class* pretraining signal, not bolted on afterward.
- **Method:** a single decoder-only Transformer trained from scratch under the **Transfusion** framework, next-token prediction for language + flow-matching/diffusion for vision.
- **Representation Autoencoder (RAE):** a frozen semantic encoder (**SigLIP 2**) gives one unified visual representation that beats VAEs (SD-VAE, FLUX.1) at *both* understanding and generation, refuting the "you need a VAE for generation" assumption.
- **Data synergy:** vision and language are complementary with minimal interference; video data even slightly *improves* language modeling.
- **Emergent world modeling:** unified pretraining enables a Navigation World Model with **no architectural changes**: actions are encoded directly as text tokens, supporting zero-shot natural-language control.
- **Scaling asymmetry:** IsoFLOP scaling laws show **vision is far more data-hungry than language** (up to 51× more data demanded at 1T params); **Mixture-of-Experts (MoE)** narrows this gap and induces emergent modality specialization.

---

### V-JEPA 2.1: Unlocking Dense Features in Video Self-Supervised Learning

[Mur-Labadia et al. · V-JEPA 2.1](pdfs/murlabadia-v-jepa-2-1.pdf)

Mur-Labadia, Muckley, Bar, Assran, Sinha, Rabbat, LeCun, Ballas, Bardes (Meta-FAIR). arXiv:2603.14482, Mar 2026.

- **Central insight:** high-quality *dense* spatio-temporal features don't emerge when the JEPA prediction loss is applied only to masked regions, extending it to **all tokens** (a Dense Predictive Loss) fixes this while preserving global understanding.
- **Four ingredients:** dense predictive loss, **deep self-supervision** (objective applied at multiple intermediate encoder layers), **multi-modal tokenizers** (unified image + video), and model/data scaling.
- **Scale:** encoder grown from ViT-L (300M) to **ViT-G (2B params)**; training images from 1M (ImageNet) to **142M** (VisionMix163M dataset).
- **Frozen-encoder SOTA:** Ego4D STA **7.71 mAP** (~35% relative gain), EPIC-KITCHENS anticipation **40.8 R@5**, NYUv2 depth **0.307 RMSE** (beats DINOv3 ViT-7B), Something-Something-v2 **77.7%**.
- **Robotics:** **+20%** real-robot grasping over V-JEPA 2; navigation planning **10× faster** than NWM (10.6s vs 103.2s).
- **Model family released:** ViT-g/G (1B/2B) plus distilled ViT-B (80M) and ViT-L (300M); the distilled ViT-L nearly closes the gap to ViT-G.

---

### LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from Pixels

[Maes et al. · LeWorldModel](pdfs/maes-leworldmodel-jepa.pdf)

Maes, Le Lidec, Scieur, LeCun, Balestriero (Mila / NYU / Samsung SAIL / Brown). arXiv:2603.19312, Jun 2026.

- **What it is:** a JEPA world model (encoder → latent, predictor → next latent) that is reward-free, reconstruction-free, and task-agnostic.
- **The problem it solves:** JEPAs are prone to **representation collapse**; prior fixes lean on heuristics (stop-gradient, EMA target encoders, frozen pretrained encoders). LeWM is presented as the **first JEPA to train stably end-to-end from raw pixels without any of those heuristics**.
- **Method:** just two loss terms, a next-embedding MSE prediction loss + **SIGReg** (Sketched-Isotropic-Gaussian Regularizer), an anti-collapse term that forces Gaussian-distributed latents. This cuts tunable loss hyperparameters from **six to one**.
- **Tiny + fast:** ~15M params total (ViT-tiny encoder + small transformer predictor), trainable on a single GPU in a few hours.
- **Results:** **48× faster planning** than DINO-WM (0.98s vs 47s full planning), ~200× fewer tokens; Push-T success **96%** (+18% over PLDM, beats DINO-WM even when DINO-WM gets proprioception).
- **Physics probing:** the latent space recovers physical quantities and shows stronger "surprise" to physical perturbations than to visual ones, evidence it captures dynamics, not just appearance.

---

### Temporal Straightening for Latent Planning

[Wang et al. · Temporal Straightening](pdfs/wang-temporal-straightening.pdf)

Wang, Bounou, Zhou, Balestriero, Rudner, LeCun, Ren (NYU / Brown / Toronto). arXiv:2603.12231, Mar 2026.

- **The idea:** a good visual encoder is not automatically a good *planning* representation, pretrained encoders can produce highly **curved** latent trajectories that hurt planning.
- **Temporal straightening:** a training-time regularizer that reduces the curvature of latent trajectories (maximizing cosine similarity between consecutive latent velocities), so Euclidean distance becomes a faithful proxy for true progress toward a goal.
- **Inspiration:** the *perceptual straightening* hypothesis in human vision (Hénaff et al., 2019), applied for the first time to planning representations.
- **Theory:** lower curvature provably improves the conditioning of the planning Hessian, making the objective closer to convex, so a cheap gradient-descent planner can replace expensive search (CEM/MPPI).
- **Results:** open-loop planning success up **20–60%**, MPC up **20–30%**; e.g. PointMaze-UMaze open-loop **44% → 94%**; reaches 100% MPC success on Wall and UMaze.
- **Limitation:** long-horizon planning still degrades from accumulated prediction error.

---

### Hierarchical Planning with Latent World Models (HWM)

[Zhang et al. · Hierarchical Planning (HWM)](pdfs/zhang-hierarchical-planning-hwm.pdf)

Zhang, Terver, Zholus, Chitnis, Sutaria, Assran, Balestriero, Bar, Bardes, LeCun, Ballas (Meta-FAIR / NYU / Mila / Brown). arXiv:2604.03208, Apr 2026.

- **Problem attacked:** long-horizon planning fails from **compounding prediction errors** and an **exponentially growing search space**; flat single-level planners hit **0% success** on non-greedy Franka pick-and-place.
- **HWM:** a **zero-shot, training-free** hierarchical MPC framework, a high-level long-horizon world model proposes latent "macro-action" sub-goals, and a low-level short-horizon model reaches them, coupled by latent-state matching. No inverse models, skills, or goal-conditioned policies needed.
- **Model-agnostic plug-in:** applied on top of three existing world models, VJEPA2-AC (robot), DINO-WM (push), PLDM (maze).
- **Results:** Franka pick-and-place **0% → 70%**, drawers **30% → 70%**, Push-T **17% → 61%**, unseen mazes **44% → 83%**.
- **Beats bigger baselines:** outperforms VLA models trained on **~77× more** robot interaction data.
- **Efficiency:** matches or beats flat planners with **3–4× less** planning-time compute (a single high-level step replaces up to 16 autoregressive low-level steps).

---

### Towards Pervasive Distributed Agentic Generative AI: A State of the Art

[Molinari & Ciravegna · Pervasive Distributed Agentic GenAI](pdfs/molinari-pervasive-agentic-genai.pdf)

Gianni Molinari & Fabio Ciravegna (University of Turin). arXiv:2506.13324, Dec 2025. *(Survey, not an empirical study.)*

- **Agent anatomy:** frames the standard four-module LLM agent, **profiling, memory, planning, action**, as the unit of analysis.
- **Edge-to-cloud continuum:** maps deployment across cloud (scalable, high-latency/privacy cost), fog (Jetson, Raspberry Pi, gateways), and edge (sensors to phones), plus enabling hardware (NPUs, SoCs) and networks (Wi-Fi 7, LoRaWAN, 5G/6G).
- **On-device efficiency toolkit:** small language models (SLMs), quantization (8-/4-bit), pruning, split inference (EdgeShard), federated learning, KV-cache compression, LLM-as-OS-service.
- **Four open challenges:** quantized-model reasoning loss, memory/context limits, the **energy cost** of the perceive-plan-act loop, and privacy/prompt-injection.
- **Proposed framework, "Agent as a Tool":** per-user personal agents that call classic tools *and* specialized agent-tools as-a-service across the continuum, communicating via **A2A** and **MCP**, with human-approval safeguards.
- **Reality-check data (cited):** SAGE 76% on complex smart-home queries; no agent scored above 60% on Agent-SafetyBench; GPT-4 leaks sensitive info in up to 25.68% of PrivacyLens cases.

---

## Cross-cutting takeaways

- **The recurring frontier:** every world-model paper here (V-JEPA 2.1, LeWorldModel, Temporal Straightening, HWM) hits the same wall, **long-horizon error accumulation** in autoregressive latent rollouts.
- **The shared bet:** predict the *next state of the world* in latent space, not the *next word*, LeCun's 2022 architecture is the common ancestor and he co-authors most of the 2026 papers.
- **Two complementary fixes to long-horizon planning:** improve the *geometry* of the latent space (Temporal Straightening) or impose *structure* on the search (Hierarchical Planning / HWM).
