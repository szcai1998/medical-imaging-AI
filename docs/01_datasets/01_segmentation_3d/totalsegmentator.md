# TotalSegmentator: Robust Anatomical CT Segmentation

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D6]`
- **Domain & Modality:** 3D Whole-Body Computed Tomography (CT)
- **Target Anatomy:** 104 Anatomical Structures (v1) expanded to 117+ structures (v2) across organs, 24 vertebrae, ribs, cardiac chambers, muscles, and major vessels
- **Release / Year:** 2023 (v1, Radiology: AI) | 2024–2026 (v2 expansion & continuous tool updates)
- **Evidence Code:** `E1` (Peer-Reviewed Primary Version-of-Record in *Radiology: AI*) + `E2` (Official Toolkit/Model Release)
- **Access Level:** Open Source & Open Weights (Apache-2.0 / CC BY 4.0)
- **Primary Source / Portal:** [GitHub: wasserth/TotalSegmentator](https://github.com/wasserth/TotalSegmentator) | [Radiology: AI Paper](https://doi.org/10.1148/ryai.230024) | [Zenodo Archive](https://zenodo.org/records/6802614)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Comprehensive, automatic segmentation of entire CT examinations enables opportunistic disease screening (osteopenia, sarcopenia, vascular calcification), radiation oncology organ-at-risk (OAR) auto-contouring, and population-level phenomics.
- **Target Classes:** 104 structures in the canonical release, grouped into 27 organs, 59 bones (vertebrae, ribs, pelvic bones), 10 muscles, and 8 vascular vessels.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **1,204 total CT examinations** from 1,204 unique patients in the primary peer-reviewed dataset release.
  - Sourced from a wide distribution of body regions: whole-body, thorax-abdomen-pelvis (TAP), abdomen-pelvis, thorax, and head-neck scans.
- **Imaging Physics & Acquisition Protocols:**
  - Multi-vendor data (Siemens, GE, Philips, Canon) across 15+ scanner models.
  - Variable contrast phases (unenhanced, arterial, portal-venous, delayed) and diverse slice thicknesses ($0.6\text{ mm}$ to $5.0\text{ mm}$).
- **Multi-Center Distribution:** Sourced from routine clinical imaging archives at the University Hospital Basel, Switzerland.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Active-learning pseudo-labeling loop: an initial model trained on public datasets generated baseline masks, which were manually inspected and corrected by medical annotators. Iterative rounds of retraining and correction were conducted, culminating in quality control audits by senior board-certified radiologists.
- **Label Provenance Category:** Iterative model-assisted human radiologist consensus.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Zenodo (`10.5281/zenodo.6802614`) and Hugging Face; ready-to-run Python CLI (`pip install TotalSegmentator`).
- **License / Terms:** Apache-2.0 for code and toolkit; CC BY 4.0 for dataset weights and images. Free for academic and commercial use.
- **Artifact Availability:** Pre-trained model weights, complete NIfTI image-mask pairs, and modular body-part task heads.

### [S] Systemic Lineage & Genealogy
- **Architecture Lineage:** Built directly on top of the self-configuring **nnU-Net v2** framework using a multi-resolution cascade (coarse $3\text{ mm}$ locator $\rightarrow$ fine $1.5\text{ mm}$ full-resolution segmenter).
- **Contamination Warning:** The 1,204 TotalSegmentator CT scans are frequently incorporated into subsequent foundation model pretraining (e.g., MedSAM, VISTA3D, BiomedParse). Evaluating any model on TotalSegmentator must distinguish between zero-shot out-of-domain testing and in-corpus evaluation.

---

## 3. Verified SOTA Benchmarks & Comparators
*Comparative performance across major anatomical regions on the held-out TotalSegmentator test set ($N=168$), evaluated by mean Dice Similarity Coefficient (DSC).*

| Rank | Model / Framework | Backbone Architecture | Evaluation Split & Setting | Overall Mean DSC | Solid Organs DSC | Bone / Vertebrae DSC | Vessels & Muscles DSC | Reference |
| :---: | :--- | :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **1** | **TotalSegmentator v2** | nnU-Net v2 (Cascade + Sub-task Heads) | Independent Held-out Test Split ($N=168$) | **0.943** | **0.958** | **0.952** | **0.912** | [GitHub / Wasserthal 2024](https://github.com/wasserth/TotalSegmentator) |
| **2** | **TotalSegmentator v1 (Original)** | nnU-Net (3D fullres + 3mm coarse) | Independent Held-out Test Split ($N=168$) | **0.916** | **0.940** | **0.925** | **0.884** | [Radiology: AI 2023](https://doi.org/10.1148/ryai.230024) |
| **3** | **VISTA3D / NV-Segment-CT** | SegResNet-based Foundation Model | Zero-Shot Evaluation on Test Split | **0.908** | **0.932** | **0.918** | **0.875** | [CVPR 2025 / MONAI](https://github.com/Project-MONAI/VISTA) |
| **4** | **MOOSE (Multi-Organ Open Seg)** | Coarse-to-fine CNN Pipeline | Test Set Benchmark Comparison | **0.887** | **0.915** | **0.894** | **0.849** | [J. Med. Imaging 2023](https://doi.org/10.1117/1.JMI.10.1.015003) |
| **5** | **SwinUNETR Benchmark** | Hierarchical Vision Transformer | Retrained Baseline on TS Split | **0.881** | **0.912** | **0.889** | **0.838** | Comparative Studies [arXiv:2308.15175](https://arxiv.org/abs/2308.15175) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Fast inference via CLI: `TotalSegmentator -i ct.nii.gz -o segmentations/ --fast` runs at $3\text{ mm}$ isotropic in $<10\text{ seconds}$ on an 8 GB GPU.
  - Full-resolution inference ($1.5\text{ mm}$) requires **8 GB to 12 GB VRAM** and takes ~1 minute per scan.
- **Minimal Local Verification / Load Command:**
  ```bash
  # Requirements: pip install TotalSegmentator
  TotalSegmentator -i /path/to/ct_scan.nii.gz -o /path/to/output_dir --roi_subset spleen liver
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Version Discrepancy:* Always record whether v1 (104 classes) or v2 (117+ classes) is being referenced; v2 includes separate pulmonary vein/artery classes not present in v1.
  2. *Severe Pathology / Metal Implants:* Streaking artifacts from hip prostheses or spinal instrumentation cause local mask dropout in adjacent pelvic muscles and vertebrae.

---

## 5. Downstream Foundation Model Consumers
The following models build directly upon or benchmark against TotalSegmentator:
- **nnU-Net v2** (`docs/02_models/01_segmentation/nnunet_v2.md`): Native backbone architecture.
- **VISTA3D** (`docs/02_models/01_segmentation/vista3d.md`): Benchmark comparison for organ class coverage.
- **MedSAM2** (`docs/02_models/01_segmentation/medsam2.md`): Pretraining and interactive segmentation evaluation.
