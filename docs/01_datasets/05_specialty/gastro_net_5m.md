# GastroNet-5M: Multicenter Endoscopy Video Frame Pretraining Corpus

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D-ENDO]` / GastroNet-5M
- **Domain & Modality:** High-Definition White-Light Endoscopy (WLE) and Narrow-Band Imaging (NBI) Video Frames (Upper GI: Gastroscopy / EGD; Lower GI: Colonoscopy)
- **Target Anatomy & Pathologies:** Gastrointestinal Tract (Esophagus, Stomach, Duodenum, Colon, Rectum). Pathologies: Colorectal adenomatous polyps, Barrett's esophagus with low/high-grade dysplasia, early gastric cancer invasion depth, and vascular angiodysplasia.
- **Release / Publication Year:** 2025–2026 (*Gastroenterology* 170(1), 174–187, Jan 2026)
- **Evidence Code:** `E1` (Peer-Reviewed *Gastroenterology* Version-of-Record) + `E2` (Official Model & Dataset Portal)
- **Access Level:** Open Model Weights via Hugging Face; Research DUA for underlying multicenter video frame archive via ThetaVision.
- **Primary Source / Portal:** [https://doi.org/10.1053/j.gastro.2025.08.012](https://doi.org/10.1053/j.gastro.2025.08.012) | Hugging Face: [https://huggingface.co/thetavision](https://huggingface.co/thetavision) | ThetaVision: [https://thetavision.nl/](https://thetavision.nl/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Screening colonoscopy and upper endoscopy are the gold standards for gastrointestinal cancer prevention, yet human adenoma miss rates range from 15% to 25% due to visual fatigue, subtle flat/sessile morphology (Paris class IIa/IIb/IIc), and rapid scope withdrawal. Prior deep learning models trained on small single-center datasets suffered from poor cross-center generalizability and high sensitivity to endoscope manufacturer differences (Olympus vs. Fujifilm vs. Pentax).
- **Task Scope:** Self-supervised pretraining for downstream computer-aided polyp detection (CADe), optical biopsy polyp characterization (CADx), and semantic lesion segmentation.

### [A] Acquisition Physics & Scale
- **Exact Counting Unit Hierarchy:**
  $$\text{patient} \neq \text{endoscopic procedure / encounter} \neq \text{video stream} \neq \text{video frame / still crop}$$
- **Verified Scale:**
  - **4,820,653 unlabeled endoscopic images / video frames**.
  - Sourced from **approximately 500,000 unique endoscopic procedures**.
  - Collected across **8 distinct Dutch hospitals and academic medical centers** over an 8-year span (2012–2020).
- **Imaging Physics & Modality Characteristics:**
  - Acquired using high-definition video endoscopes with CCD/CMOS optical tip sensors.
  - Modalities include standard White-Light Endoscopy (WLE) and optical chromoendoscopy (Narrow-Band Imaging [NBI] utilizing narrow blue $415\text{ nm}$ and green $540\text{ nm}$ illumination to enhance mucosal and vascular surface patterns).
  - High intra-procedure variability: Lens fluid droplets, peristaltic motion blur, specular glare highlights, and endoscopic tool occlusions (biopsy forceps, snare loops).

### [T] Truth & Annotation Provenance
- **Self-Supervised Pretraining:** Pure self-supervised feature learning (DINO, SimCLRv2, MoCov2) operating on 4.82M unlabeled frames without synthetic or human label hallucination.
- **Downstream Benchmark Ground Truth:** Fine-tuning and testing conducted on histopathology-verified downstream reference datasets (Kvasir-SEG, HyperKvasir, CVC-ClinicDB, ETIS-Larib) with lesion borders delineated by expert gastroenterologists.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hugging Face Model Hub (`thetavision/gastronet-5m-dino`).
- **License / Terms:** Open academic research license for foundation weights; multi-center frame repository governed by hospital research agreements.
- **Artifact Availability:** Pretrained ResNet-50, ViT-Small/16, and ViT-Base/16 checkpoints, along with fine-tuning heads for polyp segmentation and classification.

### [S] Systemic Lineage & Genealogy
- **Genealogy Evolution:** Single-center public benchmarks (Kvasir, CVC-ClinicDB, Nerthus) $\rightarrow$ GastroNet-5M (first multicenter 5M-frame pretraining foundation resource).
- **Contamination & Overlap Warning:**
  - *Procedure-Level Temporal Leakage:* Endoscopic video frames sampled seconds apart within the same procedure are virtually identical. Splitting frames from the same procedure across training and testing sets produces near-100% artificial accuracy. Splits must be strictly clustered at the procedure or patient level.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on downstream gastrointestinal polyp segmentation benchmarks (Kvasir-SEG and CVC-ClinicDB Mean Dice Similarity Coefficient, Gastroenterology 2026 [S51]). Evaluated using Dice Similarity Coefficient (DSC), Mean IoU, and Performance Gain over ImageNet pretraining.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Polyp Mean Dice ($\uparrow$) | Kvasir-SEG Dice | ImageNet Delta ($\Delta$) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **GastroNet-5M ViT-B (DINO)** | ThetaVision / Dutch Multi-Hospital Consortium | Vision Transformer (ViT-B/16) pretrained on 4.82M frames via DINO self-distillation | Standard Downstream Benchmark Split | **0.918** | **0.924** | **+4.62%** | [Gastroenterology 2026](https://doi.org/10.1053/j.gastro.2025.08.012) |
| **2** | **GastroNet-5M ResNet-50 (DINO)** | ThetaVision Consortium | Standard ResNet-50 backbone pretrained on 4.82M frames via DINO | Standard Downstream Benchmark Split | **0.896** | **0.902** | **+1.63%** | [Gastroenterology 2026](https://doi.org/10.1053/j.gastro.2025.08.012) |
| **3** | **Polyp-PVT** | Dong et al. | Pyramid Vision Transformer with cascaded dilated convolutional attention | Standard Downstream Benchmark Split | **0.884** | **0.891** | Baseline Comparator | [MICCAI 2021](https://arxiv.org/abs/2108.09038) |
| **4** | **PraNet** | Fan et al. | Parallel Reverse Attention Network for polyp area mining | Standard Downstream Benchmark Split | **0.871** | **0.880** | Baseline Comparator | [MICCAI 2020](https://arxiv.org/abs/2006.11392) |
| **5** | **Supervised UNet++ (ImageNet)** | Standard Biomedical Baseline | Nested U-Net with ResNet-50 encoder initialized from standard ImageNet-1k weights | Standard Downstream Benchmark Split | **0.821** | **0.835** | Reference Anchor | [IEEE TMI 2019](https://doi.org/10.1109/TMI.2019.2959609) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Pretrained foundation model weights occupy <500 MB per checkpoint.
  - Downstream fine-tuning or inference runs comfortably on **6–8 GB VRAM** (RTX 3060 / RTX 4060).
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install torch torchvision pillow
import torch
import torchvision.transforms as T
from PIL import Image

# Image pipeline for GastroNet-5M endoscopy frames
preprocess = T.Compose([
    T.Resize((224, 224)),
    T.ToTensor(),
    T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
])

dummy_frame = Image.new("RGB", (224, 224), color=(180, 80, 70))
input_tensor = preprocess(dummy_frame).unsqueeze(0)
print(f"Endoscopy input frame tensor shape: {input_tensor.shape}")

# Models can be loaded directly from Hugging Face:
# model = torch.hub.load('facebookresearch/dino:main', 'dino_vits16')
```
- **Dominant Failure Modes & Gotchas:**
  1. *Specular Glare False Positives:* Bright specular reflections from moist mucosal surfaces trigger false polyp detections if not mitigated by specularity suppression.
  2. *Inadequate Bowel Preparation:* Residual stool particles and bubbles mimic diminutive sessile serrated lesions, producing high false-alarm rates during withdrawal.

---

## 5. Downstream Foundation Model Consumers
The following clinical systems and models build on GastroNet-5M:
- **GastroNet Foundation Encoders** (`[S51]`): Pretrained feature representations for GI endoscopy.
- **EndoFM / EndoViT**: Real-time spatiotemporal models for video colonoscopy.
- **Commercial CADe/CADx Suites**: Real-time bounding-box guidance systems for endoscopy suites.
