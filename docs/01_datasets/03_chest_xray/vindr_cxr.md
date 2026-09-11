# VinDr-CXR: Multi-Institutional Radiologist-Consensus Chest Radiograph Benchmark with Local Bounding Boxes

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D16]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), Posteroanterior (PA) view
- **Target Anatomy & Pathologies:** Thoracic cavity and lungs; 22 local critical findings annotated with pixel-level bounding boxes (e.g., aortic enlargement, atelectasis, cardiomegaly, lung opacity, pleural effusion, pneumothorax) plus 6 global diagnoses (tuberculosis, lung tumor, pneumonia, other infection, chronic obstructive pulmonary disease [COPD], other disease)
- **Release / Challenge Year:** 2020–2022 (VinBigData / Vingroup Big Data Institute & Hanoi Medical University)
- **Evidence Code:** `E1` (Peer-Reviewed Nature Scientific Data Descriptor) + `E2` (PhysioNet v1.0.0 & Official Kaggle Challenge Portal)
- **Access Level:** Open Credentialed Research Access (PhysioNet Credentialed Data Use Agreement)
- **Primary Source / Portal:** [https://physionet.org/content/vindr-cxr/1.0.0/](https://physionet.org/content/vindr-cxr/1.0.0/) | Kaggle: [https://www.kaggle.com/c/vinbigdata-chest-xray-abnormalities-detection](https://www.kaggle.com/c/vinbigdata-chest-xray-abnormalities-detection) | Paper: [https://doi.org/10.1038/s41597-022-01498-w](https://doi.org/10.1038/s41597-022-01498-w)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Weakly supervised NLP report-mined datasets (NIH ChestX-ray14, CheXpert) provide image-level binary labels that suffer from substantial label noise and offer no native spatial bounding coordinates. VinDr-CXR provides high-fidelity, radiologist-verified spatial annotations to benchmark object detection, phrase grounding, and multi-label diagnosis on an Asian patient population.
- **Target Classes & Schema:**
  - **22 Local Abnormalities (Bounding Boxes):** Aortic enlargement, Atelectasis, Calcification, Cardiomegaly, Clavicle fracture, Consolidation, Edema, Emphysema, Enlarged PA, Extramedullary hematopoiesis, Fibrosis, Interstitial lung disease, Infiltration, Lung cavity, Lung cyst, Lung opacity, Mediastinal shift, Nodule/Mass, Pleural effusion, Pleural thickening, Pneumothorax, Rib fracture.
  - **6 Global Disease Diagnoses:** Tuberculosis, Lung tumor, Pneumonia, Other infection, COPD, Other disease.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **18,000 released chest radiographs**
  - **18,000 unique patients and examinations (strictly 1 PA CXR per patient)**
  - **15,000 training radiographs** (each labeled independently by 3 radiologists out of a pool of 17)
  - **3,000 held-out test radiographs** (each evaluated independently by 5 radiologists with multi-reader consensus adjudication)
- **Imaging Physics & Acquisition Protocols:**
  - Collected retrospectively from two major hospital networks in Vietnam: Hospital 108 and Hanoi Medical University Hospital, between 2018 and 2020.
  - Sourced from modern high-frequency digital radiography systems (GE Healthcare, Philips, Siemens).
  - High native spatial resolution (matrix dimensions up to $3072 \times 3072$ pixels at $0.14\text{ mm}$ pixel pitch), stored in native DICOM format preserving 12-to-16-bit dynamic range.
- **Multi-Center Distribution:** Multi-institutional two-center cohort from Vietnam (Southeast Asian demographic).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Gold-standard human expert consensus: Curated by a team of **17 board-certified practicing radiologists**, each with at least 8 years of clinical thoracic experience.
  - **Training Split (15,000 scans):** Each image was reviewed and annotated with bounding boxes independently by **3 different radiologists**.
  - **Test Split (3,000 scans):** Annotated independently by **5 radiologists**; discordant bounding boxes and diagnoses were reviewed in joint consensus sessions to formulate the definitive test ground truth.
- **Label Provenance Category:** Native human expert multi-reader consensus (zero automated NLP extraction, zero synthetic pseudo-labels).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted on PhysioNet (`vindr-cxr/1.0.0`) under PhysioNet Credentialed Access.
- **License / Terms:** PhysioNet Credentialed Data Use Agreement; strictly non-commercial academic research.
- **Artifact Availability:**
  - Raw uncompressed DICOM volumes (~500 GB).
  - Kaggle competition downscaled format ($1024 \times 1024$ PNGs, ~15 GB).
  - Annotation CSVs (`annotations_train.csv`, `annotations_test.csv`) specifying `image_id`, `rad_id`, `class_name`, `class_id`, `x_min`, `y_min`, `x_max`, `y_max`.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Independent native Asian clinical cohort developed under the VinBigData initiative.
- **Derivative Ecosystem:** Provided the foundational evaluation ground truth for thoracic object detection models and the Kaggle VinBigData Chest X-ray Abnormalities Detection challenge.
- **Contamination & Overlap Warning:** Foundation models (e.g., CARE-X, Ark+, MAIRA-2) that benchmark bounding-box detection on VinDr-CXR must explicitly declare whether the 15,000 training images were included in visual pretraining or instruction tuning. Zero-shot grounding claims are valid only when VinDr-CXR was strictly held out.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official VinDr-CXR / VinBigData 3,000-image held-out test set evaluated by 5-radiologist consensus. Primary competition task: 14-class thoracic abnormality detection evaluated by mean Average Precision (mAP) under PASCAL VOC 2010 criteria at $\text{IoU} > 0.4$.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | mAP @ IoU > 0.4 | Global Disease AUROC | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Keep Going (Ensemble)** | Kaggle VinBigData Winner | 12-model ensemble combining YOLOv5x, Faster R-CNN, and Cascade R-CNN with Weighted Box Fusion (WBF) and multi-scale TTA | 5-Radiologist Consensus Test ($N=3,000$) | **0.332** | — | [Kaggle Winning Solution](https://www.kaggle.com/c/vinbigdata-chest-xray-abnormalities-detection) |
| **2** | **ZFTurbo Ensemble** | Sergey Zlobin & Ivan Panshin | Ensemble of YOLOv5x6, EfficientDet-D7, and Faster R-CNN with post-hoc radiologist disagreement calibration | 5-Radiologist Consensus Test ($N=3,000$) | **0.319** | — | [GitHub Repo](https://github.com/ZFTurbo/2nd-place-solution-for-VinBigData-Chest-X-ray-Abnormalities-Detection) |
| **3** | **Scu-sen Ensemble** | Kaggle 3rd Place Team | Cascaded Mask R-CNN & YOLOv5 ensemble with radiologist confidence filtering and morphological anchors | 5-Radiologist Consensus Test ($N=3,000$) | **0.305** | — | [GitHub Repo](https://github.com/Scu-sen/VinBigData-Chest-X-ray-Abnormalities-Detection) |
| **4** | **CARE-X Localization Head** | Ranjit et al. (2026) | Multi-modal CXR foundation model with auxiliary bounding-box localization heads and text conditioning | Held-out VinDr-CXR Test Split ($N=3,000$) | **0.298** | **0.914** | [arXiv:2608.03890](https://arxiv.org/abs/2608.03890) |
| **5** | **VinDr-CXR Baseline** | Nguyen et al. (VinBigData / HMU) | Standard Faster R-CNN with ResNet-50-FPN backbone trained with SGD and standard augmentations | 5-Radiologist Consensus Test ($N=3,000$) | **0.224** | **0.892** | [Scientific Data 9:429](https://doi.org/10.1038/s41597-022-01498-w) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Kaggle preprocessed PNG dataset ($1024 \times 1024$): **~15 GB**, trainable on consumer GPUs with **12 GB–16 GB VRAM** (e.g., RTX 3060 / RTX 4070).
  - Full DICOM distribution: **~500 GB**, requiring high-speed local NVMe or SSD storage for fast multi-threaded I/O.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pydicom pillow
  import pandas as pd
  import pydicom

  # 1. Load train annotations
  ann_df = pd.read_csv("train.csv", nrows=10)
  print(f"Loaded annotations. Unique abnormalities: {ann_df['class_name'].unique()}")

  # 2. Inspect radiologist annotation record with coordinates
  sample = ann_df[ann_df['class_name'] != 'No finding'].iloc[0]
  print(f"Image ID: {sample['image_id']}, Rad ID: {sample['rad_id']}, "
        f"Box: [{sample['x_min']}, {sample['y_min']}, {sample['x_max']}, {sample['y_max']}]")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Inter-Radiologist Bounding Box Variance:* Different radiologists frequently annotate diffuse lesions (e.g., Infiltration or Lung Opacity) with widely varying box margins. Training directly on unmerged raw boxes introduces gradient conflict. **Weighted Box Fusion (WBF)** or Non-Maximum Suppression (NMS) across `rad_id` annotations is strictly recommended.
  2. *Extreme Foreground/Background Imbalance:* Upwards of 70% of training scans exhibit "No finding", and specific lesion categories (e.g., pneumothorax, clavicle fracture) have very low positive support (<2%). Anchor-based detectors require two-stage negative filtering or focal loss formulation.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize VinDr-CXR as a primary spatial localization or transfer benchmark:
- **CARE-X** (`docs/02_models/02_cxr/care_x.md`): Evaluates auxiliary bounding-box detection heads on VinDr-CXR.
- **MAIRA-2** (`docs/02_models/02_cxr/maira_2.md`): Utilizes VinDr-CXR bounding boxes for grounded visual prompt evaluation.
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Incorporates VinDr-CXR annotations for localization and multi-task learning.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Uses VinDr-CXR consensus boxes to audit concept spatial attributions.
