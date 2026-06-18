# V-JEPA 2 / V-JEPA 2.1 (Meta FAIR)

---

## What it is

Self-supervised video encoder that achieves SOTA on motion understanding and enables zero-shot robot planning via action-conditioned prediction (V-JEPA 2-AC).

This is the core model from Yann LeCun's world-model program:
- Pre-trained on **1M+ hours** of internet video (self-supervised, no labels)
- Fine-tuned with only **62 hours** of robot data (DROID dataset)
- Deployed **zero-shot** on Franka Panda arms in entirely new environments

V-JEPA 2.1 extends this with a **Dense Predictive Loss** that unlocks high-quality spatio-temporal features, improving robotics performance by +20% on grasping tasks.

---

## Key concepts

- **Joint-Embedding Predictive Architecture:** predicts representations in latent space, not raw pixels
- **Action-conditioned predictor (V-JEPA 2-AC):** given current state + action, predicts the next latent state
- **Model Predictive Control (MPC):** plans by imagining future states and picking actions that reach a goal image
- **Cross-Entropy Method (CEM):** optimization algorithm for searching the best action sequence

---

## Hardware requirements

- GPU with CUDA support (strongly recommended)
- Models range from ViT-B (80M params) to ViT-H/16 in V-JEPA 2.1 (2B params); ViT-G tops out at 1B
- The demo notebook runs on a single GPU
- For the full ViT-G model at 384 resolution, a high-VRAM GPU (24+ GB) is recommended

---

## Quick start: HuggingFace (simplest)

```bash
pip install -U git+https://github.com/huggingface/transformers
```

```python
from transformers import AutoVideoProcessor, AutoModel

hf_repo = "facebook/vjepa2-vith-fpc64-256"
model = AutoModel.from_pretrained(hf_repo)
processor = AutoVideoProcessor.from_pretrained(hf_repo)

# Process a video and get embeddings
inputs = processor(video, return_tensors="pt")
outputs = model(**inputs)
embeddings = outputs.last_hidden_state
```

HuggingFace model page: https://huggingface.co/facebook/vjepa2-vith-fpc64-256

---

## Quick start: PyTorch Hub

```bash
pip install torch torchvision timm einops
```

```python
import torch

model = torch.hub.load("facebookresearch/vjepa2", "vjepa2_vitl")
```

---

## Quick start: Colab notebook (no local setup needed)

Direct link: [vjepa2_demo.ipynb on Colab](https://colab.research.google.com/github/facebookresearch/vjepa2/blob/main/notebooks/vjepa2_demo.ipynb)

This notebook loads the model, processes a sample video, and runs classification inference. Runs on free Colab GPU.

---

## Full setup (local clone)

```bash
git clone https://github.com/facebookresearch/vjepa2.git
cd vjepa2

# Download a checkpoint
wget https://dl.fbaipublicfiles.com/vjepa2/vitg-384.pt -P checkpoints/

# Run the demo
python -m notebooks.vjepa2_demo
```

---

## Available checkpoints

### V-JEPA 2

| Model | Params | Resolution | Download |
|---|---|---|---|
| ViT-L/16 | 300M | 256 | [checkpoint](https://dl.fbaipublicfiles.com/vjepa2/vitl.pt) |
| ViT-H/16 | 600M | 256 | [checkpoint](https://dl.fbaipublicfiles.com/vjepa2/vith.pt) |
| ViT-g/16 | 1B | 256 | [checkpoint](https://dl.fbaipublicfiles.com/vjepa2/vitg.pt) |
| ViT-g/16 (384) | 1B | 384 | [checkpoint](https://dl.fbaipublicfiles.com/vjepa2/vitg-384.pt) |

### V-JEPA 2.1 (dense features)

| Model | Params | Resolution |
|---|---|---|
| ViT-B/16 | 80M | 384 |
| ViT-G/16 | 1B | 384 |
| ViT-H/16 | 2B | 384 |

See repo README for download links.

---

## What you can do with it

### 1. Video classification / understanding
The demo notebook shows this. Feed a video, get action recognition outputs. SOTA on Something-Something-v2 (77.7%), EPIC-KITCHENS, Ego4D.

### 2. Feature extraction for downstream tasks
Use the encoder as a frozen backbone for any video-related task (captioning, QA, retrieval).

### 3. Robot planning (V-JEPA 2-AC)
This requires:
- The action-conditioned predictor (post-trained on DROID data)
- A goal image specifying what state you want to reach
- MPC loop: predict future states for many action candidates, pick the one closest to goal

The robot planning code is in the repo under post-training configs, but requires robot trajectory data in DROID format.

---

## Videos and demos

| What | URL |
|---|---|
| Robot planning demos (pick-and-place, reaching, grasping) | https://ai.meta.com/research/vjepa/ |
| Blog post with embedded demo videos | https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/ |
| Paper (animated figures) | https://arxiv.org/abs/2506.09985 |

---

## Screenshots

Real sequence of the Franka robot performing zero-shot pick-and-place (no training in this environment):

![V-JEPA 2, Franka approaching the cup](figures/vjepa2-robot-approaching.png)

![V-JEPA 2, Franka grasping the cup](figures/vjepa2-robot-grasping.png)

![V-JEPA 2, Franka lifting the cup](figures/vjepa2-robot-lifting.png)

---

## Key results (from the paper)

- **Video understanding:** SOTA on SSv2, EPIC-KITCHENS, Ego4D STA
- **Robot planning:** 65-80% success on pick-and-place (zero-shot, new environments)
- **V-JEPA 2.1:** +20% grasping over V-JEPA 2; navigation planning 10x faster than NWM

---

## Limitations

- Robot planning (V-JEPA 2-AC) requires robot trajectory data (DROID format) for post-training
- Goals must be specified as images (no natural-language goals yet)
- Large models (ViT-H/16 in V-JEPA 2.1, 2B params) need significant GPU memory (24+ GB)
- Zero-shot works only for "foundational" tasks (reach, grasp, pick-place), not complex multi-step manipulation

---

## Links

- GitHub: https://github.com/facebookresearch/vjepa2
- HuggingFace: https://huggingface.co/facebook/vjepa2-vith-fpc64-256
- Colab: https://colab.research.google.com/github/facebookresearch/vjepa2/blob/main/notebooks/vjepa2_demo.ipynb
- Paper V-JEPA 2: https://arxiv.org/abs/2506.09985
- Paper V-JEPA 2.1: https://arxiv.org/abs/2603.14482
- Meta AI blog: https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/
- LearnOpenCV tutorial: https://learnopencv.com/v-jepa-2-meta-world-model-robotics-guide/

---

## Related pages

- **Theory:** [Articles - World Models](../articles.md) (V-JEPA 2.1 section)
- **Benchmarks:** [Experiments](../experiments.md) (section 4)
- **Computational cost:** [Costs](../costs.md) (V-JEPA 2: 10x faster than NWM)
- **Hierarchical planning on top of V-JEPA 2:** HWM in [Experiments](../experiments.md) (Franka pick-and-place 0% to 70%)
