# LUNA16: LUng Nodule Analysis Challenge Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D8]`
- **Domain & Modality:** 3D Low-Dose & Diagnostic Thoracic CT
- **Target Anatomy & Pathologies:** Pulmonary Nodules ($\ge 3\text{ mm}$ in diameter)
- **Release / Year:** 2016 (Challenge launch) | Canonical reference benchmark in lung cancer CAD
- **Access Level:** Open Challenge Release under Creative Commons CC BY 4.0
- **Primary Source / Portal:** [https://luna16.grand-challenge.org/](https://luna16.grand-challenge.org/) | [LIDC-IDRI Parent via TCIA](https://doi.org/10.7937/K9/TCIA.2011.D1JSTTXD)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Early detection of malignant lung nodules in low-dose CT screening reduces lung cancer mortality by $>20\%$, but radiologists face high visual fatigue and false-positive burdens. LUNA16 evaluates automated nodule detection and false-positive reduction algorithms.
- **Evaluation Tracks:**
  - **Track 1 (Nodule Detection):** End-to-end Computer-Aided Detection (CAD) taking raw CT volumes and predicting 3D nodule coordinate locations and probabilities.
  - **Track 2 (False Positive Reduction):** Classification of predefined candidate locations to weed out benign structural mimics (blood vessels, scarring, rib shadows).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **888 thoracic CT examinations** (filtered from the 1,018 scans in LIDC-IDRI).
  - Contains **1,186 verified nodule annotations**.
- **Imaging Physics & Acquisition Protocols:**
  - Slice thickness strictly $\le 2.5\text{ mm}$ (median $1.25\text{ mm}$) to avoid severe partial-volume z-axis distortion.
  - Multi-vendor CT scanners across varied clinical screening settings.
- **Multi-Center Distribution:** Sourced from 7 academic medical centers across the United States.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Two-phase consensus reading by 4 experienced thoracic radiologists from the LIDC-IDRI initiative. Reference nodules were strictly defined as lesions with diameter $\ge 3\text{ mm}$ identified and agreed upon by at least **3 out of the 4 independent radiologists**.
- **Label Provenance Category:** Multi-reader human expert adjudicated consensus (rigorous gold standard).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via Grand Challenge and Zenodo.
- **License / Terms:** CC BY 4.0 (free for commercial and academic reuse).
- **Artifact Availability:** Preprocessed `.mhd`/`.raw` volumes, CSV candidate nodule coordinate lists, and 10-fold cross-validation evaluation splits.

### [S] Systemic Lineage & Genealogy
- **Parent Cohort:** Direct curated subset of **LIDC-IDRI** (The Lung Image Database Consortium and Image Database Resource Initiative).
- **Contamination Alert:** **LUNA16 and LIDC-IDRI are not independent.** Any model claiming external generalization on LUNA16 after pretraining on LIDC-IDRI (or general thoracic foundation models) has committed severe circular validation.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official LUNA16 Free-Response ROC (FROC) evaluation. Ranked by the **Competition Performance Metric (CPM)**, representing the average sensitivity at 7 fixed false-positive rates per scan: 1/8, 1/4, 1/2, 1, 2, 4, and 8 FPs/scan.*

| Rank | Model / Framework | Developing Team | Core Architectural Innovation | Official CPM Score | Sens. @ 1 FP/scan | Sens. @ 2 FP/scan | Reference |
| :---: | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **DeepLung (3D Dual-Path ResNet)** | Wentao Zhu et al. (UC Irvine) | 3D Faster R-CNN with dual-path residual blocks and deep focal loss | **0.958** | **0.942** | **0.961** | [IEEE TMI 37](https://doi.org/10.1109/TMI.2018.2833446) |
| **2** | **nnDetection / nnU-Net CAD** | Michael Baumgartner et al. (DKFZ) | Self-configuring 3D object detection framework tailored for anisotropic volumetric medical data | **0.952** | **0.938** | **0.957** | [Nature Methods 18](https://doi.org/10.1038/s41592-021-01297-6) |
| **3** | **Multi-Crop 3D CNN Ensemble** | Jia Ding et al. (Tsinghua / Tencent) | Multi-scale contextual patch cropping with candidate feature fusion | **0.944** | **0.925** | **0.949** | [IEEE TMI 2017](https://doi.org/10.1109/TMI.2017.2725580) |
| **4** | **3D DCNN + Multi-Resolution** | Arnaud Setio et al. (Radboud UMC) | Multi-view 2D/3D convolutional ensemble with aggressive hard negative mining | **0.938** | **0.916** | **0.942** | [IEEE TMI 35](https://doi.org/10.1109/TMI.2016.2536809) |
| **5** | **Classic Diagnostic Baseline** | Challenge Organizers | Multi-stage CAD system using morphological hand-crafted filters + SVM | **0.824** | **0.782** | **0.820** | [Medical Image Analysis 42](https://doi.org/10.1016/j.media.2017.06.015) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Complete dataset storage: ~120 GB in `.mhd`/`.raw` format.
  - False positive reduction can be trained on a modest **8 GB GPU**; full 3D anchor-free detection requires **16 GB VRAM**.
- **Minimal Local Verification / Load Command:**
  ```python
  import SimpleITK as sitk
  img = sitk.ReadImage("luna16/subset0/1.3.6.1.4.1.14519.5.2.1.6279.6001.109002525524522225658609808059.mhd")
  print(f"Origin: {img.GetOrigin()}, Spacing: {img.GetSpacing()}, Dimensions: {img.GetSize()}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Sub-Solid and Ground-Glass Nodules (GGNs):* Algorithms achieve $>98\%$ sensitivity on solid calcified nodules, but performance drops sharply on faint non-solid ground-glass opacities that have adenocarcinoma in situ potential.
  2. *Juxta-Pleural & Juxta-Vascular Nodules:* Nodules attached directly to the chest wall pleura or pulmonary vessels are frequently missed or excluded by automated lung boundary masks.
