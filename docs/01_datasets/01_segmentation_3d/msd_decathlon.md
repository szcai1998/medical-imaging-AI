# Medical Segmentation Decathlon (MSD)

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D7]`
- **Domain & Modality:** Multi-Modality 3D CT & Multi-Sequence 3D MRI
- **Target Anatomy & Pathologies:** 10 Diverse Heterogeneous Anatomical & Oncological Segmentation Tasks
- **Release / Publication:** 2018 (Challenge) | 2022 (Nature Communications 13:4128)
- **Evidence Code:** `E1` (Peer-Reviewed Benchmark Paper in *Nature Communications*) + `E2` (Official Challenge Portal)
- **Access Level:** Open Benchmark under Creative Commons CC BY-SA 4.0
- **Primary Source / Portal:** [medicaldecathlon.com](http://medicaldecathlon.com/) | [Nature Comms Paper](https://doi.org/10.1038/s41467-022-30695-9) | [TCIA Collection](https://doi.org/10.5281/zenodo.3358329)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Designed to penalize hyper-tuned, single-task solutions and reward algorithm generalizability across varied human anatomy, image modalities, resolution scales, and pathological targets.
- **The 10 Canonical Tasks:**
  1. *Task01_BrainTumour:* Glioblastoma sub-regions (edema, enhancing tumor, necrotic core) on 4-sequence MRI.
  2. *Task02_Heart:* Left atrium segmentation on mono-modal MRI.
  3. *Task03_Liver:* Primary liver tumors and hepatic parenchyma on contrast CT.
  4. *Task04_Hippocampus:* Anterior and posterior hippocampus on high-resolution head MRI.
  5. *Task05_Prostate:* Peripheral and transition zones on multi-parametric MRI (T2, ADC).
  6. *Task06_Lung:* Pulmonary lesions on thoracic CT.
  7. *Task07_Pancreas:* Pancreatic parenchyma and pancreatic ductal adenocarcinoma on CT.
  8. *Task08_HepaticVessel:* Hepatic vascular tree and secondary metastases on CT.
  9. *Task09_Spleen:* Normal and abnormal spleen on abdominal CT.
  10. *Task10_Colon:* Primary colon cancer lesions on abdominal CT.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **2,633 total 3D examinations** across the 10 datasets.
  - Sized from small datasets (Hippocampus: 260 cases) to large cohorts (Liver: 201 cases; Lung: 64 cases; Brain: 484 cases).
- **Imaging Physics & Acquisition Protocols:**
  - Extreme technical diversity: isotropic vs. anisotropic voxels (slice thickness $0.5\text{ mm}$ to $7.5\text{ mm}$), varying field-of-view, high and low signal-to-noise ratios.
- **Multi-Center Distribution:** Sourced from international multi-center consortia (BraTS, LiTS, TCIA).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Standardized, high-quality expert annotations curated from established clinical challenges and specialty radiology groups.
- **Label Provenance Category:** Curated multi-expert human ground truth.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Freely downloadable via Zenodo and TCIA.
- **License / Terms:** CC BY-SA 4.0 (commercial and academic reuse permitted with attribution).
- **Artifact Availability:** Standardized 4D/3D NIfTI volumes, JSON task metadata schemas, and baseline docker containers.

### [S] Systemic Lineage & Genealogy
- **Historic Milestone:** The benchmark that gave birth to **nnU-Net**. Fabian Isensee et al. entered nnU-Net as a fully self-configuring pipeline without task-specific manual architecture tuning and swept 1st place across all 10 tasks.
- **Contamination Warning:** MSD is the most recycled benchmark in medical deep learning history. Almost every modern model has MSD data in its training or fine-tuning lineage. Testing on MSD today is a baseline sanity check, **never proof of out-of-domain generalization**.

---

## 3. Verified SOTA Benchmarks & Leaderboard
*Standings from the official Nature Communications version-of-record evaluation, ranked by overall normalized rank across all 10 tasks using Dice Similarity Coefficient (DSC) and Normalized Surface Distance (NSD).*

| Rank | Model / Framework | Developing Team | Core Architectural Philosophy | Evaluation Split & Setting | Mean Dice (Decathlon Avg) | Normalized Rank Score | Reference |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **nnU-Net (Ensemble)** | Fabian Isensee et al. (MIC-DKFZ) | Self-configuring 2D, 3D lowres, and 3D fullres U-Net ensemble with automated preprocessing | Blind Mystery Test Phase | **0.789** | **1.00 (1st Overall)** | [Nature Methods 18](https://doi.org/10.1038/s41592-020-01008-z) |
| **2** | **DiNTS / Auto3DSeg** | NVIDIA Medtech | Differentiable Neural Architecture Search with multi-path volumetric cell blocks | Blind Mystery Test Phase | **0.781** | **2.14** | [CVPR 2021](https://arxiv.org/abs/2103.15954) |
| **3** | **SwinUNETR Benchmark** | Vanderbilt / MONAI Consortium | Window-based shift self-attention transformer encoder with residual CNN decoder | Blind Mystery Test Phase | **0.774** | **3.25** | [CVPR 2022](https://arxiv.org/abs/2201.01266) |
| **4** | **UNETR** | Ali Hatamizadeh et al. (UCLA / NVIDIA) | Pure Vision Transformer (ViT) encoder directly coupled to CNN decoder | 5-Fold Cross-Validation | **0.762** | **4.10** | [WACV 2022](https://arxiv.org/abs/2103.10504) |
| **5** | **Auto-Fed-Avg / Generic U-Net** | Consortium Baselines | Standard 3D U-Net with heuristic hyperparameter tuning | Blind Mystery Test Phase | **0.735** | **5.30** | [Nature Comms 13](https://doi.org/10.1038/s41467-022-30695-9) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Download size: ~150 GB uncompressed across all 10 tasks.
  - Excellent for educational learning: tasks like *Task04_Hippocampus* can be trained in $<1\text{ hour}$ on an 8 GB consumer GPU.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel
  import json

  with open("Task01_BrainTumour/dataset.json") as f:
      meta = json.load(f)
  print(f"Dataset: {meta['name']}, Modalities: {meta['modality']}, Training cases: {len(meta['training'])}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Task07 (Pancreas) & Task08 (Hepatic Vessels):* These two tasks exhibit significantly lower Dice scores ($<0.65$) due to extreme class imbalance and micro-vascular tortuosity.
  2. *Contamination Blindness:* Reporting high MSD test scores without proving pretraining data exclusion is a critical red flag in peer review.

---

## 5. Downstream Foundation Model Consumers
- **nnU-Net v2** (`docs/02_models/01_segmentation/nnunet_v2.md`): The canonical founding benchmark.
- **MedSAM2** (`docs/02_models/01_segmentation/medsam2.md`): Multi-task prompt evaluation.
- **BiomedParse v2** (`docs/02_models/01_segmentation/biomedparse_v2.md`): Text-prompted decathlon baseline.
