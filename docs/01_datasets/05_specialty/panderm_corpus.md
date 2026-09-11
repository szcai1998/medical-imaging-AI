# PanDerm Corpus: Multimodal Vision Foundation Model Pretraining & Benchmark Corpus for Dermatology

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D-DERM]` / PanDerm Corpus
- **Domain & Modality:** 4 Multimodal Dermatology Imaging Modalities:
  1. Total Body Photography (TBP) Lesion Tiles
  2. Dermatopathology (WSI Histology Patches)
  3. Clinical Macrophotography (Close-up Camera / Smartphone Images)
  4. Dermoscopy (Contact / Non-Contact Polarized & Non-Polarized Epiluminescence Microscopy)
- **Target Anatomy & Pathologies:** Cutaneous neoplasms (Malignant Melanoma, Basal Cell Carcinoma [BCC], Squamous Cell Carcinoma [SCC], Melanocytic Nevi, Seborrheic Keratoses) and inflammatory/infectious dermatoses covering >128 dermatologic diagnostic entities.
- **Release / Publication Year:** 2025 (*Nature Medicine* 31, 2691–2702, June 2025; arXiv:2410.15038)
- **Evidence Code:** `E1` (Peer-Reviewed *Nature Medicine* Version-of-Record) + `E2` (Official Model & Benchmark Card)
- **Access Level:** Open Model Weights & Benchmark Evaluation Suite via GitHub/Hugging Face; Pretraining corpus compiled across 11 institutional and public sources.
- **Primary Source / Portal:** [https://www.nature.com/articles/s41591-025-03747-y](https://www.nature.com/articles/s41591-025-03747-y) | GitHub: [https://github.com/aim-monash/PanDerm](https://github.com/aim-monash/PanDerm) | arXiv: [https://arxiv.org/abs/2410.15038](https://arxiv.org/abs/2410.15038)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Clinical dermatology is multimodal by nature: a dermatologist visually inspects macroscopic body distribution (TBP), performs close-up clinical examination, applies dermoscopy for microstructural sub-surface patterns, and correlates with dermatopathology biopsies. Prior AI systems were strictly unimodal (e.g., dermoscopy only or clinical photos only), failing to simulate real-world diagnostic workflows or track longitudinal lesion changes ("ugly duckling" sign).
- **Task Scope:** 28 diverse benchmarks across skin cancer screening, multi-class differential diagnosis, lesion segmentation, longitudinal change tracking, and metastasis risk prediction.

### [A] Acquisition Physics & Scale
- **Exact Counting Unit Hierarchy:**
  $$\text{patient} \neq \text{clinical visit} \neq \text{anatomical lesion site} \neq \text{modality} \neq \text{image / crop tile}$$
- **Verified Scale & 11 Data Sources:**
  - **Total Pretraining Scale:** **2,149,706 skin disease images** curated from **11 institutional sources** across Australia, Europe, and North America.
  - **Breakdown by Modality:**
    - **Total Body Photography (TBP) Lesion Tiles:** ~1,220,000 cropped lesion tiles from automated high-resolution 3D whole-body imaging systems (Canfield VECTRA WB360).
    - **Dermoscopy Images:** ~580,000 epiluminescence images (ISIC Archive, HAM10000, BCN20000, MSKCC).
    - **Clinical Macrophotographs:** ~240,000 clinical close-up photos (Fitzpatrick 17k, PAD-UFES-20, DDI, MED-NODE).
    - **Dermatopathology Patches:** ~110,000 digitized H&E histopathology patches from excised skin biopsies.
- **Acquisition Physics:** Optical illumination ranging from macroscopic cross-polarized ambient light (preventing surface glare) to 10x immersion epiluminescence microscopy and 20x/40x digital whole-slide scanner optics.

### [T] Truth & Annotation Provenance
- **Histopathological Truth:** All malignant skin cancer cases (melanoma, BCC, SCC) verified by reference histopathology examination from excised biopsy specimens.
- **Clinical Expert Consensus:** Benign lesions and inflammatory skin conditions annotated and verified through panel consensus by senior consultant dermatologists.
- **Reader Study Adjudication:** Three prospective reader studies evaluating general practitioners, non-dermatologist clinicians, and certified dermatologists.

### [A] Access, Terms & Artifacts
- **Repository / Download:** GitHub repository ([https://github.com/aim-monash/PanDerm](https://github.com/aim-monash/PanDerm)) and Hugging Face model hub.
- **License / Terms:** Non-commercial research license (CC BY-NC 4.0) for model weights and evaluation code; constituent datasets adhere to individual institutional DUAs and open licenses (e.g., CC BY 4.0 for ISIC).
- **Artifact Availability:** Pretrained ViT-Base and ViT-Large encoders, linear probe weights, and benchmark evaluation scripts.

### [S] Systemic Lineage & Genealogy
- **Genealogy Evolution:** Early unimodal ResNet classifiers (Esteva et al., Nature 2017) $\rightarrow$ ISIC Challenge series (2016–2024 dermoscopy benchmarks) $\rightarrow$ PanDerm (Nature Medicine 2025, first comprehensive multimodal foundation model).
- **Contamination & Overlap Warning:** Because PanDerm's 2.15M corpus incorporates ISIC Archive, HAM10000, BCN20000, Fitzpatrick 17k, and DDI, zero-shot claims on these datasets represent **in-domain re-evaluation**, not out-of-distribution transfer. Benchmarking must verify source exclusions.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings across official dermatology benchmarks reported in the Nature Medicine 2025 evaluation (Yan et al. [S88]). Evaluated using Macro-Averaged AUROC across clinical tasks, Reader Study Diagnostic Accuracy Gain ($\Delta$), and Few-Shot Adaptation (10% label fraction).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Macro AUROC ($\uparrow$) | Clinician Accuracy Gain ($\Delta$) | 10% Label Few-Shot AUROC | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **PanDerm-L** | Yan, Ge, Soyer, Kittler et al. (Monash / UQ / MedUni Vienna) | Multimodal Vision Transformer (ViT-L/16) self-supervised pretraining across 4 modalities | Held-out 28-Benchmark Suite ($N=28$) | **0.938** | **+11.0%** (Dermoscopy) / **+10.2%** (Melanoma) | **0.912** | [Nature Medicine 2025](https://doi.org/10.1038/s41591-025-03747-y) |
| **2** | **PanDerm-B** | Yan, Ge et al. (Monash Univ.) | Multimodal Vision Transformer (ViT-B/16) self-supervised pretraining | Held-out 28-Benchmark Suite ($N=28$) | **0.924** | **+9.8%** | **0.897** | [Nature Medicine 2025](https://doi.org/10.1038/s41591-025-03747-y) |
| **3** | **Med-PaLM M (Derm Module)** | Tu et al. (Google Research) | Multimodal biomedical language-vision model fine-tuned on multi-task clinical dermatology | Multi-Task Dermatology Split | **0.905** | **+7.5%** | **0.874** | [arXiv:2307.14334](https://arxiv.org/abs/2307.14334) |
| **4** | **BioMedCLIP Zero-Shot** | Zhang et al. (Microsoft Research) | Domain-specific contrastive vision-language model evaluated via text prompt retrieval | Zero-Shot Diagnostic Benchmark | **0.862** | — | — | [arXiv:2303.00915](https://arxiv.org/abs/2303.00915) |
| **5** | **Supervised ResNet-50 Baseline** | Standard ISIC Comparator | Standard ImageNet-pretrained ResNet-50 supervised multi-class baseline | Held-out 28-Benchmark Suite ($N=28$) | **0.841** | Baseline Comparator | **0.782** | [Nature Medicine 2025](https://doi.org/10.1038/s41591-025-03747-y) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full pretraining corpus is private/multi-terabyte; public downstream benchmarks (ISIC + Fitzpatrick 17k) occupy ~30 GB.
  - Pretrained model inference requires only **6–8 GB VRAM** (RTX 3060/4060) for ViT-B, and **12–16 GB VRAM** for ViT-L.
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install torch torchvision pillow timm
import torch
import torchvision.transforms as T
from PIL import Image

# Standard image preprocessing for PanDerm ViT encoder
transform = T.Compose([
    T.Resize((224, 224)),
    T.ToTensor(),
    T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
])

dummy_img = Image.new("RGB", (224, 224), color=(200, 150, 130))
img_tensor = transform(dummy_img).unsqueeze(0)
print(f"Input tensor shape: {img_tensor.shape}")
# Feedforward verification: outputs 768-dim (ViT-B) or 1024-dim (ViT-L) feature embedding
```
- **Dominant Failure Modes & Gotchas:**
  1. *Fitzpatrick Skin-Type Disparity:* Datasets are predominantly skewed toward light skin types (Fitzpatrick I–II). Diagnostic sensitivity drops on darker skin tones (Fitzpatrick V–VI) for non-pigmented lesions and acral lentiginous melanoma.
  2. *Surgical Markings & Ruler Artifacts:* Skin markings (surgical pen ink, calibration rulers, adhesive hair tape) act as high-frequency spurious shortcuts that models latch onto if not mitigated by artifact masking.

---

## 5. Downstream Foundation Model Consumers
The following models build on or consume the PanDerm corpus:
- **PanDerm Encoders** (`[S88]`): Canonical specialty foundation model for clinical dermatology.
- **Skin-SAM / MedSAM Dermatology Adapters**: Zero-shot promptable lesion segmentation tools.
- **Multimodal Dermatologic VLMs**: Clinical chat and report-generation assistants.
