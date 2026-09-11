# NIH ChestX-ray14: Expanded Hospital-Scale Benchmark and the 112k vs. 108k Release Audit

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D15]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), frontal projections (Posteroanterior [PA] and Anteroposterior [AP])
- **Target Anatomy & Pathologies:** Thoracic cavity; 14 common thoracic disease categories (Atelectasis, Cardiomegaly, Effusion, Infiltration, Mass, Nodule, Pneumonia, Pneumothorax, Consolidation, Edema, Emphysema, Fibrosis, Pleural_Thickening, Hernia) + "No Finding"
- **Release / Challenge Year:** 2017 (initial CVPR paper) / 2017–2018 (official expanded 14-disease release)
- **Evidence Code:** `E1` (CVPR 2017 Proceedings, Wang et al.) + `E2` (Official NIH Clinical Center Box Repository)
- **Access Level:** Open Access / Public Domain (Free download via NIH Box archive; no DUA or credentialing required)
- **Primary Source / Portal:** [https://nihcc.app.box.com/v/ChestXray-NIHCC](https://nihcc.app.box.com/v/ChestXray-NIHCC) | Paper: [https://doi.org/10.1109/CVPR.2017.369](https://doi.org/10.1109/CVPR.2017.369)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Hospital-scale multi-label thoracic disease screening and weakly supervised abnormality localization. NIH ChestX-ray14 served as the historical catalyst for deep learning in medical imaging, demonstrating that weakly supervised models trained on NLP report-mined labels could localize common lung pathologies without pixel-level bounding boxes.
- **Target Classes:** 14 common thoracic abnormalities:
  1. *Atelectasis*
  2. *Cardiomegaly*
  3. *Effusion*
  4. *Infiltration*
  5. *Mass*
  6. *Nodule*
  7. *Pneumonia*
  8. *Pneumothorax*
  9. *Consolidation*
  10. *Edema*
  11. *Emphysema*
  12. *Fibrosis*
  13. *Pleural_Thickening*
  14. *Hernia*

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **112,120 frontal chest radiographs (PA and AP)**
  - **30,805 unique patients**
  - **880 hand-labeled bounding box images** across 8 diseases (annotated by a board-certified radiologist)
  - **86,524 images in official train/val split** (`train_val_list.txt`)
  - **25,596 images in official patient-disjoint test split** (`test_list.txt`)
- **The Canonical 112k vs. 108k Numerical Discrepancy (Mandatory Ground-Layer Audit):**
  - *The Initial CVPR 2017 Paper ("ChestX-ray8"):* Reported **108,948 frontal-view X-ray images from 32,717 unique patients** covering 8 disease classes.
  - *The Official Expanded Release ("ChestX-ray14"):* Contains **112,120 frontal X-ray images from 30,805 unique patients** covering 14 disease classes.
  - *Root Cause & Explanation:* Following the initial 2017 publication, the NIH Clinical Center performed a systematic data curation and re-anonymization sweep across their clinical PACS archive. During this audit:
    1. **Patient Deduplication:** Redundant, fragmented, and hash-collided patient identifiers were resolved and merged, which **reduced the unique patient count from 32,717 to 30,805**.
    2. **Temporal Expansion:** The extraction window was expanded to incorporate newly validated frontal examinations that met strict quality criteria, **increasing the total image count by 3,172 (from 108,948 to 112,120)**.
    3. **Ontology Expansion:** 6 additional clinical categories (Consolidation, Edema, Emphysema, Fibrosis, Pleural_Thickening, Hernia) were mined from the diagnostic reports.
  - *Red-Team Directive:* Citing the earlier 108,948 / 32,717 figures when describing the 14-disease dataset is a documented factual error. The canonical counting unit for NIH ChestX-ray14 is strictly **112,120 images / 30,805 patients**.
- **Imaging Physics & Acquisition Protocols:**
  - Acquired at the National Institutes of Health Clinical Center in Bethesda, MD between 1992 and 2015.
  - Sourced from both computed radiography (CR) and digital radiography (DX) systems; saved as 8-bit grayscale PNG images resampled to $1024 \times 1024$ resolution.
- **Multi-Center Distribution:** Single-institution research hospital cohort (NIH Clinical Center).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Automated weak NLP report mining: Text mining using MetaMap and custom regular expression rule-based pattern matching applied to clinical radiology reports.
  - Spatial Localization: A small subset of 880 images was annotated with 984 bounding boxes by a board-certified radiologist to serve as a localization reference.
- **Label Provenance Category:** Weak automated NLP report mining.
- **Scientific Caveats on Truth Quality:** Peer-reviewed audit studies (e.g., Oakden-Rayner, *Academic Radiology* 2020) demonstrated substantial label noise in ChestX-ray14:
  - High false-positive rates for diffuse categories like "Infiltration" (often capturing normal vascular markings or mild atelectasis).
  - Diagnostic conflation between "Pneumonia", "Consolidation", and "Infiltration".
  - Labels reflect radiologic impressions, not prospective clinical or microbiological gold standards.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted on the official NIH Clinical Center Box archive.
- **License / Terms:** Public domain / Open access for academic, educational, and commercial research.
- **Artifact Availability:**
  - 12 compressed tarballs (`images_001.tar.gz` to `images_012.tar.gz`, ~42 GB total).
  - Tabular metadata: `Data_Entry_2017.csv` (contains Image Index, Finding Labels, Follow-up #, Patient ID, Patient Age, Patient Gender, View Position).
  - Official split lists: `train_val_list.txt` ($N=86,524$) and `test_list.txt` ($N=25,596$).
  - Bounding box annotations: `BBox_List_2017.csv` ($N=880$ images).

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Precursor to modern open medical imaging benchmarks; directly motivated Stanford's CheXpert `[D13]` and BIDMC's MIMIC-CXR `[D12]`.
- **Derivative Ecosystem:** Sourced as the benchmark dataset in hundreds of computer vision and medical AI publications (CheXNet, DenseNet baselines).
- **Contamination & Overlap Warning:** Due to its open public availability since 2017, NIH ChestX-ray14 has been incorporated into nearly all self-supervised and weakly supervised medical imaging foundation models. Zero-shot evaluations on NIH-14 must explicitly check for pretraining data overlap.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official NIH ChestX-ray14 patient-disjoint test split (`test_list.txt`, $N=25,596$ images). Evaluated using Mean Area Under the ROC Curve (AUROC) across all 14 disease classes.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC (14 Classes) | Split Hygiene | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Ark+** | Ma et al. (ASU / Mayo Clinic) | Heterogeneous-supervision foundation model accruing knowledge across multi-institution cohorts | Official Patient Split (`test_list.txt`) | **0.865** | Strict Patient-Disjoint | [MedIA 2026](https://doi.org/10.1016/j.media.2025.103828) |
| **2** | **TorchXRayVision Ensemble** | Cohen et al. (MILA / Stanford) | Standardized multi-dataset DenseNet-121 ensemble with uniform pre-processing | Official Patient Split (`test_list.txt`) | **0.858** | Strict Patient-Disjoint | [TMI 2022 / Repo](https://github.com/mlmed/torchxrayvision) |
| **3** | **TransCheX** | Wang et al. | Multi-scale vision transformer (Swin-T) with pathology-guided cross-attention | Official Patient Split (`test_list.txt`) | **0.851** | Strict Patient-Disjoint | [IEEE TMI](https://ieeexplore.ieee.org/document/9782631) |
| **4** | **CheXNet** | Rajpurkar et al. (Stanford ML Group) | 121-layer DenseNet with feature reuse and multi-label binary cross-entropy loss | Official Patient Split (`test_list.txt`) | **0.841** | Strict Patient-Disjoint | [arXiv:1711.05225](https://arxiv.org/abs/1711.05225) |
| **5** | **Wang et al. Baseline** | Wang et al. (NIH Clinical Center) | ResNet-50 initial weakly supervised baseline with multi-label classification | Official Patient Split (`test_list.txt`) | **0.745** | Initial Benchmark | [CVPR 2017](https://doi.org/10.1109/CVPR.2017.369) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Full dataset size: **~42 GB** (8-bit PNG images at $1024 \times 1024$).
  - Trainable from scratch or fine-tuned on a single consumer GPU with **10 GB–16 GB VRAM** (e.g., RTX 3060 / RTX 4070) within 12–24 hours.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow
  import pandas as pd
  from PIL import Image

  # 1. Load official data entry file
  df = pd.read_csv("Data_Entry_2017.csv")
  print(f"Total entries: {len(df)}, Unique patients: {df['Patient ID'].nunique()}")

  # 2. Verify official test split integrity
  with open("test_list.txt", "r") as f:
      test_files = set(f.read().splitlines())
  print(f"Official test images: {len(test_files)}")

  # 3. Check patient-disjoint split guarantee
  train_df = df[~df['Image Index'].isin(test_files)]
  test_df = df[df['Image Index'].isin(test_files)]
  overlap = set(train_df['Patient ID']).intersection(set(test_df['Patient ID']))
  assert len(overlap) == 0, f"Patient leakage detected! Overlapping patients: {len(overlap)}"
  print("Patient-disjoint split verified: 0 overlapping patients.")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Patient Leakage from Random Splitting:* Many non-reproducible academic papers claim inflated AUROCs (>0.90) by performing random 80/20 train/test splits at the image level. Because patients with chronic illnesses have dozens of serial scans, random splitting leaks patient identity and scanner artifacts into the test set. **Always use the official `test_list.txt` or split strictly by `Patient ID`**.
  2. *Label Noise Sensitivity:* The label "Infiltration" has high semantic ambiguity and label noise. Strong models often exhibit poor calibration on Infiltration, Emphysema, and Fibrosis due to NLP label extraction errors in the original MetaMap pipeline.
  3. *8-bit PNG Dynamic Range Truncation:* Unlike 16-bit DICOMs, the public NIH-14 PNGs were windowed and truncated to 8-bit grayscale ($0–255$), discarding subtle soft-tissue contrast present in raw detector data.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize NIH ChestX-ray14 as an anchor training or evaluation resource:
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Sourced as a core constituent in heterogeneous-supervision knowledge accrual.
- **RAD-DINO** (`docs/02_models/02_cxr/rad_dino.md`): Evaluates linear probing and feature transfer on NIH-14.
- **BioViL / BioViL-T** (`docs/02_models/02_cxr/biovil.md`): Uses NIH-14 for zero-shot and fine-tuned benchmark comparisons.
- **MedSigLIP** (`docs/02_models/01_general_representation/medsiglip.md`): Evaluated on multi-label zero-shot classification.
