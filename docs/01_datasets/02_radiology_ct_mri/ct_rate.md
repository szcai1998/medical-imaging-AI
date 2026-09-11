# CT-RATE: 3D Chest CT and Radiology Text Multi-Modal Dataset

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D20]`
- **Domain & Modality:** 3D Non-Contrast Chest Computed Tomography (Chest CT) + Paired Free-Text Radiology Reports
- **Target Anatomy & Pathologies:** Thoracic Cavity, Lungs, Mediastinum, Pleura, Cardiovascular Structures; 18 clinical abnormality categories (Cardiomegaly, Consolidation, Pleural Effusion, Atelectasis, Pneumothorax, Lung Nodule/Mass, Emphysema, Fibrosis, etc.)
- **Release / Publication Year:** 2024–2026 (*Nature Biomedical Engineering*, 2026)
- **Evidence Code:** `E1` (Peer-Reviewed Journal Version-of-Record in *Nature Biomedical Engineering*) + `E2` (Official Hugging Face Dataset Card)
- **Access Level:** Gated / Research-only under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) via Hugging Face
- **Primary Source / Portal:** [Nature Biomedical Engineering (Hamamci et al., 2026)](https://doi.org/10.1038/s41551-025-01599-y) | [Hugging Face Dataset: ibrahimhamamci/CT-RATE](https://huggingface.co/datasets/ibrahimhamamci/CT-RATE) | [GitHub: CT-CLIP](https://github.com/ibrahimhamamci/CT-CLIP)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Medical vision-language pretraining historically relied almost exclusively on 2D chest radiographs (e.g., MIMIC-CXR). However, complex thoracic pathologies—such as subtle pulmonary nodules, interstitial lung disease, and vascular calcifications—require full 3D volumetric context. CT-RATE provides the first open, health-system-scale 3D chest CT–report corpus to overcome this 2D bottleneck.
- **Target Classes & Scope:** 18 binary abnormality labels extracted from radiology reports, accompanied by comprehensive unconstrained clinical reports (Findings and Impression sections).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **21,304 unique patients**
  - **25,692 non-contrast chest CT examinations/studies**
  - **50,188 reconstructed 3D volumes**
  - *Counting Unit Discipline:* The expansion from 25,692 studies to 50,188 volumes reflects multiple reconstruction kernels (e.g., sharp lung kernel vs. smooth soft-tissue/mediastinal kernel) applied to identical physical scan acquisitions. The 50,188 volumes must **never** be cited as 50,188 independent patient encounters.
- **Imaging Physics & Acquisition Protocols:**
  - Multi-detector helical CT (MDCT) scanners.
  - Non-contrast thoracic examinations. Variable axial slice thickness ($0.75\text{ mm}$ to $5.0\text{ mm}$), with an in-plane matrix of $512 \times 512$ voxels.
  - Standardized preprocessed volumes are commonly resampled to $240 \times 424 \times 424$ or $64 \times 128 \times 128$ for 3D Vision Transformer embedding.
- **Multi-Center Distribution:** Sourced from Istanbul University-Cerrahpasa medical archive, representing a high-volume clinical hospital environment.

### [T] Truth & Annotation Provenance
- **Annotation & Extraction Protocol:**
  - Free-text narrative reports originally dictated in Turkish by board-certified radiologists during standard clinical care.
  - Translated into English via a customized, radiologist-verified translation pipeline.
  - 18 binary abnormality target labels were automatically extracted from the free-text reports using modified CheXbert / LLM extraction pipelines, validated on an expert radiologist-curated test subset.
- **Label Provenance Category:** Native human clinical reports + automated report-mined label extraction.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official distribution via Hugging Face (`ibrahimhamamci/CT-RATE`).
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0). Requires Hugging Face user login, clinical intent declaration, and credential acceptance.
- **Artifact Availability:** Preprocessed 3D NIfTI volumes (`.nii.gz`), raw reconstructions, original Turkish and translated English reports (CSV/JSON), binary abnormality label matrices, and official patient-disjoint train/validation/test splits.

### [S] Systemic Lineage & Genealogy
- **Parent Cohort Status:** CT-RATE is the primary parent cohort for an expanding downstream spatial derivative ecosystem:
  - **RadGenome-ChestCT `[D21]`:** Introduces 197 model-assisted segmentation categories, 665k grounded reports, and 1.2M/1.3M grounded VQA pairs on the CT-RATE scans.
  - **PatchChestCT `[D22]`:** Adds physician-reviewed 3D patch-level spatial annotations for 9 abnormalities across 2,201 CT-RATE studies.
- **Contamination & Overlap Warning:** Foundation models pretrained on CT-RATE cannot claim zero-shot generalization when benchmarked against RadGenome-ChestCT or PatchChestCT, because the underlying physical scans and patients are identical. True external validation requires independent cohorts such as **RAD-ChestCT `[D31]`**.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the CT-RATE official held-out test split ($N=3,260$ volumes from patient-disjoint holdouts). Evaluated on 18-abnormality classification (Mean AUROC and Macro-F1) and cross-modal retrieval.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC (18 Classes) | Macro-F1 Score | Text-to-Image R@5 | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **CT-CLIP (Fine-Tuned / LoRA)** | Hamamci et al. (ETH Zurich / TUM) | 3D ViT-B/16 Vision Transformer + ClinicalBERT with parameter-efficient fine-tuning | Official Held-out Test Split ($N=3,260$) | **0.947** | **0.762** | **58.4%** | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **2** | **CT-CLIP (Zero-Shot)** | Hamamci et al. (ETH Zurich / TUM) | Native volumetric contrastive image-text pretraining with prompt pair engineering | Official Held-out Test Split ($N=3,260$) | **0.898** | **0.684** | **52.1%** | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **3** | **CT-CHAT** | Hamamci et al. (ETH Zurich / TUM) | 3D CT-CLIP visual encoder + Vicuna-7B LLM backbone via projection adapter | Official Held-out Test Split ($N=3,260$) | **0.885** | **0.661** | *N/A* (Gen) | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **4** | **MedGemma 1.5 4B** | Google Health AI | Multimodal autoregressive Transformer with native 3D patch tokenization | CT-RATE Validation Split (Zero/Few-Shot) | *N/A* | **0.270** | *N/A* (Gen) | [MedGemma Model Card](https://developers.google.com/health-ai-developer-foundations/medgemma/model-card) |
| **5** | **Supervised 3D DenseNet Baseline** | MIC-DKFZ / Organizers | Standard 3D DenseNet-121 trained from scratch on 18 binary abnormality labels | Official Held-out Test Split ($N=3,260$) | **0.742** | **0.518** | *N/A* | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Full dataset size: ~1.5 TB compressed, >3 TB uncompressed NIfTI files.
  - Inference with pre-trained 3D ViT encoders (FP16): requires a minimum of **16 GB VRAM** (e.g., RTX 4080 / RTX 3090).
  - Volumetric contrastive pretraining from scratch requires distributed 80 GB A100/H100 clusters due to 3D patch memory footprints.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel pandas
  import nibabel as nib
  import pandas as pd

  # Load sample metadata and inspect a 3D chest CT volume
  metadata = pd.read_csv("ct_rate/train_reports.csv")
  scan = nib.load("ct_rate/dataset/train/train_1/train_1_a.nii.gz")
  print(f"Shape: {scan.shape}, Voxel Spacing: {scan.header.get_zooms()}")
  print(f"Sample English Report: {metadata.loc[0, 'Findings_EN'][:120]}...")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Kernel-Induced Feature Shifts:* Smooth soft-tissue kernels and sharp lung kernels from the same examination yield different high-frequency texture features; models not regularized against kernel variance may predict divergent risk scores for identical patients.
  2. *Non-Contrast Diagnostic Boundaries:* As non-contrast CT scans, these volumes cannot definitively rule out acute pulmonary embolism or detailed vascular lumen stenosis.
  3. *Translation Nuance:* Some nuanced radiological expressions translated from Turkish may carry slight phrasing shifts compared to native Anglo-American dictations.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages in `docs/02_models/` directly utilize CT-RATE for pretraining or benchmark evaluation:
- **CT-CLIP / CT-CHAT** (`docs/02_models/02_volumetric_ct_mri/ct_clip_chat.md`): Core native pretraining corpus and benchmark.
- **MedGemma 1.5 4B** (`docs/02_models/02_volumetric_ct_mri/medgemma_1_5.md`): Evaluated on CT-RATE validation split for multi-label report generation.
- **Comparative CT FMs (Curia, SPECTRE, UMedPT, TAP-CT)** (`docs/02_models/02_volumetric_ct_mri/ct_comparators.md`): Evaluated in frozen feature extractor comparisons [S81].
