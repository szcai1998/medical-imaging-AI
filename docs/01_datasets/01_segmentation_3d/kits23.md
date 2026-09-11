# KiTS23: Kidney and Kidney Tumor Segmentation Challenge

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D9]`
- **Domain & Modality:** 3D Abdominal Contrast-Enhanced CT (CECT)
- **Target Anatomy & Pathologies:** Kidneys, Renal Masses (Tumours), and Renal Cysts
- **Release / Challenge Year:** 2023–2024 (MICCAI Challenge / Grand Challenge)
- **Evidence Code:** `E2` (Official Challenge Portal) + `E1` (Peer-Reviewed Challenge Proceedings)
- **Access Level:** Challenge-gated / Public Training Data with Held-out Test Labels
- **Primary Source / Portal:** [https://kits-challenge.org/kits23/](https://kits-challenge.org/kits23/) | Grand Challenge: [kits23.grand-challenge.org](https://kits23.grand-challenge.org/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Preoperative surgical planning for partial vs. radical nephrectomy requires precise 3D delineation of renal parenchyma, malignant neoplasms, and benign renal cysts to spare healthy nephrons and assess vascular involvement.
- **Target Classes:** 3 nested/semantic classes:
  1. *Kidney parenchyma*
  2. *Renal mass / tumor*
  3. *Renal cyst*

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **599 total patient CT examinations**
  - **489 training cases** (open image volumes + voxel ground-truth segmentations)
  - **110 held-out evaluation cases** (test images released, test labels withheld for blind benchmarking)
- **Imaging Physics & Acquisition Protocols:**
  - Multi-detector CT scans in late arterial / corticomedullary, nephrogenic, and excretory phases.
  - Highly variable slice thickness ($0.5\text{ mm}$ to $>5.0\text{ mm}$), creating severe z-axis anisotropy in routine clinical scans.
- **Multi-Center Distribution:** Multi-institutional cohort sourced from Fairview Health Services, University of Minnesota Medical Center, Cleveland Clinic, and international contributing hospitals.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Hierarchical, multi-stage manual delineation by trained medical students and research fellows, followed by rigorous review, correction, and adjudication by expert urologic oncologists and abdominal radiologists.
- **Label Provenance Category:** Native human expert consensus (verified ground truth; zero synthetic or model-hallucinated labels).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official distribution via Grand Challenge and Zenodo (`10.5281/zenodo.7998379`).
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0). Restricted to academic and non-commercial research.
- **Artifact Availability:** Preprocessed NIfTI files, raw DICOMs, evaluation scripts, and official baseline Docker containers.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Expanded from KiTS19 (210 training / 90 test cases) and KiTS21.
- **Derivative Ecosystem:** Sourced as a core constituent benchmark within multi-dataset aggregations, including CVPR-BiomedSegFM `[D1]` and FLARE PanCancerCTSeg `[D3]`.
- **Contamination & Overlap Warning:** Foundation models (e.g., MedSAM, VISTA3D, BiomedParse) pretrained on public biomedical CT corpora almost universally incorporate the 489 KiTS training cases. Zero-shot claims on KiTS must explicitly verify that pretraining excluded KiTS.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official KiTS23 110-case held-out test evaluation. Evaluated using rank-then-aggregate over Dice Similarity Coefficient (DSC) and Normalized Surface Dice (NSD). Sørensen-Dice on the Tumor region serves as the official tiebreaker.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Overall Mean Dice | Surface Dice (NSD) | Tumor Dice (Tiebreaker) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **Auto3DSeg Ensemble** | Andriy Myronenko et al. (NVIDIA) | 15-model ensemble combining SegResNet, DiNTS (NAS), and SwinUNETR with test-time augmentation | Official Blind Test Set ($N=110$) | **0.835** | **0.723** | **0.792** | [arXiv:2309.07348](https://arxiv.org/abs/2309.07348) |
| **2** | **Cascaded ResEnc-UNet** | Kwang-Hyun Uhm et al. (Korea University) | Two-stage coarse-to-fine residual encoder U-Net with anisotropic patch resampling | Official Blind Test Set ($N=110$) | **0.820** | **0.712** | **0.768** | [KiTS23 Proceedings](https://kits-challenge.org/kits23/) |
| **3** | **Boundary-Guided DynUNet** | Yasmeen George et al. (Monash University) | DynUNet with deep supervision, compound focal/top-k cross-entropy loss, and contour regularization | Official Blind Test Set ($N=110$) | **0.819** | **0.707** | **0.761** | [KiTS23 Proceedings](https://kits-challenge.org/kits23/) |
| **4** | **3D UX-Net + Anisotropic Attention** | Shuolin Liu et al. (Independent) | Large-kernel volumetric convolutional network with depthwise separable convolutions | Official Blind Test Set ($N=110$) | **0.812** | **0.698** | **0.754** | Grand Challenge Submission |
| **5** | **Standard nnU-Net v2 Baseline** | MIC-DKFZ / Challenge Organizers | Self-configuring 3D full-resolution nnU-Net v2 (5-fold cross-validation ensemble) | Official Blind Test Set ($N=110$) | **0.804** | **0.689** | **0.742** | [Nature Methods 18](https://doi.org/10.1038/s41592-020-01008-z) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full dataset size: ~110 GB uncompressed NIfTI files.
  - Baseline validation requires a minimum of **16 GB VRAM** (e.g., RTX 4080 / RTX 3090) due to 3D patch volumes ($128 \times 128 \times 128$).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel
  import nibabel as nib

  case_img = nib.load("kits23/dataset/case_00001/imaging.nii.gz")
  case_seg = nib.load("kits23/dataset/case_00001/segmentation.nii.gz")
  print(f"Shape: {case_img.shape}, Spacing: {case_img.header.get_zooms()}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Cyst vs. Necrotic Tumor:* Small benign cysts are easily conflated with hypodense necrotic portions of renal cell carcinoma.
  2. *Slice Thickness Discrepancies:* Thick-slice CT scans ($5\text{ mm}$) suffer from partial volume artifacts at the upper and lower renal poles.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages in `docs/02_models/` utilize KiTS23 as a core training or transfer benchmark:
- **nnU-Net v2** (`docs/02_models/01_segmentation/nnunet_v2.md`): Task-specific gold baseline.
- **VISTA3D / NV-Segment-CTMR** (`docs/02_models/01_segmentation/vista3d.md`): Multi-organ pretraining.
- **MedSAM2** (`docs/02_models/01_segmentation/medsam2.md`): Prompt-based 3D lesion propagation.
- **BiomedParse v2** (`docs/02_models/01_segmentation/biomedparse_v2.md`): Text-driven segmentation target.
