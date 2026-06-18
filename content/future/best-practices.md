# Best Practices - Future Agents & World Models

> Overview / guideline page derived from the team's two presentations:
> [Presentation 1 · Intelligent Agents in the Future](pdfs/presentation-1-agents-in-the-future.pdf) (37 slides) and
> [Presentation 2 · JEPA & World Models](pdfs/presentation-2-jepa-world-models.pdf) (26 slides).
> Team: Athus Vasconcellos, José Janio de Castro Junior, Aylla de Melo Lino.
> Same spirit as the *present* team's "Best Practices" pages: a traceable reference distilled from a primary presentation + the papers it cites.

---

## 1) Scope and objective

This page consolidates the narrative of where **intelligent agents are heading**: from today's passive, prompt-driven assistants toward **autonomous, proactive, multi-agent systems that act in the physical world**. It is grounded in the team's two decks and the LeCun-lineage papers they summarize (JEPA → V-JEPA 2 → LeWorldModel → HWM), plus the broader "Physical AI" landscape.

The central argument across both presentations: **LLMs alone are insufficient for agents that act in the physical world**, and the path forward runs through **world models** that predict the next *state of the world* rather than the next *word*.

---

## 2) The big picture: three eras of AI

The decks frame a paradigm transition (Presentation 1, slide 33):

| Era | Period | What it does |
| --- | --- | --- |
| **Generative AI** | 2022–2024 | Creates content (text, image, code) |
| **Agentic AI** | 2024–2025 | Decides, plans, executes tasks |
| **Physical AI** | 2025 → | Perceives, reasons, and **acts in the real world** |

> "LLMs predict the next word; World Models predict the next state of the world.", Presentation 1, slide 23

---

## 3) Levels of autonomy (maturity ladder)

A framework for classifying agents (Presentation 1, slide 7):

1. **Assistive**: support only, no decision autonomy.
2. **Semi-autonomous**: rule/model-based, still needs human supervision.
3. **Autonomous**: independent decisions and actions from data and defined goals.
4. **Full autonomy**: agents define their own strategies and goals, operating continuously and adaptively.

**Practice:** match the level of autonomy to the cost of error. The decks stress that physical action carries an **irreversible cost of error**, so higher autonomy demands stronger governance.

---

## 4) Why LLMs are not enough (and what replaces the gap)

From "Beyond Language Modeling" (Tong et al.) and LeCun's framing:

- **Language is a lossy compression of reality.** A language-only agent can *describe* a falling ball but does not robustly *understand* its physical behavior well enough to act.
- **Vision brings physics, geometry, causality**: position, movement, spatial relations, temporal continuity, object interaction.
- The future is **not "LLMs with vision bolted on"** but **natively multimodal models** trained from the start to integrate language and perception.
- **Vision is more data-hungry than language** → **Mixture-of-Experts (MoE)** is the defended architectural answer (better performance per unit of active compute, with modality specialization).

---

## 5) World models: the core construct

**World Foundation Models (WFM):** models that learn to predict/simulate how the physical world works in 3D, spatial relations, physics, object permanence, causality.

### 5.1 Reference architecture (Presentation 1, slide 24)

```
visual observation (camera/sensors) + proposed action
        │
        ▼
  [ Visual Encoder ]  →  latent representation of world state
        │
        ▼
  [ Dynamics Predictor ]  →  simulate "what happens if I do this action"
        │
        ▼
  [ Physics constraints ]  (gravity, collision, friction)
        │
        ▼
  [ Generative Decoder ]  →  visualization of predicted result
        │
        ▼
  "If I do X, the world will look like this."
```

### 5.2 The JEPA lineage (Presentation 2)

- **JEPA**: predict the *embedding* of the future from context, not raw pixels. Open challenge: representation collapse.
- **V-JEPA 2**: pretrain on 1M+ hours of internet video (intuitive physics), then fine-tune with actions (DROID / Franka robot) → an action-conditioned world model usable for **zero-shot** robot planning via **MPC** with a goal image (65–80% pick-and-place success).
- **LeWorldModel**: solves training instability/collapse with **end-to-end pixel training + SIGReg** (one hyperparameter instead of six); up to 48× faster planning.
- **HWM (Hierarchical Planning)**: attacks long-horizon failure with a **two-scale, training-free** planning hierarchy.

---

## 6) Planning: the hard core

