# NVIDIA Cosmos 3

---

## What it is

Omnimodal world model for Physical AI, a unified Mixture-of-Transformers architecture that jointly processes and generates **language, images, video, audio, and action sequences**. It effectively subsumes vision-language models, video generators, world simulators, and world-action models into a single framework.

Released June 2026. Fully open (code, checkpoints, datasets, deployment tools).

---

## Key concepts

- **Omnimodal:** one model handles text, image, video, audio, and robot actions
- **Reasoner + Generator:** Cosmos 3 has two sub-models, one for understanding/reasoning and one for generation
- **Physical AI:** designed for robotics, autonomous vehicles, and smart infrastructure
- **NIM microservices:** NVIDIA's production-ready inference containers for easy deployment

---

## Hardware requirements

| Variant | Params | Hardware needed |
|---|---|---|
| **Cosmos 3 Nano** | 16B (8B reasoner + 8B generator) | Workstation GPU (RTX PRO 6000) |
| **Cosmos 3 Super** | 64B (32B reasoner + 32B generator) | NVIDIA Hopper or Blackwell GPUs |

NIM containers require Docker + NVIDIA GPU runtime.

---

## Quick start: NIM container (easiest for inference)

```bash
export NGC_API_KEY=<your_key>

# Nano Reasoner
docker run --gpus=all \
  -e NGC_API_KEY=$NGC_API_KEY \
  -e NIM_MODEL_SIZE=nano \
  -p 8000:8000 \
  nvcr.io/nim/nvidia/cosmos3-reasoner:latest

# Query via OpenAI-compatible API
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/cosmos3-reasoner",
    "messages": [{"role": "user", "content": "Describe this scene..."}]
  }'
```

For Super Reasoner: set `NIM_MODEL_SIZE=super`.

---

## Quick start: HuggingFace

```python
from diffusers import Cosmos3OmniPipeline

pipe = Cosmos3OmniPipeline.from_pretrained("nvidia/Cosmos3-Nano")
# Use for text-to-video, image-to-video, and action generation
```

Models:
- [nvidia/Cosmos3-Nano](https://huggingface.co/nvidia/Cosmos3-Nano)
- [nvidia/Cosmos3-Super](https://huggingface.co/nvidia/Cosmos3-Super)

---

## Full setup (GitHub)

```bash
git clone https://github.com/nvidia/cosmos.git
cd cosmos

# Prerequisites
# - uv, git, git-lfs installed
# - HuggingFace token (for gated model access)
# - NGC API key (for NIM containers)

# Authenticate
uvx huggingface-hub@latest auth login
export HF_TOKEN=<your_token>
```

See `cookbooks/cosmos3/README.md` for complete examples including:
- Text-to-video generation
- Image-to-video generation
- Physical reasoning
- Temporal localization
- Grounding
- Action generation

---

## Capabilities

### 1. World Understanding (Reasoner)
- Scene description and physical reasoning
- Temporal event localization ("at what point does X happen?")
- Spatial grounding ("where is the object?")

### 2. World Generation (Generator)
- Text-to-video: "A robot arm picks up a red cube and places it in a box"
- Image-to-video: extend a still image into a video sequence
- Action generation: output pixel-space (x,y) trajectory coordinates for robot control

### 3. Vision-to-Action
Example prompt: "Put the flower into the red bottle"
Output: pixel trajectory coordinates for a pick-and-place task

---

## Videos and demos

| What | URL |
|---|---|
| Cosmos 3 capabilities (reasoning, generation, action) | https://research.nvidia.com/labs/cosmos-lab/cosmos3/ |
| Technical blog | https://developer.nvidia.com/blog/develop-physical-ai-reasoning-world-and-action-models-with-nvidia-cosmos-3/ |
| GTC keynote presentation | https://www.youtube.com/watch?v=kChwwFb5gMU |
| HuggingFace launch blog | https://huggingface.co/blog/nvidia/cosmos-3-for-physical-ai |

---

## Who uses Cosmos

NVIDIA lists these companies as Cosmos ecosystem users:
- **1X, Agility, XPENG**: humanoid robotics
- **Uber, Waabi, Waymo**: autonomous driving simulation
- **Universal Robots**: collaborative robots (cobots)

---

## Limitations

- **Nano** (16B) requires a workstation-grade GPU, not a consumer card
- **Super** (64B) requires data-center hardware
- Action generation is still early-stage compared to dedicated robot models
- Requires NVIDIA ecosystem (NGC, NIM, CUDA)

---

## Links

- GitHub: https://github.com/nvidia/cosmos
- Cookbooks: https://github.com/NVIDIA/cosmos/blob/main/cookbooks/cosmos3/README.md
- Technical report: https://research.nvidia.com/labs/cosmos-lab/cosmos3/technical-report.pdf
- Blog: https://developer.nvidia.com/blog/develop-physical-ai-reasoning-world-and-action-models-with-nvidia-cosmos-3/
- HuggingFace: https://huggingface.co/nvidia/Cosmos3-Nano
- Project page: https://research.nvidia.com/labs/cosmos-lab/cosmos3/

---

## Related pages

- **Mentioned in:** [Best Practices](../best-practices.md) section 7 (frontier of foundational world models)
- **Cost comparison:** [Costs](../costs.md): Cosmos (generative) takes ~4 min/action vs 16s for V-JEPA 2, illustrating the latent vs pixel difference
- **Industrial context:** see [Hands-On](../hands-on.md) for deployment examples
