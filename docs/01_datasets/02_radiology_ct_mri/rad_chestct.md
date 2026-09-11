# RAD-ChestCT: Duke University Large-Scale Chest CT Cohort

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D31]`
- **Domain & Modality:** 3D Chest Computed Tomography (Contrast & Non-Contrast CT) + Structured Abnormality & Anatomical Location Multi-Label Matrices
- **Target Anatomy & Pathologies:** Thoracic Cavity: 84 distinct radiological abnormalities across 52 anatomical locations (nodules, infiltrates, pleural thickening, mediastinal lymphadenopathy, emphysema, osseous fractures)
- **Release / Publication Year:** 2021 (*Medical Image Analysis* 67, 101857) | Zenodo Release: 2022
- **Evidence Code:** `E1` (Peer-Reviewed Journal Publication in *Medical Image Analysis*) + `E2` (Official Zenodo Open Release) + `E5A` (Canonical External Validation Cohort for CT-RATE / CT-CLIP)
- **Access Level:** Public Research Access for the 3,630-scan Zenodo release under Creative Commons Attribution 4.0 International (CC BY 4.0); full 35,747-scan cohort governed by Duke University institutional access
- **Primary Source / Portal:** [Medical Image Analysis (Draelos et al., 2021)](https://doi.org/10.1016/j.media.2020.101857) | [Zenodo Repository: 10.5281/zenodo.6406114](https://zenodo.org/records/6406114) | [GitHub: racheldraelos/RAD-ChestCT](https://github.com/racheldraelos/RAD-ChestCT)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Models developed on single-institution datasets often fail when deployed in new clinical settings due to distribution shifts in patient demographics, disease prevalence, and scanner protocols. RAD-ChestCT was designed to predict dozens of concurrent abnormalities from whole 3D CT volumes and serves as a cornerstone **external validation benchmark (`E5A`)** for evaluating the cross-institutional generalizability of chest CT foundation models (such as CT-CLIP).
- **Target Classes:** 84 thoracic abnormalities localized across 52 anatomical compartments.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **19,661 adult patients**
  - **35,747 total clinical CT scans** in the full Duke Medical Center archive
  - **3,630 openly released 3D CT scans** in the official Zenodo repository
  - *Counting Unit Discipline:* As established in Section 3A.4 and 17.1 of the Master Dossier, literature must **never** cite the 35,747 full-cohort count as if all volumes were available in the public Zenodo download. The open public benchmark consists strictly of the 3,630 released CT scans.
- **Imaging Physics & Acquisition Protocols:**
  - Helical thoracic CT scanners (GE, Siemens).
  - Both contrast-enhanced and non-contrast chest examinations. Axial resolution $512 \times 512$, slice thickness varying from $1.0\text{ mm}$ to $5.0\text{ mm}$.
  - Zenodo volumes are preprocessed into standardized $3\text{D}$ NumPy arrays ($128 \times 128 \times 128$ or original axial dimensions with Hounsfield Unit scaling).
- **Multi-Center Distribution:** Sourced from Duke University Medical Center (Durham, North Carolina, USA).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Abnormality and location labels were extracted from clinical radiology reports using an automated, rule-based medical NLP parsing pipeline designed specifically for chest CT grammar.
  - The NLP extraction logic and label veracity were rigorously validated and refined against a human-annotated test set reviewed by attending board-certified radiologists.
- **Label Provenance Category:** Rule-based clinical report-mined label extraction with radiologist verification.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Openly downloadable via Zenodo (DOI: 10.5281/zenodo.6406114).
- **License / Terms:** Creative Commons Attribution 4.0 International (CC BY 4.0).
- **Artifact Availability:** 3,630 3D CT volumes stored as `.npy` and NIfTI arrays, accompanied by master CSV files containing scan metadata and the complete $84 \times 52$ binary label matrices.

### [S] Systemic Lineage & Genealogy
- **Institutional Independence:** Originates from Duke University Health System (USA), entirely independent of CT-RATE `[D20]` (Turkey) and Merlin `[D30]` (Stanford).
- **Canonical Role (`E5A`):** Sits as the premier out-of-distribution external benchmark in volumetric chest AI. When CT-CLIP demonstrated an AUROC increase of 0.085 over supervised baselines on RAD-ChestCT, it provided authoritative evidence of cross-continent foundation model transfer.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the RAD-ChestCT external validation protocol (evaluating models on multi-abnormality classification under geographic and institutional distribution shift).*

| Rank | Model / Submission | Developing Team | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC | Macro-F1 Score | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **CT-CLIP (Zero-Shot Transfer)** | Hamamci et al. (ETH Zurich / TUM) | 3D ViT-B/16 contrastive VLM pretrained on CT-RATE | External Validation Split ($N=1,344$ scans) | **0.874** (±0.006) | **0.658** | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **2** | **CT-Net (Supervised)** | Draelos et al. (Duke University) | 3D CNN with axial attention pooling trained on Duke cohort | Internal Held-out Test Split ($N=3,600$) | **0.816** | **0.585** | [Med. Image Anal. 2021](https://doi.org/10.1016/j.media.2020.101857) |
| **3** | **3D DenseNet-121 Baseline** | Duke / Baselines | Standard 3D DenseNet trained with multi-label binary cross-entropy | Internal Held-out Test Split ($N=3,600$) | **0.789** | **0.562** | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **4** | **3D ResNet-50 Supervised** | Duke Baseline Reference | 3D Residual Network with volumetric average pooling | Internal Held-out Test Split ($N=3,600$) | **0.772** | **0.541** | [Med. Image Anal. 2021](https://doi.org/10.1016/j.media.2020.101857) |
| **5** | **2D Slice-Averaged ResNet Baseline** | Comparative Baseline | 2D ResNet-50 with max/mean pooling across axial slices | Internal Held-out Test Split ($N=3,600$) | **0.738** | **0.496** | [Med. Image Anal. 2021](https://doi.org/10.1016/j.media.2020.101857) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Open Zenodo release download size: ~180 GB.
  - Evaluation of preprocessed volumes is highly lightweight and can run on a single consumer GPU with **12 GB to 16 GB VRAM** (RTX 3080 / RTX 4080).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install numpy pandas
  import numpy as np
  import pandas as pd

  # Load master labels and inspect a 3D scan array
  labels = pd.read_csv("rad_chestct/labels.csv")
  scan_array = np.load("rad_chestct/scans/scan_0001.npy")
  print(f"3D Scan Shape: {scan_array.shape}, Range: [{scan_array.min()}, {scan_array.max()}]")
  print(f"Positive Labels in Case: {labels.columns[(labels.iloc[0] == 1)].tolist()[:5]}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Full vs. Open Split Ambiguity:* Researchers frequently misattribute results by reporting evaluations on the 3,630 public scans as if they evaluated the full 35,747 Duke health system cohort. Always explicitly report $N=3,630$ for open Zenodo studies.
  2. *Thick-Slice Anisotropy:* Certain scans in the legacy Duke archive have $5.0\text{ mm}$ slice thicknesses, introducing partial-volume blending that degrades detection of subcentimeter parenchymal nodules.

---

## 5. Downstream Foundation Model Consumers
The following foundation models utilize RAD-ChestCT as a primary external validation benchmark:
- **CT-CLIP / CT-CHAT** (`docs/02_models/02_volumetric_ct_mri/ct_clip_chat.md`): Serves as the canonical out-of-distribution external validation cohort (`E5A`) [S13].
- **Comparative CT FMs (SPECTRE / TAP-CT)** (`docs/02_models/02_volumetric_ct_mri/ct_comparators.md`): External cross-institutional evaluation.
