# CheXpert: Large Chest Radiograph Dataset with Uncertainty Labels & Expert Radiologist Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D13]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), frontal (PA/AP) and lateral projections
- **Target Anatomy & Pathologies:** Thorax; 14 observation categories (12 common thoracic pathologies + "No Finding" + "Support Devices"), focused on 5 benchmark conditions: Atelectasis, Cardiomegaly, Consolidation, Edema, Pleural Effusion
- **Release / Challenge Year:** 2019 (Stanford Machine Learning Group / Stanford AIMI)
- **Evidence Code:** `E1` (AAAI 2019 Proceedings) + `E2` (Official Stanford AIMI Portal & Competition Leaderboard)
- **Access Level:** Open Research Access upon registration (Stanford Research Use Agreement; non-commercial research use)
- **Primary Source / Portal:** [https://aimi.stanford.edu/datasets/chexpert-chest-x-rays](https://aimi.stanford.edu/datasets/chexpert-chest-x-rays) | Leaderboard: [https://stanfordmlgroup.github.io/competitions/chexpert/](https://stanfordmlgroup.github.io/competitions/chexpert/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Automated multi-label detection of thoracic pathologies from chest radiographs with explicit modeling of diagnostic uncertainty. Diagnostic ambiguity is an inherent property of chest radiography (e.g., distinguishing atelectasis from early consolidation); CheXpert explicitly formalizes uncertainty rather than discarding ambiguous studies.
- **Target Classes:** 14 radiologic observations:
  1. *Enlarged Cardiomediastinum*
  2. *Cardiomegaly*
  3. *Lung Opacity*
  4. *Lung Lesion*
  5. *Edema*
  6. *Consolidation*
  7. *Pneumonia*
  8. *Atelectasis*
  9. *Pneumothorax*
  10. *Pleural Effusion*
  11. *Pleural Other*
  12. *Fracture*
  13. *Support Devices*
  14. *No Finding*

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **224,316 chest radiographs**
  - **65,240 unique patients**
  - **500 validation studies** (annotated independently by 3 board-certified radiologists)
  - **500 hidden test studies** (evaluated by consensus of 5 board-certified radiologists)
- **Imaging Physics & Acquisition Protocols:**
  - Routine inpatient, emergency room, and outpatient examinations collected from Stanford Hospital between October 2002 and July 2017.
  - Multi-vendor digital systems; mix of bedside AP portable acquisitions and standing upright PA and lateral projections.
  - Original matrix sizes reach up to $3000 \times 3000$ pixels; downscaled standard research release (`CheXpert-v1.0-small`) resampled to $320 \times 320$ or $390 \times 320$ pixels.
- **Multi-Center Distribution:** Single-center academic health system (Stanford Health Care).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - **Training Set (Weak Supervision):** Labeled using the automated CheXpert rule-based labeler applied to free-text radiology reports. The labeler parses negation and uncertainty, outputting 1 (Positive), 0 (Negative), -1 (Uncertain), or blank (Unmentioned).
  - **Hidden Test Set (Consensus Ground Truth):** 500 chest radiographic studies were independently reviewed by 5 board-certified radiologists. The consensus reference standard was established through panel adjudication, providing a gold standard devoid of NLP extraction errors.
- **Label Provenance Category:** Automated weak NLP report labels (training set) contrasted with high-grade multi-radiologist consensus (held-out test set).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official download via Stanford AIMI after registering and accepting the Stanford Research Use Agreement.
- **License / Terms:** Non-commercial academic research agreement; redistribution or commercial exploitation strictly prohibited.
- **Artifact Availability:**
  - Full-resolution dataset (`CheXpert-v1.0.zip`, ~439 GB).
  - Downscaled dataset (`CheXpert-v1.0-small.zip`, ~11 GB).
  - Metadata CSV files: `train.csv`, `valid.csv`, and CodaLab evaluation suite.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Inspired by NIH ChestX-ray14 `[D15]`; developed to rectify lack of uncertainty handling and absent radiologist consensus test sets.
- **Derivative Ecosystem:** Sourced as the primary benchmark for CheXbert (transformer-based labeler) and CheXzero (zero-shot CLIP adapter).
- **Contamination & Overlap Warning:** CheXpert is universally present in vision-language pretraining pipelines (e.g., BioViL, Ark/Ark+, MedSigLIP). Any claim of "zero-shot" performance on CheXpert must explicitly verify that the model was not pretrained on CheXpert images.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official CheXpert Competition hidden test set ($N=500$ studies), benchmarked against the consensus of 5 board-certified radiologists across the 5 primary evaluation pathologies (Atelectasis, Cardiomegaly, Consolidation, Edema, Pleural Effusion). Evaluated using Mean Area Under the ROC Curve (AUROC).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC (5 Pathologies) | Rad Consensus Delta | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **DeepAUC-v1 (Ensemble)** | Yuan et al. | Multi-architecture ensemble optimizing direct surrogate AUC loss functions with self-paced curriculum | Official Hidden Test ($N=500$) | **0.930** | Superior to $3/5$ radiologists | [Leaderboard Entry](https://stanfordmlgroup.github.io/competitions/chexpert/) |
| **2** | **Hierarchical-Learning-V1** | Vingroup Big Data Institute (VinBrain) | DenseNet-121 + EfficientNet ensemble with hierarchical dependency modeling between thoracic findings | Official Hidden Test ($N=500$) | **0.930** | Superior to $3/5$ radiologists | [Leaderboard Entry](https://stanfordmlgroup.github.io/competitions/chexpert/) |
| **3** | **Conditional-Training-LSR** | Stanford / Independent | Label smoothing regularization (LSR) combined with conditional pathology hierarchy heads | Official Hidden Test ($N=500$) | **0.929** | Superior to $3/5$ radiologists | [Leaderboard Entry](https://stanfordmlgroup.github.io/competitions/chexpert/) |
| **4** | **YWW Ensemble** | Independent Researcher | Multi-resolution ResNeSt + DenseNet-201 ensemble with multi-scale test-time augmentation | Official Hidden Test ($N=500$) | **0.929** | Superior to $3/5$ radiologists | [Leaderboard Entry](https://stanfordmlgroup.github.io/competitions/chexpert/) |
| **5** | **CheXNeXt Baseline Ensemble** | Rajpurkar et al. (Stanford ML Group) | 30-model ensemble of 121-layer DenseNets with U-Ignore and U-Ones uncertainty policies | Official Hidden Test ($N=500$) | **0.893** | Baseline comparator (Rad Avg: 0.924) | [AAAI 2019 / PLOS Med](https://doi.org/10.1609/aaai.v33i01.3301590) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Downscaled `CheXpert-v1.0-small` archive: **~11 GB**, fits on standard workstation SSDs and is trainable within hours on a single **8 GB–16 GB VRAM** GPU (e.g., RTX 3070 / RTX 4080).
  - Full-resolution dataset (~439 GB) requires substantial multi-terabyte storage.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow torchvision torch
  import pandas as pd
  from PIL import Image
  import os

  # 1. Load CheXpert validation split labels
  val_df = pd.read_csv("CheXpert-v1.0-small/valid.csv")
  print(f"Validation cases: {len(val_df)}, Pathologies: {list(val_df.columns[5:10])}")

  # 2. Inspect sample image geometry and view orientation
  sample_row = val_df.iloc[0]
  img_path = sample_row['Path']
  if os.path.exists(img_path):
      img = Image.open(img_path)
      print(f"Path: {img_path}, Size: {img.size}, Orientation: {sample_row['Frontal/Lateral']}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *The Uncertainty Label Policy (-1 Discrepancy):* How -1 labels are handled dramatically shifts reported performance by 2–4% AUROC. Common strategies include:
     - **U-Ignore:** Ignore uncertain labels during loss calculation.
     - **U-Zeros:** Treat all -1 as negative (0).
     - **U-Ones:** Treat all -1 as positive (1) (frequently best for edema/consolidation).
     - **U-MultiClass:** Formulate as 3-class classification.
     Never compare two models on CheXpert without ensuring identical uncertainty policies.
  2. *Frontal vs. Lateral Mixture:* Unlike datasets with only frontal views, CheXpert includes lateral radiographs. Standard benchmarks evaluate strictly on frontal images (`Frontal/Lateral == 'Frontal'`). Failing to filter out lateral images degrades classification accuracy.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize CheXpert as a primary representation or fine-tuning benchmark:
- **RAD-DINO** (`docs/02_models/02_cxr/rad_dino.md`): Evaluated on CheXpert linear-probe classification.
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Integrates CheXpert within its multi-dataset heterogeneous supervision hierarchy.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Uses CheXpert observations for clinical concept decomposition.
- **MedSigLIP** (`docs/02_models/01_general_representation/medsiglip.md`): Evaluates zero-shot transfer capabilities.
