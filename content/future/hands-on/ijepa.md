# I-JEPA (Meta FAIR)

---

## What it is

Image-based Joint-Embedding Predictive Architecture, predicts the representation of masked image patches from visible context patches, entirely in latent space (no pixel reconstruction).

Published at CVPR 2023. This is the conceptual ancestor of V-JEPA and the simplest way to understand the JEPA paradigm hands-on.

---

## Key concepts

- **Predict representations, not pixels:** the target encoder produces a representation of the masked region; the predictor tries to match it from context
- **No data augmentation bias:** unlike contrastive methods (SimCLR, DINO), I-JEPA does not rely on hand-crafted augmentations
- **Self-supervised:** no labels required for pre-training
- **Abstract representations:** the model learns to predict *semantic meaning* of patches, not pixel-level details

---

## Hardware requirements

- **Inference (HuggingFace):** CPU is sufficient for small batches; GPU speeds up batch processing
- **Pre-training from scratch:** multi-GPU setup (the paper used 16 A100 80GB GPUs for ImageNet-1K)
- **Fine-tuning / linear probing:** single GPU sufficient

---

## Quick start: HuggingFace pipeline (5 lines)

```python
from transformers import pipeline

feature_extractor = pipeline(
    task="image-feature-extraction",
    model="facebook/ijepa_vith14_1k",
    device=0,  # use -1 for CPU
)

features = feature_extractor(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/pipeline-cat-chonk.jpeg",
    return_tensors=True
)
print(f"Feature shape: {features.shape}")
```

---

## Image similarity example

```python
from transformers import AutoModel, AutoProcessor
from torch.nn.functional import cosine_similarity
from PIL import Image
import requests

processor = AutoProcessor.from_pretrained("facebook/ijepa_vith14_1k")
model = AutoModel.from_pretrained("facebook/ijepa_vith14_1k", device_map="auto")

def get_embedding(url):
    image = Image.open(requests.get(url, stream=True).raw)
    inputs = processor(image, return_tensors="pt").to(model.device)
    outputs = model(**inputs)
    return outputs.last_hidden_state.mean(dim=1)

embed_1 = get_embedding("http://images.cocodataset.org/val2017/000000039769.jpg")
embed_2 = get_embedding("http://images.cocodataset.org/val2017/000000219578.jpg")

similarity = cosine_similarity(embed_1, embed_2)
print(f"Similarity: {similarity.item():.4f}")
```

This demonstrates that I-JEPA learns meaningful semantic representations: similar images will have high cosine similarity even though the model was never trained with labels.

---

## Training from scratch (local)

```bash
git clone https://github.com/facebookresearch/ijepa.git
cd ijepa
pip install pyyaml numpy opencv-python submitit torch torchvision

# Single machine, multiple GPUs
python main.py --fname configs/in1k_vith14_ep300.yaml --devices cuda:0 cuda:1 cuda:2

# SLURM cluster (distributed)
python main_distributed.py \
  --fname configs/in1k_vith14_ep300.yaml \
  --folder $path_to_save_logs \
  --partition $slurm_partition \
  --nodes 2 --tasks-per-node 8 \
  --time 1000
```

---

## What you can do with it

1. **Feature extraction:** use I-JEPA embeddings as input to downstream classifiers, retrieval systems, or similarity search
2. **Image classification:** fine-tune or linear-probe on ImageNet (achieves strong performance without labels)
3. **Understand JEPA:** the simplest entry point to see how "prediction in latent space" works in practice

---

## Links

- GitHub: https://github.com/facebookresearch/ijepa
- HuggingFace model: https://huggingface.co/facebook/ijepa_vith14_1k
- HuggingFace docs: https://huggingface.co/docs/transformers/en/model_doc/ijepa
- Blog (with architecture diagrams): https://ai.meta.com/blog/yann-lecun-ai-model-i-jepa/
- Paper: https://arxiv.org/abs/2301.08243
- Explainer: https://encord.com/blog/i-jepa-explained/

---

## Related pages

- **Theory:** [Articles - World Models](../articles.md) (LeCun's "A Path Towards AMI" introduces the JEPA concept)
- **Evolution:** I-JEPA (images, 2023) → V-JEPA (video, 2024) → V-JEPA 2 (robot planning, 2025) → V-JEPA 2.1 (dense features, 2026)
- **Next step:** to see JEPA applied to *planning and action*, see [V-JEPA 2](vjepa2.md) or [LeWorldModel](leworldmodel.md)
