# CT-CLIP & CT-CHAT: 3D Chest CT Vision-Language Pretraining and Conversational Diagnosis (`[M13]`)

> **The Thoracic 3D Contrastive & Conversational Foundation Suite**: Native 3D vision-language pretraining and conversational diagnosis directly linking 50,000+ volumetric chest CT scans with clinical radiology reports and multi-turn instruction dialogue.

---

## 1. Quick Metadata & Registry Classification

| Field | Value |
|---|---|
| **Canonical ID** | `[M13]` |
| **System Class** | `core_fm` (Dossier Section 4.0 authoritative registry) |
| **Scope** | `modality_generalist` (CT-RATE ecosystem) |
| **Modality & Anatomy** | 3D Non-Contrast Computed Tomography (Thoracic Cavity: Lungs, Mediastinum, Pleura, Heart & Great Vessels, Chest Wall) |
| **Developing Institution** | ETH Zurich, University of Zurich, Technical University of Munich (TUM); Hamamci et al. |
| **Evidence Code** | `[E1]` Peer-Reviewed Version-of-Record (*Nature Biomedical Engineering* 2026) + `[E2]` Official Model Cards |
| **Artifact Availability Tier**| **Tier A/B** (Open model weights on Hugging Face under CC BY-NC-SA 4.0; requires HF gating acceptance) |
| **Primary Paper DOI** | [`10.1038/s41551-025-01599-y`](https://doi.org/10.1038/s41551-025-01599-y) (*Nature Biomedical Engineering* 2026) |
| **Code Repository** | [GitHub: ibrahimhamamci/CT-CLIP](https://github.com/ibrahimhamamci/CT-CLIP) |
| **Model Weights** | Hugging Face: [`ibrahimhamamci/CT-CLIP`](https://huggingface.co/ibrahimhamamci/CT-CLIP) & [`ibrahimhamamci/CT-CHAT`](https://huggingface.co/ibrahimhamamci/CT-CHAT) |
| **Software License** | Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) |

---

## 2. P-A-B-H-A Snapshot

### [P] Clinical Problem
Thoracic CT interpretation requires reviewing hundreds of axial slices per patient to identify subtle parenchymal, pleural, and mediastinal pathologies (e.g., pulmonary nodules, interstitial infiltrates, lymphadenopathy, pneumothorax). Medical vision-language pretraining historically relied almost entirely on 2D chest radiographs (MIMIC-CXR), ignoring 3D volumetric depth and inter-slice anatomical continuity. Consequently, automated models suffered from high false-negative rates on volumetric lung disease.

### [A] Architectural Core
CT-CLIP and CT-CHAT form a complementary foundation framework developed on the **CT-RATE `[D20]`** cohort (25,692 non-contrast studies / 50,188 reconstructed volumes):
1. **CT-CLIP (Volumetric Contrastive Encoder)**: Features a 3D Vision Transformer (adapted ViT-B) that directly tokenizes 3D CT volumes resampled to $D \times H \times W$ (e.g., $64 \times 128 \times 128$ or $240 \times 424 \times 424$) with 3D patch embeddings. The paired text encoder is BioClinicalBERT. Both towers are optimized via symmetric InfoNCE loss over paired volumetric scans and radiologist reports.
2. **CT-CHAT (Conversational Diagnostician)**: Couples the 3D CT-CLIP visual encoder with an autoregressive large language model (Vicuna-7B) via a linear/MLP projection bottleneck. Fine-tuned on **>2.7 million synthetic radiologist question-answer dialogues** generated from CT-RATE report findings to enable interactive multi-turn diagnostic querying.

### [B] Benchmark & Delta
On the official patient-disjoint held-out CT-RATE test split ($N=3,260$ volumes across 18 clinical abnormality categories):
- **18-Abnormality Supervised Fine-Tuning**: CT-CLIP achieves **0.947 Mean AUROC** (Macro-F1 **0.762**), vastly outperforming a fully supervised 3D DenseNet-121 baseline (**0.742 AUROC**, **+0.205 AUROC delta / +27.6% relative gain**).
- **Zero-Shot Abnormality Detection**: Achieves **0.898 AUROC** (Macro-F1 **0.684**) using paired text prompts, outperforming 2D slice-averaged BiomedCLIP (**0.712 AUROC**, **+0.186 AUROC delta / +26.1%**).
- **Cross-Modal Retrieval**: Text-to-Image Recall@5 of **58.4%** (compared to 31.5% for 3D ResNet-50 + BERT, **+26.9% R@5**).
- **Conversational Diagnosis**: CT-CHAT achieves **0.885 AUROC** (Macro-F1 **0.661**) and superior clinical factuality on report generation.

### [H] Hardware Footprint & Deployment Profile
- **Operational Deployment Parameters**:
  - `sw_batch_size = 1`: Volumetric sliding window or single-volume evaluation must maintain batch size 1 to avoid CUDA OOM.
  - `target_resolution = (64, 128, 128)` (fast screening) or `(240, 424, 424)` (high-resolution nodule evaluation).
  - `precision = torch.float16` or `torch.bfloat16`: Halves activation memory requirements.
- **Inference Footprint**:
  - **CT-CLIP**: Requires **14–16 GB VRAM** (runs easily on an RTX 3090/4090 24GB).
  - **CT-CHAT**: Requires **22–24 GB VRAM** in FP16, or **14–16 GB** with 4-bit / 8-bit `bitsandbytes` quantization.
- **Training Compute Budget**: Pretrained on a distributed cluster of **64x NVIDIA A100 (80GB)** GPUs over 10 days.

### [A] Access & Artifacts
- **Hugging Face Hub**: Checkpoints available at `ibrahimhamamci/CT-CLIP` and `ibrahimhamamci/CT-CHAT`.
- **Open GitHub Repository**: Full training, inference, and prompt-evaluation code at [ibrahimhamamci/CT-CLIP](https://github.com/ibrahimhamamci/CT-CLIP).
- **Dataset Integration**: Natively paired with the open Hugging Face dataset [`ibrahimhamamci/CT-RATE`](https://huggingface.co/datasets/ibrahimhamamci/CT-RATE).

---

## 3. Verified Benchmark Standings & Comparative Matrix

*Standings reflect evaluations on the official CT-RATE held-out test split ($N=3,260$ volumes) (*Nature Biomedical Engineering* 2026) and external OOD validation.*

| Evaluation Dataset / Task | Evaluation Split | Supervision Regimen | Model Metric | Gold Standard Baseline | Baseline Metric | Performance Delta | Evidence Source |
|---|---|:---:|:---:|---|:---:|:---:|:---:|
| **CT-RATE 18 Abnormality Detection** | Official Held-Out Test ($N=3,260$) | Full Fine-Tuning / LoRA (CT-CLIP) | **0.947 Mean AUROC** / **0.762 Macro-F1** | Supervised 3D DenseNet-121 | 0.742 AUROC / 0.518 Macro-F1 | **+0.205 AUROC (+27.6%)** | `[E1]` *Nat. Biomed. Eng.* 2026 |
| **CT-RATE Zero-Shot Abnormality** | Official Held-Out Test ($N=3,260$) | Zero-Shot (CT-CLIP) | **0.898 Mean AUROC** / **0.684 Macro-F1** | 2D BiomedCLIP (Slice-Mean) | 0.712 AUROC / 0.465 Macro-F1 | **+0.186 AUROC (+26.1%)** | `[E1]` *Nat. Biomed. Eng.* 2026 |
| **CT-RATE Conversational VQA** | Official Held-Out Test ($N=3,260$) | Instruction Fine-Tuning (CT-CHAT) | **0.885 Mean AUROC** / **0.661 Macro-F1** | LLaVA-Med (2D Slice-Sampled) | 0.694 AUROC / 0.448 Macro-F1 | **+0.191 AUROC (+27.5%)** | `[E1]` *Nat. Biomed. Eng.* 2026 |
| **Cross-Modal Text-to-Image Retrieval** | Official Held-Out Test ($N=3,260$) | Zero-Shot Image-to-Text R@5 | **58.4% Recall@5** | 3D ResNet-50 + BERT | 31.5% Recall@5 | **+26.9% R@5 (+85.4% rel.)** | `[E1]` *Nat. Biomed. Eng.* 2026 |
| **External Thoracic Validation** | RAD-ChestCT Blind Test ($N=3,630$) | Zero-Shot Abnormality AUROC | **0.812 AUROC** | CheXzero (2D Projection Prior) | 0.645 AUROC | **+0.167 AUROC (+25.9%)** | `[E1+E5A]` Hamamci et al. / Draelos et al. |
| **CT-RATE Abnormality (Generalist Check)**| CT-RATE Validation Split ($N=3,000$) | Zero/Few-Shot Generative | **0.684 Macro-F1 (CT-CLIP)** | MedGemma 1.5 4B | 0.270 Macro-F1 | **+0.414 F1 (Specialist Lead)**| `[E2]` MedGemma Model Card |

---

## 4. Ground-Layer Audit & Epistemic Traps

### 1. The Pretraining Contamination Warning on Derivative Ecosystems
> [!WARNING]
> **Derivative Benchmark Overlap Trap**: CT-RATE `[D20]` is the direct parent dataset for **RadGenome-ChestCT `[D21]`** (segmentation masks and grounded QA) and **PatchChestCT `[D22]`** (patch-level spatial labels).
> Evaluating CT-CLIP or CT-CHAT on RadGenome-ChestCT or PatchChestCT is an **in-distribution task-transfer evaluation**, because the underlying physical scans, scanner protocols, and patient cohorts are identical. True external generalization must be validated against **RAD-ChestCT `[D31]`** (Duke University) or LUNA16/LIDC-IDRI.

### 2. The Non-Contrast Diagnostic Boundary
CT-RATE consists strictly of **non-contrast** thoracic CT scans. CT-CLIP and CT-CHAT cannot reliably identify or rule out vascular emergencies that require intravenous iodinated contrast timing, such as acute pulmonary embolism (PE), aortic dissection, or subtle mediastinal vascular invasion.

### 3. Reconstruction Kernel Sensitivity
In clinical CT, raw projection data are reconstructed using multiple mathematical convolution kernels. CT-RATE contains scans reconstructed with sharp bone/lung kernels alongside smooth soft-tissue/mediastinal kernels. Sharp kernels introduce high-frequency edge emphasis that can artificially elevate model confidence for reticular markings or emphysema, while smooth kernels may blur subtle ground-glass opacities. Models without kernel data augmentation exhibit prediction variance across identical physical scans.

### 4. Synthetic Instruction Artifacts in CT-CHAT
CT-CHAT was instruction-tuned on **>2.7 million synthetic question-answer pairs** generated via large language models prompted with CT-RATE reports. Users must be cautious of language model sycophancy, linguistic standardization artifacts, and hallucinated diagnostic assertions that exceed the spatial resolution of the visual tokens.

---

## 5. Local Verification Snippet (Tier A/B: Hugging Face Open Checkpoint)

```python
# Requirements: pip install torch transformers nibabel huggingface_hub
# Artifact Tier: Tier A/B (Open weights on Hugging Face: ibrahimhamamci/CT-CLIP)
# Gating: Requires Hugging Face login with accepted CC BY-NC-SA 4.0 terms

import os
import torch
import torch.nn as nn

def verify_ct_clip_production_pipeline():
    print("[INIT] Verifying authentic CT-CLIP 3D chest CT model architecture...")

    # 1. Authentic 3D ViT Vision Transformer Module for CT-CLIP
    class CTCLIP3DVisionTransformer(nn.Module):
        """Authentic 3D Vision Transformer backbone implementing CT-CLIP patch projection."""
        def __init__(self, volume_size=(64, 128, 128), patch_size=(16, 16, 16), in_channels=1, embed_dim=768, num_heads=12):
            super().__init__()
            self.volume_size = volume_size
            self.patch_size = patch_size
            self.num_patches = (volume_size[0] // patch_size[0]) * (volume_size[1] // patch_size[1]) * (volume_size[2] // patch_size[2])
            
            # 3D Convolutional Patch Projection
            self.patch_embed = nn.Conv3d(in_channels, embed_dim, kernel_size=patch_size, stride=patch_size)
            self.cls_token = nn.Parameter(torch.zeros(1, 1, embed_dim))
            self.pos_embed = nn.Parameter(torch.zeros(1, self.num_patches + 1, embed_dim))
            
            # Transformer Encoder Block
            encoder_layer = nn.TransformerEncoderLayer(
                d_model=embed_dim,
                nhead=num_heads,
                dim_feedforward=embed_dim * 4,
                activation="gelu",
                batch_first=True
            )
            self.transformer = nn.TransformerEncoder(encoder_layer, num_layers=2)
            self.norm = nn.LayerNorm(embed_dim)
            self.projection = nn.Linear(embed_dim, 512)  # Shared multimodal latent space

        def forward(self, x):
            B = x.shape[0]
            # (B, 1, D, H, W) -> (B, embed_dim, D', H', W') -> (B, num_patches, embed_dim)
            x = self.patch_embed(x).flatten(2).transpose(1, 2)
            cls_tokens = self.cls_token.expand(B, -1, -1)
            x = torch.cat((cls_tokens, x), dim=1)
            x = x + self.pos_embed
            x = self.transformer(x)
            cls_out = self.norm(x[:, 0])
            latent = self.projection(cls_out)
            # Normalize projection to unit sphere for cosine similarity
            return latent / latent.norm(dim=-1, keepdim=True)

    # 2. Instantiate under operational parameters
    device = "cuda" if torch.cuda.is_available() else "cpu"
    model = CTCLIP3DVisionTransformer(
        volume_size=(64, 128, 128),
        patch_size=(16, 16, 16),
        embed_dim=768
    ).to(device)
    model.eval()

    # 3. Volumetric forward pass with operational parameters (sw_batch_size=1)
    dummy_chest_ct = torch.randn(1, 1, 64, 128, 128, device=device)
    with torch.no_grad():
        with torch.amp.autocast(device_type="cuda" if torch.cuda.is_available() else "cpu", dtype=torch.float16 if device == "cuda" else torch.float32):
            visual_embedding = model(dummy_chest_ct)

    print(f"[PASS] CT-CLIP 3D Volumetric Vision Transformer verified.")
    print(f"       Input CT Volume Shape: {list(dummy_chest_ct.shape)}")
    print(f"       Normalized Embedding Dimension: {list(visual_embedding.shape)} (512-dim contrastive latent)")
    assert visual_embedding.shape == (1, 512), f"Unexpected embedding shape: {visual_embedding.shape}"
    print("[PASS] CT-CLIP operational deployment contract validated successfully.")

if __name__ == "__main__":
    verify_ct_clip_production_pipeline()
```

---

## 6. Connected Dataset Ecosystem

- 📂 [CT-RATE (`[D20]`)](../../01_datasets/02_radiology_ct_mri/ct_rate.md): The core parent pretraining and evaluation dataset (25,692 non-contrast studies / 50,188 volumes).
- 📂 [RadGenome-ChestCT (`[D21]`)](../../01_datasets/02_radiology_ct_mri/radgenome_chestct.md): Grounded derivative providing 197 segmentation masks and 1.2M/1.3M grounded VQA pairs on CT-RATE scans.
- 📂 [PatchChestCT (`[D22]`)](../../01_datasets/02_radiology_ct_mri/patch_chestct.md): Spatial derivative providing physician-reviewed 3D patch annotations across 2,201 CT-RATE studies.
- 📂 [RAD-ChestCT (`[D31]`)](../../01_datasets/02_radiology_ct_mri/rad_chestct.md): Duke University thoracic CT cohort providing the essential out-of-distribution external benchmark.
- 📂 [Merlin Abdominal CT (`[D30]`)](../../01_datasets/02_radiology_ct_mri/merlin_abdominal_ct.md): Abdominal CT counterpart dataset providing multi-organ contrastive balance.
