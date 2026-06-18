# LeWorldModel (MILA / NYU / Samsung SAIL)

---

## What it is

The first JEPA that trains stably end-to-end from raw pixels without heuristics. No EMA, no stop-gradient, no frozen pre-trained encoder. Uses only 2 loss terms: a next-embedding prediction loss and SIGReg (Stable Isotropic Gaussian Regularization).

**~15M parameters.** Trainable on a **single GPU** in a **few hours.** Plans **48x faster** than DINO-WM.

This is the most accessible world model to reproduce the full pipeline: encode observations, predict dynamics, and plan actions, all from pixels.

---

## Key concepts

- **End-to-end JEPA:** encoder + predictor trained jointly from raw pixels (no frozen backbone)
- **SIGReg:** regularizer that forces Gaussian-distributed latents, preventing representation collapse with a single hyperparameter
- **Cross-Entropy Method (CEM):** at test time, optimizes action sequences by rolling out candidates through the predictor and picking those whose final embedding is closest to the goal
- **Goal-image planning:** given a start image and a goal image, the model plans how to get from one to the other in latent space

---

## Hardware requirements

- **Single GPU** (any modern NVIDIA GPU with 8+ GB VRAM)
- Trains in a **few hours** (not days)
- Python 3.10 recommended
- Uses [uv](https://docs.astral.sh/uv/) for environment management

---

## Installation

```bash
git clone https://github.com/lucas-maes/le-wm.git
cd le-wm

uv venv --python=3.10
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

uv pip install stable-worldmodel[train,env]
```

Dependencies managed by:
- [stable-worldmodel](https://github.com/galilai-group/stable-worldmodel), environment management, planning, evaluation
- [stable-pretraining](https://github.com/galilai-group/stable-pretraining), training loop

---

## Training

Configure WandB settings in `config/train/lewm.yaml` (entity and project name), then:

```bash
python train.py data=pusht
```

Available environments:
| Environment | Description |
|---|---|
| `pusht` | 2D pushing task, push a T-shaped block to a target |
| `ogbench-cube` | 3D cube manipulation |
| `two-room` | Navigation between two rooms |
| `reacher` | Reach a target position |

Training uses [Hydra](https://hydra.cc/) for configuration. The core model architecture is in `jepa.py`.

---

## Planning (after training)

```python
import swm

cost_model = swm.policy.AutoCostModel('pusht/lewm')
# Plans from a start image to a goal image using CEM
# Outputs: optimized action sequence
```

The planning loop:
1. Encode start and goal images into latent space
2. Use CEM to propose candidate action sequences
3. Roll out each candidate through the predictor
4. Select the sequence whose final predicted embedding is closest to the goal embedding
5. Execute the first action, re-plan from new observation

Planning completes in **~1 second** (vs 47 seconds for DINO-WM).

---

## Key results

| Environment | LeWM | DINO-WM | PLDM |
|---|---|---|---|
| Push-T | **96%** | 74% (92% w/ proprioception) | 78% |
| Reacher | **86%** | 79% | 78% |
| OGBench-Cube | 74% | **86%** | 65% |
| Two-Room | 87% | **100%** | 97% |

- **48x faster planning** than DINO-WM (0.98s vs 47s)
- **~200x fewer tokens** in latent representation
- **1 hyperparameter** to tune (vs 6 for alternatives)
- Physics probing shows the latent space encodes physical structure (surprise to physical violations > visual violations)

---

## Videos and demos

| What | URL |
|---|---|
| Planning demos (Push-T, 3D environments) | https://le-wm.github.io/ |
| Surprise detection (physics violation) | https://le-wm.github.io/ (scroll to "Surprise Evaluation") |
| Paper | https://arxiv.org/abs/2603.19312 |

---

## Screenshots

LeWorldModel results in the Push-T environment: success (the T block reaches the green target) and a failure:

![LeWorldModel, Push-T success and failure](figures/leworldmodel-pusht.png)

---

## Limitations

- Environments are **simulated** (2D/3D control tasks), not real robots
- No real-time deployment on physical hardware demonstrated yet
- Focused on **goal-image planning** (CEM), not language-based instructions
- Smaller model capacity (~15M) means it may not scale to very complex real-world scenes without modifications

---

## Why this matters for students

LeWorldModel is arguably the **best entry point** for understanding world models practically:
- Small enough to train on a laptop GPU
- Fast enough to iterate experiments in hours, not days
- Complete pipeline (train, predict, plan) in one codebase
- Clean implementation: `jepa.py` is the entire model architecture
- Demonstrates every key concept: latent prediction, anti-collapse, planning via optimization

---

## Links

- GitHub: https://github.com/lucas-maes/le-wm
- Project page (with videos): https://le-wm.github.io/
- Paper: https://arxiv.org/abs/2603.19312
- HuggingFace Papers: https://huggingface.co/papers/2603.19312
- stable-worldmodel: https://github.com/galilai-group/stable-worldmodel
- stable-pretraining: https://github.com/galilai-group/stable-pretraining

---

## Related pages

- **Theory:** [Articles - World Models](../articles.md) (LeWorldModel section)
- **Benchmarks:** [Experiments](../experiments.md) (section 3)
- **Computational cost:** [Costs](../costs.md) (48x faster, 1 hyperparameter, single GPU)
- **Problem it solves:** representation collapse in JEPAs, the same problem discussed in [Best Practices](../best-practices.md) section 5.2
