# AMOS22: Multi-Modality Abdominal Multi-Organ Segmentation

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D10]`
- **Domain & Modality:** 3D Abdominal Computed Tomography (CT) & Magnetic Resonance Imaging (MRI)
- **Target Anatomy:** 15 Abdominal Organs (Spleen, Right Kidney, Left Kidney, Gallbladder, Esophagus, Liver, Stomach, Aorta, IVC, Pancreas, Right Adrenal Gland, Left Adrenal Gland, Duodenum, Bladder, Prostate/Uterus)
- **Release / Challenge Year:** 2022–2023 (MICCAI Challenge / Grand Challenge)
- **Access Level:** Public Research Access / Official Grand Challenge Test Portal
- **Primary Source / Portal:** [https://amos22.grand-challenge.org/](https://amos22.grand-challenge.org/) | [Zenodo Repository](https://zenodo.org/records/7155725)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Comprehensive multi-organ delineation is critical for radiotherapy dose planning, computer-assisted abdominal surgery, and automated opportunistic screening across both CT and MRI modalities without retraining modality-isolated models.
- **Evaluation Tasks:**
  - **Task 1:** Single-modality abdominal organ segmentation (CT only).
  - **Task 2:** Cross-modality abdominal organ segmentation (single unified model evaluated jointly on CT and MRI).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **600 multi-modality 3D abdominal volumes from 600 unique patients**
  - **500 CT scans** (300 training, 100 validation, 100 blind test)
  - **100 MRI scans** (40 training, 20 validation, 40 blind test)
- **Imaging Physics & Acquisition Protocols:**
  - CT: Contrast-enhanced and unenhanced abdominal CT acquired across multiple scanner models (GE, Siemens, Philips) with variable slice thicknesses ($1.0\text{ mm} - 5.0\text{ mm}$).
  - MRI: Multi-sequence 3D T1-weighted (in-phase, out-phase, and contrast-enhanced) and T2-weighted abdominal MRI sequences.
- **Multi-Center Distribution:** Sourced from two premier hospitals in China across 8 distinct scanner systems.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Initial deep learning pseudo-labeling followed by multi-round voxel-level corrections by junior radiologists, refined by two senior abdominal radiologists with over 10 years of clinical experience.
- **Label Provenance Category:** Semi-automated initialization with rigorous human radiologist adjudication.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via Zenodo and Grand Challenge.
- **License / Terms:** CC BY-NC-SA 4.0; research and academic benchmarking only.
- **Artifact Availability:** Fully preprocessed NIfTI images, 15-class integer mask labels, and official evaluation scripts.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Inspired by BTCV (Beyond the Cranial Vault) and FLARE challenges, but expanding anatomical classes from 13 to 15 and introducing paired MRI validation.
- **Contamination & Overlap Warning:** Ubiquitous in 2024–2026 foundation model training (e.g., VISTA3D, MedSAM2, NV-Segment-CTMR). Any zero-shot evaluation must verify whether AMOS training cases were in the pretraining mix.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official AMOS22 Test Set across 15 abdominal organs, evaluated by Dice Similarity Coefficient (DSC) and Normalized Surface Dice (NSD).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Task 1 (CT) Mean DSC | Task 2 (CT+MRI) Mean DSC | Reference / Links |
| :---: | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Metric-Aware nnU-Net Ensemble** | Fabian Isensee et al. (MIC-DKFZ) | Self-configuring nnU-Net v2 with metric-specific validation tuning, heavy spatial augmentations, and residual encoders | **0.908** | **0.891** | [Grand Challenge / DKFZ](https://arxiv.org/abs/2211.04253) |
| **2** | **Auto3DSeg Multi-Backbone** | Dong Yang, Andriy Myronenko (NVIDIA) | Blended ensemble of SegResNet, DiNTS, and SwinUNETR with adaptive spacing normalization | **0.902** | **0.884** | [arXiv:2210.15859](https://arxiv.org/abs/2210.15859) |
| **3** | **CoTr-v2 Hybrid Transformer** | Yutong Xie et al. (University of Adelaide) | CNN-Transformer hybrid utilizing deformable self-attention to capture multi-scale organ context | **0.893** | **0.875** | [MICCAI AMOS22](https://doi.org/10.1007/978-3-031-18500-7) |
| **4** | **SwinUNETR + Modality Token** | Yucheng Tang et al. (Vanderbilt / NVIDIA) | Hierarchical Swin Transformer encoder with modality-conditioning tokens for joint CT/MRI processing | **0.889** | **0.871** | [CVPR 2022](https://arxiv.org/abs/2201.01266) |
| **5** | **Standard 3D nnU-Net Baseline** | Challenge Organizers (Ji et al.) | Vanilla 3D full-resolution nnU-Net without post-processing or ensembling | **0.880** | **0.865** | [Medical Image Analysis 84](https://doi.org/10.1016/j.media.2022.102711) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Storage: ~45 GB compressed NIfTI.
  - VRAM Requirement: **16 GB to 24 GB** (RTX 3090 / 4090) required for training 3D patches at $192 \times 192 \times 64$ voxel resolution.
- **Minimal Local Verification / Load Command:**
  ```python
  import nibabel as nib
  ct_img = nib.load("amos22/imagesTr/amos_0001.nii.gz")
  ct_lbl = nib.load("amos22/labelsTr/amos_0001.nii.gz")
  print(f"Modality: CT, Voxels: {ct_img.shape}, Organs present: {len(set(ct_lbl.get_fdata().flat)) - 1}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Small Organ Collapse:* Right and left adrenal glands and the duodenum show the lowest Dice scores ($<0.75$) due to low contrast, small volume, and surrounding retroperitoneal fat variability.
  2. *Cross-Modality Intensity Inversion:* MRI intensities are qualitative (non-Hounsfield), leading models to fail on bone boundaries and gas-tissue interfaces unless separate intensity normalization pipelines are applied.
