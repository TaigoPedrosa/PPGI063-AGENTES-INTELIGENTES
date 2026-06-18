# DreamerV4 (Community PyTorch Implementation)

---

## What it is

Unofficial PyTorch implementation of Dreamer-V4's world model component: a **causal tokenizer** and an **interactive dynamics model (denoiser)**. Trained on real-world robotic datasets with interactive demos you can control with a keyboard or Xbox joystick.

You can literally **play inside a learned world model** in real time.

---

## Key concepts

- **Causal tokenizer:** converts video frames into discrete token sequences
- **Dynamics denoiser:** predicts future tokens conditioned on past tokens + actions
- **Interactive environment:** KV-caching enables real-time generation, you send actions, the model generates the next frame
- **Scalable world models:** the paper ("Training Agents Inside of Scalable World Models") proposes this as the architecture for training policies at scale

---

## Hardware requirements

- **Single 4090 GPU** (8 GB VRAM for the 110M-param small model)
- Real-time inference with KV-caching
- Conda environment recommended

---

## Installation

```bash
git clone https://github.com/Serenade-Intermezzo/dreamer-v4.git
cd dreamer-v4
conda env create -f environment.yml
conda activate dreamer-v4
pip install -e .
```

---

## Download checkpoints

Available at: https://huggingface.co/Rooholla/dreamer-v4

| Environment | Config | Resolution | Params | Checkpoint |
|---|---|---|---|---|
| SOAR (robotic manipulation) | soar-small.yaml | 256x256 | 110M | [download](https://huggingface.co/Rooholla/dreamer-v4/resolve/main/dynamics-soar-110M.pt) |
| PushT (2D pushing) | pusht config | 96x96 | 110M | [download](https://huggingface.co/Rooholla/dreamer-v4/resolve/main/dynamics-pushT-110M.pt) |

You also need the tokenizer checkpoint and downscaled datasets from the same HuggingFace repo.

---

## Interactive demos

### SOAR environment (robot manipulation)

```bash
python scripts/play-soar.py \
  --config-name dynamics/soar-small.yaml \
  tokenizer_ckpt=path/to/tokenizer.pt \
  dynamics_ckpt=path/to/dynamics.pt \
  dataset.data_dir=/path/to/soar_data_sharded
```

### PushT environment

```bash
python scripts/play-pushT.py \
  tokenizer_ckpt=path/to/soar/tokenizer.pt \
  dynamics_ckpt=path/to/dynamics.pt \
  dataset.data_dir=/path/to/pusht_data_sharded
```

### Controls
- **Keyboard:** WASD keys to command the robot
- **Xbox controller:** add `--xbox` flag (strongly recommended for smoother control)

---

## Training your own model

### Tokenizer training

```bash
torchrun --standalone --nproc_per_node=1 train_tokenizer.py \
  --config-path scripts/config/tokenizer \
  dataset.data_dir=/path/to/data
```

### Dynamics model training

```bash
torchrun --standalone --nproc_per_node=1 train_dynamics.py \
  --config-path scripts/config/dynamics \
  dataset.data_dir=/path/to/data \
  train.batch_per_gpu=1 \
  train.grad_accum_steps=1
```

Multi-GPU: increase `--nproc_per_node`.

---

## What makes this special

This is one of the few world models where you can **interact with it in real time** as a user. Instead of just watching pre-recorded demos, you actively control the robot and see how the world model generates the consequences of your actions frame by frame.

This makes it an extremely powerful demonstration tool for understanding what "world models" actually do: predict the next state of the world given your action.

---

## Links

- GitHub: https://github.com/Serenade-Intermezzo/dreamer-v4
- Checkpoints: https://huggingface.co/Rooholla/dreamer-v4
- Paper: https://arxiv.org/abs/2509.24527
- Alternate implementation (lucidrains): https://github.com/lucidrains/dreamer4

---

## Related pages

- **Previous generation:** see [DreamerV3](dreamerv3.md) for the algorithm published in Nature 2025
- **Same environment (PushT):** LeWorldModel also uses PushT, compare approaches in [Experiments](../experiments.md)
- **Conceptual:** demonstrates the "dream inside the model" paradigm discussed in [Best Practices](../best-practices.md) section 5
