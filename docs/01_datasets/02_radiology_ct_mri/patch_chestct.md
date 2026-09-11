# PatchChestCT: 3D Patch-Level Spatial Annotation Dataset for Chest CT

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D22]`
- **Domain & Modality:** 3D Non-Contrast Chest Computed Tomography (Chest CT) + 3D Patch-Level Multi-Abnormality Binary Spatial Matrices
- **Target Anatomy & Pathologies:** Nine clinically critical chest CT abnormalities:
  1. *Arterial wall calcification*
  2. *Coronary artery calcification*
  3. *Pericardial effusion*
  4. *Hiatal hernia*
  5. *Lymphadenopathy*
  6. *Atelectasis*
  7. *Lung opacity*
  8. *Consolidation*
  9. *Bronchiectasis*
- **Release / Publication Year:** 2026 (*Scientific Data* 13:1180, published 12 August 2026, DOI: 10.1038/s41597-026-07793-0)
- **Evidence Code:** `E1` (Peer-Reviewed Data Descriptor in *Scientific Data*) + `E2` (Official Zenodo Repository)
- **Access Level:** Annotations openly accessible on Zenodo under Creative Commons Attribution 4.0 International (CC BY 4.0); underlying 3D CT volumes must be obtained separately from CT-RATE under CC BY-NC-SA 4.0 terms
- **Primary Source / Portal:** [Scientific Data (Li et al., 2026)](https://doi.org/10.1038/s41597-026-07793-0) | [Zenodo Dataset: SadVoxel/PatchChestCT](https://doi.org/10.5281/zenodo.13328224) | [GitHub: SadVoxel/PatchChestCT](https://github.com/SadVoxel/PatchChestCT)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Full voxel-level dense segmentation of dozens of 3D thoracic abnormalities is prohibitively expensive, while study-level global classification fails to explain where abnormalities reside. PatchChestCT solves this intermediate-granularity bottleneck by establishing a standardized 3D patch grid ($24 \times 12 \times 12$), aligning directly with the patch token embeddings of modern 3D Vision Transformers (ViTs) and hierarchical CNNs.
- **Target Classes:** 9 clinically prevalent parenchymal, mediastinal, and cardiovascular findings.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **2,201 physician-reviewed 3D CT studies**
  - **9 abnormality classes**
  - **3,456 spatial patches per volume** (structured as a $24\text{ [depth]} \times 12\text{ [height]} \times 12\text{ [width]}$ 3D grid)
  - Over **7.6 million total evaluated patch-abnormality instances**.
- **Imaging Physics & Acquisition Protocols:**
  - Non-contrast thoracic CT scans derived from the CT-RATE parent cohort.
  - Voxel volumes resampled to standardized spatial dimensions ($384 \times 192 \times 192$ voxels), where each grid cell corresponds to a localized $16 \times 16 \times 16$ voxel physical volume block.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Candidate regions identified by automated candidate detectors, followed by systematic slice-by-slice manual review, boundary verification, and adjudication by licensed clinical physicians and radiologists.
  - Patch labels are binary ($1 = \text{abnormality present in patch}$, $0 = \text{absent}$).
- **Label Provenance Category:** Native human physician-reviewed spatial ground truth (strictly validated by medical doctors, not purely synthetic LLM extractions).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official annotation repository hosted on Zenodo (`SadVoxel/PatchChestCT`, DOI: 10.5281/zenodo.13328224).
- **License / Terms:** Creative Commons Attribution 4.0 International (CC BY 4.0) for the patch annotation arrays.
- **Artifact Availability:** Binary NumPy `.npz` files for all 2,201 studies keyed by CT-RATE volume IDs (`train_XXXX_a_1.npz`), mapping code, and validation splits.
- **Important Ingestion Rule:** Zenodo distributes **only the spatial annotation files**. Researchers must independently download the matching CT volumes from CT-RATE.

### [S] Systemic Lineage & Genealogy
- **Parent Lineage:** Direct spatial derivative of **CT-RATE `[D20]`**.
- **Contamination & Experimental Design Warning:**
  - Sourced directly from CT-RATE; models pretrained on CT-RATE share underlying patient data.
  - The authors explicitly highlight that the 2,201 studies were enriched for abnormalities to maximize spatial training efficiency. Consequently, PatchChestCT should be utilized **primarily as a spatial training and localization supervision resource**, rather than an unselected clinical prevalence benchmark.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the PatchChestCT held-out evaluation split ($N=440$ studies). Evaluated on 3D Patch Localization (Mean Dice Similarity Coefficient over active patch grids) and Decision AUROC across the 9 abnormality categories.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean Patch DSC | Mean AUROC (9 Classes) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Patch-Supervised 3D ViT-Base** | Li et al. (Tsinghua / Peking Univ.) | Token-aligned 3D Vision Transformer trained with patch-level cross-entropy loss | Held-out Test Split ($N=440$) | **0.422** | **0.892** | [Sci. Data 2026](https://doi.org/10.1038/s41597-026-07793-0) |
| **2** | **Patch-Supervised 3D ResNet-50** | Li et al. (Tsinghua / Peking Univ.) | 3D Residual Network with multi-scale 3D patch prediction heads | Held-out Test Split ($N=440$) | **0.398** | **0.884** | [Sci. Data 2026](https://doi.org/10.1038/s41597-026-07793-0) |
| **3** | **Patch-Supervised 3D DenseNet-121** | Li et al. (Tsinghua / Peking Univ.) | 3D DenseNet with feature map grid pooling matching $24 \times 12 \times 12$ | Held-out Test Split ($N=440$) | **0.387** | **0.876** | [Sci. Data 2026](https://doi.org/10.1038/s41597-026-07793-0) |
| **4** | **CT-CLIP Linear Probe (Transferred)** | Hamamci et al. / Li et al. | Pretrained frozen CT-CLIP visual encoder with linear patch classification probe | Held-out Test Split ($N=440$) | **0.312** | **0.835** | [Sci. Data 2026](https://doi.org/10.1038/s41597-026-07793-0) |
| **5** | **Weakly Supervised CAM Baseline** | Baseline Reference | Standard 3D DenseNet trained on study-level labels only via Class Activation Mapping | Held-out Test Split ($N=440$) | **0.156** | **0.791** | [Sci. Data 2026](https://doi.org/10.1038/s41597-026-07793-0) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Annotation archive size: <500 MB (compressed `.npz` files).
  - High-throughput training feasible on a single **16 GB to 24 GB GPU** (RTX 3090 / RTX 4090) because patch-level loss can be computed directly on downsampled feature maps without rendering memory-heavy full-resolution voxel masks.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install numpy
  import numpy as np

  # Inspect 3D patch grid (24, 12, 12) for a given study
  patch_data = np.load("patch_chestct/train_1003_a_1.npz")
  print(f"Annotated abnormalities: {list(patch_data.keys())}")
  atelectasis_grid = patch_data["atelectasis"]
  print(f"Grid shape: {atelectasis_grid.shape}, Positive patches: {atelectasis_grid.sum()}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Spatial Quantization & Edge Artifacts:* Small focal lesions (e.g. solitary sub-centimeter lung nodules) that cross grid boundaries can be divided between adjacent patches, leading to lower patch-boundary Dice scores.
  2. *Two-Step Data Assembly:* Since image volumes reside on Hugging Face (CT-RATE) and annotations reside on Zenodo, pipelines must rigorously verify volume-ID alignment (`train_XXXX_a_1`) to prevent label misalignment.

---

## 5. Downstream Foundation Model Consumers
The following foundation models utilize PatchChestCT for spatial supervision:
- **CT-CLIP / CT-CHAT** (`docs/02_models/02_volumetric_ct_mri/ct_clip_chat.md`): Used to evaluate and calibrate 3D visual attention maps against physician-reviewed spatial ground truth.
- **CARE-X** (`docs/02_models/02_volumetric_ct_mri/care_x.md`): Auxiliary patch-level supervision and tool-augmented localized measurement.