Both decks frame planning as **decision-making under uncertainty in an exponentially large search space** (state-space search; combinatorial explosion), a *structural* difficulty that bigger models alone do not remove.

**Current limitations (today's agents):**
- Short horizon, few steps; loses coherence over long tasks.
- No real memory, forgets, doesn't learn over time.
- Brittle, small environment changes break the plan; errors accumulate.
- Heavy prompt dependence; seeks a *plausible* plan, not the *best* plan.

**Future planning should be (Presentation 1, slide 16):**
- **Adaptive**: continuously updated, not fixed.
- **Hierarchical**: goals, sub-goals, actions across levels.
- **Constraint-aware**: time, cost, risk, rules.
- **With memory**: learns from past experience.
- **Multi-agent**: distributed among specialized agents.

**Two research fixes the team highlights:**
- **Temporal Straightening**: fix the *geometry* of the latent space so cheap gradient planning works.
- **HWM**: impose *structure* (two time scales) so the search space stays tractable.

---

## 7) From digital to physical: four frontiers

From Presentation 1 (slides 21–22):

1. **Foundational world models**: simulate real-world 3D physics; "think with physics, not just text." (NVIDIA Cosmos, Google Genie 3, DreamDojo.)
2. **Embodied agents & generalist robotics**: **VLA (Vision-Language-Action)** models: see, understand language, act in one model. (Gemini Robotics 1.5, NVIDIA GR00T, Tesla Optimus.)
3. **Computer-use & GUI agents**: operate existing software like a human, no APIs needed (critical for legacy/government systems).
4. **Reasoning under uncertainty**: partially observable environments modeled as **POMDPs**; map-less navigation in chaotic/never-seen settings.

---

## 8) Limitations, risks & governance

**Technical (recurring across all world-model papers):**
- **Long-horizon error accumulation**: autoregressive latent rollouts diverge after a few seconds. *The* open problem.
- Single temporal scale (motivates hierarchical H-JEPA / HWM).
- Training instability / representation collapse (motivates LeWorldModel).
- Sensitivity to camera position; visual-only goals (no natural-language instructions yet).

**Ethical & organizational:**
- **Transparency & explainability**: decisions must be auditable and justifiable.
- **Responsibility & regulation**: who answers for an agent's actions?
- **Systemic-failure risk**: large-scale cascading failures from dependence.
- **Irreversible cost of physical error**: a wrong physical action cannot always be undone.

---

## 9) Practitioner checklist

- [ ] Decide whether the task needs a **world model** (physical/embodied, planning under uncertainty) or whether an LLM agent suffices.
- [ ] Plan in **latent space**, not pixel space, when speed matters (see LeWorldModel: 48× faster).
- [ ] For long horizons, prefer **hierarchical** planning or latent-geometry regularization over flat single-level search.
- [ ] Match **autonomy level** to the **reversibility and cost** of the agent's actions; add human-approval gates for critical/physical actions.
- [ ] Treat **long-horizon reliability** as the key risk to test for, measure error accumulation, not just single-step accuracy.
- [ ] Build in **governance** from the start: explainability, auditing, accountability.

---

## 10) Key references (from the decks)

- LeCun, Y. *A Path Towards Autonomous Machine Intelligence.* OpenReview, 2022.
- Tong, S. et al. *Beyond Language Modeling: An Exploration of Multimodal Pretraining.* arXiv:2603.03276, 2026.
- Wang, Y. et al. *Temporal Straightening for Latent Planning.* arXiv:2603.12231, 2026.
- Mur-Labadia, L. et al. *V-JEPA 2.1: Unlocking Dense Features in Video Self-Supervised Learning.* arXiv:2603.14482, 2026.
- Maes, L. et al. *LeWorldModel.* arXiv:2603.19312, 2026.
- Zhang, W. et al. *Hierarchical Planning with Latent World Models.* arXiv:2604.03208, 2026.
- Molinari, G.; Ciravegna, F. *Towards Pervasive Distributed Agentic Generative AI.* arXiv:2506.13324, 2025.
- Murugesan, S. *The Rise of Agentic AI.* IEEE Intelligent Systems, 2025., Acharya et al. *Agentic AI.* IEEE Access, 2025., Hosseini & Seilani. *The Role of Agentic AI in Shaping a Smart Future.* Array, 2025.

> See `Articles · World Models` for per-paper summaries, `Experiments` for the numbers, and `Costs` for the compute story.
