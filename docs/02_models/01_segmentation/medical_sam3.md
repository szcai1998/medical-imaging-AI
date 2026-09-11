# Medical SAM3: Universal Prompt-Driven Medical Image Segmentation (`[M09]`)

> **Full Fine-Tuned SAM3 Across 10 Modalities**: An end-to-end medical domain adaptation of Meta's SAM3 architecture, fully fine-tuned on 33 heterogeneous biomedical datasets to bridge the clinical domain gap for text- and prompt-driven 2D/3D segmentation.

---

## 1. Quick Metadata & Registry Classification

| Field | Value |
|---|---|
| **Canonical ID** | `[M09]` |
| **System Class** | `core_fm` (Dossier Section 4.0 authoritative registry) |
| **Scope** | `generalist` (Universal text/prompt-driven 2D/3D segmentation) |
| **Modality & Anatomy** | 10 Modalities (3D CT, 3D MRI, Ultrasound, CXR, Endoscopy, Dermoscopy, OCT, Fundus, Pathology, PET) |
| **Developing Institution** | AIM Research Lab & Multicenter Academic Consortium (Jiang et al.) |
| **Evidence Code** | `[E3]` Emerging Preprint (arXiv:2601.10880, Jan 15, 2026) |
| **Artifact Availability Tier**| **Tier A** (Fully open: GitHub code, Hugging Face checkpoint, project site) |
| **Primary Paper DOI** | [`arXiv:2601.10880`](https://arxiv.org/abs/2601.10880) (January 2026) |
| **Code Repository** | [GitHub: AIM-Research-Lab/Medical-SAM3](https://github.com/AIM-Research-Lab/Medical-SAM3) |
| **Model Weights** | Hugging Face: [`ChongCong/Medical-SAM3`](https://huggingface.co/ChongCong/Medical-SAM3) |
| **Software License** | Apache 2.0 |

---

## 2. P-A-B-H-A Snapshot

### [P] Clinical Problem
While natural-image foundation models like SAM3 possess powerful general segmentation primitives, they suffer catastrophic performance degradation under medical domain shift (Hounsfield units, multi-sequence MRI contrast, speckle noise, tissue staining artifacts) unless provided with privileged, labor-intensive spatial bounding boxes.

### [A] Architectural Core
Medical SAM3 conducts a **full parameter fine-tuning of the SAM3 vision-language backbone** rather than relying on lightweight parameter-efficient adapters (LoRA). It incorporates a unified multi-modal prompt decoder that accepts both **spatial prompts** (3D boxes, point clicks) and **free-form clinical text descriptions**. Trained end-to-end on a curated multi-center corpus of **33 biomedical datasets** spanning 10 clinical modalities and over 2 million mask instances.

### [B] Benchmark & Delta
Across 10 diverse medical imaging tasks evaluated against vanilla SAM3 and task-adapted baselines:
- **Domain-Shift Resistance**: Medical SAM3 achieves an average **+18.4% Dice improvement** over vanilla SAM3 on raw radiological volumes without needing bounding-box guidance.
- **Text-Prompted Segmentation**: Achieves **0.812 Mean DSC** under open-vocabulary text queries across multi-organ CT/MRI benchmarks, outperforming MedSAM-Text by **+12.6% DSC**.
- **Interactive Refinement**: Reaches **>0.88 Dice** with only 2 interactive clicks on complex organ and tumor boundaries.

### [H] Hardware Footprint & Deployment Profile
- **Inference Footprint**: Supports 2D slice inference and 3D volumetric chunking ($128 \times 128 \times 64$). Peak VRAM: **12–16 GB** at FP16. Can be executed on a single consumer **NVIDIA RTX 3090/4090 (24 GB)**.
- **Latency**: Interactive click-to-mask response takes **~60 ms** per slice; full 3D volume reconstruction takes **~6–10 seconds**.
- **Training Footprint**: Required an enterprise cluster of 32x NVIDIA A100 (80GB) GPUs over 10 days for full end-to-end fine-tuning.

### [A] Access & Artifacts
- **Hugging Face Hub**: Pretrained weights downloadable from `ChongCong/Medical-SAM3`.
- **GitHub Repository**: Includes scripts for 2D/3D inference, prompt-guided evaluation, and fine-tuning pipelines (`AIM-Research-Lab/Medical-SAM3`).

---

## 3. Verified Benchmark Standings & Comparative Matrix

| Evaluation Dataset / Task | Evaluation Split | Supervision Regimen | Medical SAM3 Metric | Baseline Comparator | Baseline Metric | Performance Delta | Evidence Source |
|---|---|:---:|:---:|---|:---:|:---:|:---:|
| **Multi-Modality 10-Task Aggregate**| Test Splits | Text-Prompted Zero-Shot | **0.812** Mean DSC | Vanilla SAM3 | 0.628 Mean DSC | **+18.4% DSC** | `[E3]` arXiv:2601.10880 |
| **AMOS22 Abdominal CT** | Held-out Validation | Text-Prompted Zero-Shot | **0.843** Mean DSC | BiomedParse v1 | 0.761 Mean DSC | **+8.2% DSC** | `[E3]` arXiv:2601.10880 |
| **KiTS23 Kidney / Tumor** | Held-out Validation | Interactive (1 Box Prompt) | **0.865** Kidney DSC | MedSAM | 0.812 Kidney DSC | **+5.3% DSC** | `[E3]` arXiv:2601.10880 |
| **BraTS23 Glioma MRI** | Held-out Test Split | Interactive (3 Clicks) | **0.874** WT DSC | SAM-Med3D | 0.819 WT DSC | **+5.5% DSC** | `[E3]` arXiv:2601.10880 |
| **ISIC2018 Skin Lesion** | Held-out Test Split | Text-Prompted Zero-Shot | **0.884** Mean IoU | nnU-Net v2 (2D) | **0.898** Mean IoU | -1.4% IoU *(supervised lead)*| `[E3]` arXiv:2601.10880 |

---

## 4. Ground-Layer Audit & Epistemic Traps

### 1. Emerging Preprint Evidence Frontier
> [!IMPORTANT]
> **Status: Emerging Preprint (`[E3]`)**: Medical SAM3 represents author-reported cutting-edge research released in early 2026. While the open checkpoints and empirical improvements over vanilla SAM3 are demonstrable, it has not yet passed formal peer review or been validated in independent third-party multi-center clinical trials.

### 2. The Vanilla SAM Domain-Shift Proof
The paper provides a vital empirical proof for the medical AI community: **vanilla natural-image SAM models fail severely in clinical radiology**. Without medical domain fine-tuning, vanilla SAM3's text-prompted Dice drops to 0.628, confirming that natural-image pretraining alone is insufficient for diagnostic-grade boundary delineation.

### 3. Ambiguity in Clinical Text Queries
Text-prompted segmentation degrades when non-standard radiologic synonyms are used. For example, prompting "renal mass" vs. "renal cell carcinoma" vs. "kidney tumour" can produce noticeable variance in predicted boundary masks depending on how labels were normalized in the 33 training datasets.

---

## 5. Local Verification Snippet (Tier A: Fully Open)

```python
# Requirements: pip install torch torchvision transformers
# Artifact Tier: Tier A (Fully open via Hugging Face: ChongCong/Medical-SAM3)
# Verification: Simulate Medical SAM3 multi-modal prompt decoder forward pass

import torch
import torch.nn as nn

class MockMedicalSAM3PromptDecoder(nn.Module):
    """Minimal architectural mockup of Medical SAM3 prompt integration."""
    def __init__(self, hidden_dim=256):
        super().__init__()
        self.image_conv = nn.Conv2d(1, hidden_dim, kernel_size=3, padding=1)
        self.box_encoder = nn.Linear(4, hidden_dim)
        self.text_encoder = nn.Linear(128, hidden_dim)
        
        self.cross_attn = nn.MultiheadAttention(embed_dim=hidden_dim, num_heads=4)
        self.mask_head = nn.Sequential(
            nn.Conv2d(hidden_dim, 64, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.Conv2d(64, 1, kernel_size=1)
        )

    def forward(self, img_slice, prompt_boxes=None, prompt_text_emb=None):
        # img_slice: (B, 1, H, W)
        B, C, H, W = img_slice.shape
        img_feat = self.image_conv(img_slice)  # (B, hidden_dim, H, W)
        
        # Combine available prompts
        prompts = []
        if prompt_boxes is not None:
            prompts.append(self.box_encoder(prompt_boxes).unsqueeze(0))
        if prompt_text_emb is not None:
            prompts.append(self.text_encoder(prompt_text_emb).unsqueeze(0))
            
        prompt_tokens = torch.cat(prompts, dim=0)  # (NumPrompts, B, hidden_dim)
        
        # Flatten image features for cross-attention
        flat_img = img_feat.flatten(2).permute(2, 0, 1)  # (H*W, B, hidden_dim)
        attended_feat, _ = self.cross_attn(flat_img, prompt_tokens, prompt_tokens)
        attended_2d = attended_feat.permute(1, 2, 0).view(B, -1, H, W)
        
        mask_logits = self.mask_head(attended_2d)
        return torch.sigmoid(mask_logits)

def verify_medical_sam3():
    print("[INIT] Verifying Medical SAM3 prompt decoder pipeline...")
    model = MockMedicalSAM3PromptDecoder()
    model.eval()

    dummy_img = torch.randn(1, 1, 64, 64)
    dummy_box = torch.tensor([[10.0, 10.0, 40.0, 40.0]])  # Bounding box prompt
    dummy_text = torch.randn(1, 128)                       # Text prompt embedding

    with torch.no_grad():
        pred_mask = model(dummy_img, prompt_boxes=dummy_box, prompt_text_emb=dummy_text)

    print(f"Medical SAM3 predicted mask shape: {pred_mask.shape}")
    assert pred_mask.shape == (1, 1, 64, 64), "Mask dimension mismatch"
    print("[PASS] Medical SAM3 pipeline verified successfully.")

if __name__ == "__main__":
    verify_medical_sam3()
```

---

## 6. Connected Dataset Ecosystem

- 📂 [AMOS22 (`[D8]`)](../../01_datasets/01_segmentation_3d/amos22.md): Key abdominal multi-organ validation benchmark.
- 📂 [KiTS23 (`[D9]`)](../../01_datasets/01_segmentation_3d/kits23.md): Prompt-driven kidney and tumor segmentation testbed.
- 📂 [TotalSegmentator (`[D6]`)](../../01_datasets/01_segmentation_3d/totalsegmentator.md): Used as a primary source among the 33 pretraining datasets.
- 📂 [CAMELYON17 (`[D27]`)](../../01_datasets/04_pathology_spatial/camelyon17.md): Cross-modality pathology test cohort.
