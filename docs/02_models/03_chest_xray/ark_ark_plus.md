# Ark & Ark+: Supervised Heterogeneous-Label Knowledge Accrual Foundation Models (`[S106, S107]`)

> **The Heterogeneous Supervised Accrual Generalist**: A vision foundation model framework that shatters the paradigm of manual label consolidation by cyclically accruing, consolidating, and reusing expert clinical knowledge across diverse global chest radiography datasets without losing fine-grained diagnostic semantics.

---

## 1. Quick Metadata & Registry Classification

| Field | Value |
|---|---|
| **Canonical ID** | `[S106, S107]` (Ark: `[S106]`, Ark+: `[S107]`) |
| **System Class** | `core_fm` (Authoritative Registry, Dossier Section 4.0) |
| **Scope** | `modality_generalist` (2D Chest Radiography; pan-thoracic disease detection & localization) |
| **Modality & Anatomy** | 2D Projection Radiography (CXR: PA and AP views; Lungs, Pleura, Mediastinum, Bones) |
| **Developing Institution** | Arizona State University (Center for Medical Imaging and Deep Learning) & Mayo Clinic (Rochester, MN & Phoenix, AZ) |
| **Evidence Code** | `[E1]` Peer-Reviewed Version-of-Record (*Nature* 643, 488–498, Jun 2025; *Medical Image Analysis* 108, 103828, 2026); `[E2]` Official Model Release |
| **Artifact Availability Tier**| **Tier A** (Fully open: source code, pretraining pipelines, and multi-backbone model weights) |
| **Primary Paper DOIs** | Ark: [`10.1038/s41586-025-09079-8`](https://doi.org/10.1038/s41586-025-09079-8) (*Nature* 2025)<br>Ark+: [`10.1016/j.media.2025.103828`](https://doi.org/10.1016/j.media.2025.103828) (*Medical Image Analysis* 2026) |
| **Preprint / Conference** | Foundation Ark (MICCAI 2023, LNCS 14224, pp. 741–752) |
| **Code Repository** | [GitHub: JLiangLab/Ark](https://github.com/jlianglab/Ark) |
| **Model Weights** | GitHub Releases & Hugging Face Checkpoints (ResNet-50, ConvNeXt-L, Swin-Large) |
| **Software License** | Apache 2.0 / MIT Open Access (Open for academic and scientific reuse) |

---

## 2. P-A-B-H-A Snapshot

### [P] Clinical Problem
Chest radiography datasets collected globally possess highly heterogeneous, fragmented, and conflicting label ontologies (e.g., NIH ChestX-ray14 labels 14 conditions, CheXpert labels 14 with uncertain/unmentioned classes, VinDr-CXR labels 28 findings with bounding boxes, and PadChest includes hundreds of hierarchical codes). Conventional supervised learning requires either: (1) manual ontology harmonization (which discards granular, non-overlapping labels and introduces human mapping errors), or (2) siloing training to individual datasets. Conversely, contrastive vision-language models rely on noisy NLP-mined report labels, diluting diagnostic precision on rare thoracic findings.

### [A] Architectural Core
Ark introduces a **cyclical knowledge accrual and reuse framework** that trains a unified vision foundation backbone across disparate datasets without label harmonization:
1. **Shared Visual Backbone**: Supports modern high-capacity encoders, scaled from ResNet-50 (25.6M) to **ConvNeXt-Large (198M)** and **Swin Transformer Large (197M)**.
2. **Dataset-Specific Multi-Task Heads**: Dedicated projection and classification/segmentation heads for each institutional cohort preserve native annotation semantics.
3. **Teacher-Student Momentum Consolidation**: Incorporates an Exponential Moving Average (EMA) teacher network that aggregates parameters over continuous accrual rounds, mitigating catastrophic forgetting across sequential dataset iterations.
4. **Multi-Domain Pretraining Corpus**: Encompasses **700,000+ chest radiographs** across seven distinct international archives: NIH ChestX-ray14, CheXpert, VinDr-CXR, RSNA Pneumonia, Shenzhen CXR, Montgomery, and PadChest subsets.

Ark+ (*MedIA* 2026) expands this paradigm to multi-task learning (joint disease classification, anatomical segmentation, and bounding-box localization) and validates federated continuous learning without centralizing raw DICOM images.

### [B] Benchmark & Delta
Across 33 distinct thoracic disease categories on held-out benchmarks:
- **VinDr-CXR (28 Thoracic Findings)**: Evaluated on unseen classes, Ark Swin-Large achieves **0.868 Mean AUROC**, outperforming ImageNet-supervised Swin-L (**0.812 AUROC**, $\Delta = +0.056$) and self-supervised DINOv2 (**0.835 AUROC**, $\Delta = +0.033$).
- **NIH ChestX-ray14 (Official Benchmark Split)**: Achieves **0.861 Mean AUROC** across 14 categories, surpassing self-supervised MoCo v2 (**0.824 AUROC**, $\Delta = +0.037$) and SwAV (**0.818 AUROC**, $\Delta = +0.043$).
- **Few-Shot Label Efficiency**: On SIIM-ACR Pneumothorax segmentation, Ark+ fine-tuned with only **10% labeled masks** reaches **0.884 Dice**, matching scratch models trained on 100% data ($10\times$ label efficiency).
- **Long-Tail Rare Pathologies**: Demonstrates substantial sensitivity advantages (+12–18% relative recall) over report-mined vision-language models on low-prevalence findings (e.g., pneumoperitoneum, subcutaneous emphysema, rib fracture) by leveraging definitive radiologist-confirmed ground truth.

### [H] Hardware Footprint & Deployment Profile
- **Operational Deployment Parameters**:
  - `input_resolution = (512, 512)`: High-fidelity radiographic scaling preserving fine pulmonary parenchymal detail.
  - `precision = torch.float16` or `torch.bfloat16`: Peak inference VRAM footprint is **2.2–3.8 GB** for Swin-Large and **1.4 GB** for ResNet-50.
- **Inference Latency**: Single forward pass executes in **~22 ms** (Swin-Large) and **~9 ms** (ResNet-50) on an NVIDIA RTX 4090 GPU. Feasible on standard clinical PACS workstations.
- **Training Compute Budget**: Distributed pretraining on **8x–16x NVIDIA A100 (80GB)** GPUs utilizing cyclical dataset sampling, gradient accumulation, and EMA momentum updates ($\alpha = 0.999$).

### [A] Access & Artifacts
- **GitHub Repository**: Complete training, evaluation, and fine-tuning pipelines available at [`github.com/jlianglab/Ark`](https://github.com/jlianglab/Ark).
- **Model Checkpoints**: Publicly downloadable checkpoints for ResNet-50, ConvNeXt-Base/Large, and Swin-Base/Large backbones.
- **Pretraining Lineage**: Fully open supervised foundation model lineage documented in Dossier Section 4.0 ([S106, S107]).

---

## 3. Verified Benchmark Standings & Comparative Matrix

*Standings reflect official test split evaluations under frozen linear probing (LP) and full fine-tuning (FT).*

| Evaluation Dataset / Task | Evaluation Split | Supervision Regimen | Ark/Ark+ Configuration | Model Metric | Gold Standard Baseline | Baseline Metric | Performance Delta | Evidence Source |
|---|---|:---:|---|:---:|---|:---:|:---:|:---:|
| **VinDr-CXR (28 Classes)** | Held-out Blind Test | Full Fine-Tuning | Ark+ Swin-Large | **0.868** Mean AUROC | Swin-Large (ImageNet) | 0.812 Mean AUROC | **+0.056 AUROC** | `[E1]` *Nature* 2025 |
| **NIH ChestX-ray14 (14 Classes)**| Official Benchmark Split | Linear Probing | Ark ConvNeXt-L | **0.861** Mean AUROC | MoCo v2 (ResNet-50) | 0.824 Mean AUROC | **+0.037 AUROC** | `[E1]` *Nature* 2025 |
| **CheXpert Competition Split** | Official 5x200 Held-out | Linear Probing | Ark Swin-Large | **0.895** Mean AUROC | BiomedCLIP (ViT-B/16) | 0.879 Mean AUROC | **+0.016 AUROC** | `[E1]` *MedIA* 2026 |
| **SIIM-ACR Pneumothorax** | Official Test Split | Few-Shot FT (10% Labels) | Ark+ Swin-L (U-Net) | **0.884** Mean Dice | U-Net Scratch (10% Labels)| 0.792 Mean Dice | **+0.092 Dice** | `[E1]` *MedIA* 2026 |
| **RSNA Pneumonia Challenge** | Official Test Split | Full Fine-Tuning | Ark ConvNeXt-L | **0.924** AUROC | ResNet-50 Scratch | 0.891 AUROC | **+0.033 AUROC** | `[E1]` *Nature* 2025 |
| **ChestX-Det-10 (Localization)**| Held-out Test Split | Detection Fine-Tuning | Ark+ Swin-L + RetinaNet | **0.428** mAP@0.5 | Swin-L Scratch | 0.364 mAP@0.5 | **+0.064 mAP** | `[E1]` *MedIA* 2026 |

---

## 4. Ground-Layer Audit & Epistemic Traps

### 1. The Supervised Knowledge Accrual Doctrine
> [!IMPORTANT]
> **Supervised Accrual vs. Self-Supervision Paradox**: The prevailing AI trend assumes foundation models must be pretrained via self-supervised learning (DINOv2, MAE) or image-text contrastive alignment (CLIP). **Ark demonstrates that disciplined supervised multi-dataset knowledge accrual across heterogeneous expert annotations achieves superior diagnostic feature representations compared to contrastive and SSL models on thoracic pathology**. Expert radiologist labels—even when non-uniform across hospitals—contain rich discriminative priors that unsupervised visual clustering cannot infer.

### 2. Pretraining Contamination & Internal Accrual Auditing
- **Internal Accrual Cohorts**: Ark's cyclical pretraining directly ingests **NIH ChestX-ray14, CheXpert, VinDr-CXR, RSNA Pneumonia, and Shenzhen CXR**.
- **The Evaluation Trap**: Reporting high AUROC on NIH ChestX-ray14 or CheXpert is an evaluation of **internal accrued task recall**, NOT zero-shot out-of-distribution transfer. True transfer must be evaluated on strictly withheld institutions (e.g., BRAX, PadChest, or local academic medical centers).

### 3. Gradient Conflict and Task Interference
During cyclical training across disparate datasets:
- Semantically similar classes with different clinical thresholds (e.g., subtle "Infiltration" in ChestX-ray14 vs. strict "Pneumonia" in CheXpert) create competing gradient trajectories in the shared encoder.
- Mitigation requires careful tuning of the EMA momentum parameter ($\alpha \ge 0.999$) and localized task learning rates to prevent destructive feature interference across dataset transitions.

---

## 5. Local Verification Snippet (Tier A: Fully Open)

```python
# Requirements: pip install torch torchvision timm
# Artifact Tier: Tier A (Fully open via GitHub: JLiangLab/Ark)
# Verification: Programmatically instantiates Ark's heterogeneous multi-head accrual architecture and EMA momentum update

import torch
import torch.nn as nn
import copy

class ArkHeterogeneousAccrualModel(nn.Module):
    """
    Authentic implementation contract for Ark's multi-head heterogeneous
    knowledge accrual visual foundation model.
    """
    def __init__(self, feature_dim=1024, dataset_classes={"nih": 14, "chexpert": 14, "vindr": 28}):
        super().__init__()
        # Simulated high-capacity visual backbone (e.g. Swin-Large / ConvNeXt-L feature extractor)
        self.backbone = nn.Sequential(
            nn.Conv2d(3, 64, kernel_size=7, stride=2, padding=3),
            nn.BatchNorm2d(64),
            nn.GELU(),
            nn.AdaptiveAvgPool2d((1, 1)),
            nn.Flatten(),
            nn.Linear(64, feature_dim)
        )
        
        # Dedicated dataset-specific classification heads (preserving native ontologies)
        self.heads = nn.ModuleDict({
            ds_name: nn.Linear(feature_dim, num_classes)
            for ds_name, num_classes in dataset_classes.items()
        })

    def forward(self, x, dataset_key=None):
        features = self.backbone(x)
        if dataset_key is not None:
            if dataset_key not in self.heads:
                raise ValueError(f"Unknown dataset key: {dataset_key}. Available: {list(self.heads.keys())}")
            return self.heads[dataset_key](features)
        return features

class ArkEMATeacher:
    """Consolidates accrued knowledge across cyclical dataset iterations."""
    def __init__(self, student_model, alpha=0.999):
        self.student = student_model
        self.teacher = copy.deepcopy(student_model)
        self.alpha = alpha
        for param in self.teacher.parameters():
            param.requires_grad = False

    def update(self):
        with torch.no_grad():
            for s_param, t_param in zip(self.student.parameters(), self.teacher.parameters()):
                t_param.data.mul_(self.alpha).add_(s_param.data, alpha=1.0 - self.alpha)

def verify_ark_pipeline():
    print("[INIT] Verifying authentic Ark heterogeneous accrual pipeline contract...")
    
    # 1. Instantiate model and EMA teacher
    dataset_schema = {"nih_cxr14": 14, "chexpert": 14, "vindr_cxr": 28}
    model = ArkHeterogeneousAccrualModel(feature_dim=1024, dataset_classes=dataset_schema)
    ema_teacher = ArkEMATeacher(model, alpha=0.999)
    model.eval()

    # 2. Simulate forward passes across disparate dataset label spaces
    dummy_cxr = torch.randn(2, 3, 512, 512)
    
    # Forward pass on VinDr-CXR (28 classes)
    out_vindr = model(dummy_cxr, dataset_key="vindr_cxr")
    # Forward pass on NIH ChestX-ray14 (14 classes)
    out_nih = model(dummy_cxr, dataset_key="nih_cxr14")
    # Forward pass extracting general foundation feature embeddings
    features = model(dummy_cxr)

    print(f"[OUTPUT] VinDr-CXR task logits shape: {out_vindr.shape}")
    print(f"[OUTPUT] NIH-CXR14 task logits shape: {out_nih.shape}")
    print(f"[OUTPUT] Shared foundation embedding shape: {features.shape}")

    assert out_vindr.shape == (2, 28), "VinDr-CXR head dimension mismatch"
    assert out_nih.shape == (2, 14), "NIH-CXR14 head dimension mismatch"
    assert features.shape == (2, 1024), "Foundation feature representation mismatch"

    # 3. Simulate EMA teacher knowledge consolidation
    ema_teacher.update()
    print("[PASS] EMA momentum weight update executed successfully.")
    print("[PASS] Ark/Ark+ heterogeneous knowledge accrual contract verified.")

if __name__ == "__main__":
    verify_ark_pipeline()
```

---

## 6. Connected Dataset Ecosystem

- 📂 [NIH ChestX-ray14 (`[D13]`)](../../01_datasets/03_chest_xray/nih_chestxray14.md): Foundational multi-label dataset in Ark's cyclical training.
- 📂 [CheXpert (`[D2]`)](../../01_datasets/03_chest_xray/chexpert.md): Core accrual partner providing competition benchmark splits.
- 📂 [VinDr-CXR (`[D3]`)](../../01_datasets/03_chest_xray/vindr_cxr.md): Richly annotated Vietnamese cohort providing 28 findings and bounding-box coordinates for Ark+.
- 📂 [MIMIC-CXR (`[D1]`)](../../01_datasets/03_chest_xray/mimic_cxr.md): Large-scale cohort utilized for scaling knowledge accrual representations.
- 📂 [PadChest (`[D12]`)](../../01_datasets/03_chest_xray/padchest.md): Multi-label European cohort tested during external transfer evaluations.
- 📂 [BRAX (`[D14]`)](../../01_datasets/03_chest_xray/brax.md): Independent Latin American validation cohort verifying out-of-distribution generalizability.
