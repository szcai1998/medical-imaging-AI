# autoPET: Automated Lesion Segmentation in Whole-Body PET/CT

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D11A]`
- **Domain & Modality:** Multi-Modal 3D Functional & Anatomical Imaging: $^{18}\text{F-FDG}$ PET/CT
- **Target Anatomy & Pathologies:** Whole-Body Malignant Metabolic Lesions (Lymphoma, Melanoma, Lung Cancer)
- **Release / Challenge Year:** 2022 (autoPET I) | 2023–2024 (autoPET II - Domain Generalization)
- **Evidence Code:** `E1` (Peer-Reviewed Paper in *Radiology: AI*) + `E2` (Official Grand Challenge Portal)
- **Access Level:** Public Training Data via TCIA / Official Grand Challenge Blind Test
- **Primary Source / Portal:** [autoPET Grand Challenge](https://autopet.grand-challenge.org/) | [autoPET II Portal](https://autopet-ii.grand-challenge.org/) | [TCIA Collection](https://doi.org/10.7937/8D-PETCT)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Positron Emission Tomography combined with Computed Tomography ($^{18}\text{F-FDG}$ PET/CT) is the clinical standard for systemic cancer staging and therapy monitoring. Differentiating true metabolic malignancy from high physiological FDG uptake (brain, myocardium, renal collecting system, urinary bladder, active bowel) is a major clinical hurdle for automated AI tools.
- **Evaluation Tasks:**
  - **autoPET I:** Automated metabolic tumor segmentation in whole-body volumes.
  - **autoPET II:** Domain generalization testing model robustness across shifted clinical sites, scanners, and reconstruction kernels.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **1,014 PET/CT examination studies from 900 unique patients** in the primary training release.
  - Paired 3D co-registered PET volumes (Standardized Uptake Value, SUV) and CT volumes (Hounsfield Units, HU).
- **Imaging Physics & Acquisition Protocols:**
  - Standardized whole-body oncology protocol from skull base to mid-thigh.
  - Injected tracer: $^{18}\text{F-fluorodeoxyglucose}$ ($^{18}\text{F-FDG}$).
  - PET voxel size: $\sim 2.0 \times 2.0 \times 3.0\text{ mm}$; CT resampled to match PET grid.
- **Multi-Center Distribution:** Primary training data sourced from University Hospital Tübingen, Germany; autoPET II test data enriched with external cohorts from University Hospital Munich (LMU).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Semi-automated thresholding followed by voxel-level manual editing and adjudication by nuclear medicine physicians and dual-boarded radiologists with extensive clinical PET/CT reporting experience.
- **Label Provenance Category:** Multi-reader expert nuclear medicine consensus.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via The Cancer Imaging Archive (TCIA) and Grand Challenge.
- **License / Terms:** Creative Commons CC-BY 4.0 (open for scientific and commercial benchmarking with attribution).
- **Artifact Availability:** Fully aligned PET (SUV) and CT (HU) volumes in NIfTI format, 3D tumor binary masks, and clinical diagnosis metadata.

### [S] Systemic Lineage & Genealogy
- **Domain Specialization:** Represents the definitive benchmark for multi-modal metabolic/anatomical fusion. Natural image vision backbones (like DINOv2) perform poorly on this task without domain adaptation because PET physics (SUV semantics, tracer kinetics) have no natural image counterpart.
- **Contamination Warning:** Training cohort is single-site; testing on external scanner datasets (autoPET II) is mandatory to establish true clinical transportability.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings from the official autoPET challenge evaluations, ranked by composite score balancing Dice Similarity Coefficient (DSC), False Positive Volume (FPV in mL), and False Negative Volume (FNV in mL).*

| Rank | Model / Submission | Lead Team / Affiliation | Core Architectural Mechanism | Evaluation Split & Setting | Mean Lesion DSC | False Positive Vol (FPV) | Award / Citation |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **nnU-Net Dual-Stream (PET-CT)** | Fabian Isensee et al. (MIC-DKFZ) | Independent encoder branches for SUV and CT with multi-scale cross-attention fusion and hard false-positive suppression | Official Blind Test Set ($N=150$) | **0.784** | **1.82 mL** | **Engineering Award** [MICCAI 2023](https://doi.org/10.1007/978-3-031-43904-9) |
| **2** | **Domain-Invariant SegResNet** | Adrian Galdran et al. (University of Adelaide) | Modality-specific instance normalization with spatial intensity dropout to resist site-shift | Official Blind Test Set ($N=150$) | **0.776** | **2.05 mL** | **Scientific Award** [autoPET II](https://autopet-ii.grand-challenge.org/) |
| **3** | **SwinUNETR Multi-Modal** | NVIDIA Medtech Consortium | 3D hierarchical transformer processing concatenated 2-channel SUV+CT feature tokens | Official Blind Test Set ($N=150$) | **0.768** | **2.34 mL** | [Grand Challenge Proceedings](https://autopet.grand-challenge.org/) |
| **4** | **Dual-Stream DINOv3 (Adapted)** | MIDL 2026 Academic Study | Dual-path vision foundation model adapted for metabolic tumor localization | Independent External Test Cohort | **0.759** | **2.61 mL** | [MIDL 2026 / S77](https://doi.org/10.1038/s41551-026-01741-4) |
| **5** | **Standard 3D nnU-Net Baseline** | Challenge Organizers (Gatidis et al.) | Concatenated 2-channel 3D fullres nnU-Net without specialized physiological masking | Official Blind Test Set ($N=150$) | **0.742** | **3.85 mL** | [Radiology: AI 2022](https://doi.org/10.1148/ryai.220087) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Download size: ~180 GB for the combined 1,014 paired volumes.
  - Requires a minimum of **16 GB to 24 GB VRAM** (RTX 3090 / 4090) due to dual-channel 3D patch inputs ($128 \times 128 \times 128$).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel
  import nibabel as nib

  pet_suv = nib.load("autoPET/imagesTr/PETCT_001_0001.nii.gz") # Channel 1: SUV
  ct_hu = nib.load("autoPET/imagesTr/PETCT_001_0000.nii.gz")   # Channel 0: CT
  print(f"PET shape: {pet_suv.shape}, Peak SUV in volume: {pet_suv.get_fdata().max():.2f}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Physiological Excretion Traps:* High false-positive rates frequently occur in the renal calyces, ureters, and urinary bladder where excreted $^{18}\text{F-FDG}$ concentrates at intense metabolic levels mimicking tumors.
  2. *Brain / Myocardial Misclassification:* Incomplete patient fasting causes myocardial uptake, leading naive models to misclassify heart tissue as mediastinal lymphoma.

---

## 5. Downstream Foundation Model Consumers
- **Open Whole-Body FDG PET/CT Foundation Model** (`docs/02_models/05_specialty_fms/petct_fm.md`): Pretrained metabolic backbone.
- **Dual-Stream DINOv3 PET/CT** (`docs/02_models/05_specialty_fms/dual_stream_dinov3_petct.md`): Domain-adapted DINOv3 architecture.
