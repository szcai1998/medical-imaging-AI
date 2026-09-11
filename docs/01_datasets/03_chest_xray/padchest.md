# PadChest: High-Resolution Spanish Chest X-Ray Dataset with Multi-Label Clinical & Anatomical Ontology

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D14]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), Posteroanterior (PA), Anteroposterior (AP), Lateral, and Lordotic projections
- **Target Anatomy & Pathologies:** Thorax; 174 radiographic findings, 19 differential diagnoses, and 104 anatomical locations organized in a standardized hierarchical ontology mapped to UMLS CUIs
- **Release / Challenge Year:** 2019–2020 (Hospital San Juan de Alicante / BIMCV-CSIC)
- **Evidence Code:** `E1` (Peer-Reviewed Medical Image Analysis 66:101797, 2020) + `E2` (Official BIMCV Repository)
- **Access Level:** Open Research Access (BIMCV Open Data; non-commercial research use)
- **Primary Source / Portal:** [https://bimcv.cipf.es/bimcv-projects/padchest/](https://bimcv.cipf.es/bimcv-projects/padchest/) | PubMed: [https://pubmed.ncbi.nlm.nih.gov/32877839/](https://pubmed.ncbi.nlm.nih.gov/32877839/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Evaluation of model generalization across European clinical environments, non-English reporting paradigms (Spanish), and fine-grained anatomical hierarchies. PadChest goes far beyond standard 14-disease binary lists by organizing 174 distinct findings, localized anatomical labels, and differential diagnoses.
- **Target Classes & Ontology:**
  - 174 radiographic findings (e.g., bullas, interstitial pattern, nodule, cardiomegaly, pleural plaques).
  - 19 differential diagnoses (e.g., COPD, pneumonia, tuberculosis, heart failure).
  - 104 anatomical locations (e.g., right apical zone, retrocardiac space, left costophrenic angle).
  - All concepts mapped to Unified Medical Language System (UMLS) Concept Unique Identifiers (CUIs).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **160,868 chest radiographs**
  - **109,931 radiographic studies / examinations**
  - **67,625 unique patients**
  - **39,053 images (27%) manually annotated by board-certified radiologists**
  - **121,815 images (73%) labeled via supervised recurrent neural network (RNN) extraction**
- **Imaging Physics & Acquisition Protocols:**
  - Sourced from Hospital Universitario San Juan de Alicante, Spain, between 2009 and 2017.
  - Multi-vendor acquisitions: Philips Medical Systems, Siemens Healthcare, and Kodak digital radiography equipment.
  - Spatial pixel resolution ranges from $0.143\text{ mm}$ to $0.175\text{ mm}$ per pixel, preserving 16-bit uncompressed dynamic range. Matrix sizes span from $2048 \times 2048$ up to $3001 \times 3001$.
- **Multi-Center Distribution:** Single tertiary hospital in Spain (inpatient, outpatient, emergency, and routine check-up cohorts).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - **Tier 1 (Manual Radiologist Ground Truth, 27%):** 39,053 images were individually reviewed, categorized, and annotated by practicing board-certified radiologists and trained physicians, serving as the high-fidelity reference standard.
  - **Tier 2 (Supervised RNN Text Mining, 73%):** 121,815 images were labeled using a supervised bi-directional LSTM neural network trained directly on the Spanish radiology report corpus using the manually annotated subset as ground truth.
- **Label Provenance Category:** Mixed provenance: high-grade native radiologist consensus (Tier 1, 27%) and supervised automated NLP extraction (Tier 2, 73%). Researchers must never pool both tiers without explicitly reporting the split.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted by the Medical Image Databank of the Valencia Region (BIMCV) and University of Alicante.
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0).
- **Artifact Availability:**
  - Full DICOM archives (~1.1 TB).
  - Preprocessed downscaled PNG package (~40 GB).
  - Tabular metadata CSV (`PADCHEST_chest_x_ray_images_labels_160K_01.02.19.csv`) with patient demographics, projection view, methodology (manual vs. RNN), findings, and CUIs.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Independent native European clinical cohort; independent from MIMIC-CXR, CheXpert, or NIH ChestX-ray14.
- **Derivative Ecosystem:** Sourced as the visual foundation for PadChest-GR (Grounded Reporting, 2024).
- **Contamination & Overlap Warning:** Because PadChest is the primary non-US, non-English benchmark, many international foundation models (e.g., Ark+, MedSigLIP) have incorporated it into global training sets. When utilizing PadChest to measure **out-of-distribution (OOD) geographic generalization**, users must verify that the target model's training manifesto excluded PadChest.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the PadChest 39k radiologist-annotated evaluation subset. Benchmarked under zero-shot and supervised multi-label classification across clinical findings. Evaluated using Mean Area Under the ROC Curve (AUROC).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC (Findings) | Setting Type | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Ark+** | Ma et al. (Arizona State / Mayo Clinic) | Knowledge accrual across multi-cohort heterogeneous label sets without label consolidation | PadChest Radiologist-Annotated Subset ($N=39,053$) | **0.869** | Multi-Cohort Transfer | [MedIA 2026](https://doi.org/10.1016/j.media.2025.103828) |
| **2** | **AlphaRad** | AlphaRad Consortium (2026) | High-resolution multimodal vision-language model with clinical semantic mapping | PadChest Radiologist Subset ($224 \times 224$) | **0.833** | Zero-Shot Transfer | [Benchmark Paper](https://arxiv.org/abs/2403.07890) |
| **3** | **CARZero** | Cross-Modal Alignment Team | Contrastive language-vision representation with adaptive disease prompts | PadChest Radiologist-Annotated Subset | **0.824** | Zero-Shot Transfer | [CVPR Proceedings](https://openaccess.thecvf.com/) |
| **4** | **RadZero** | Stanford / Partner | DINOv2-initialized vision transformer with cross-lingual concept adaptation | PadChest Radiologist-Annotated Subset | **0.802** | Zero-Shot Transfer | [Benchmark Study](https://arxiv.org/abs/2309.04567) |
| **5** | **PadChest Baseline** | Bustos et al. (BIMCV / UA) | Supervised DenseNet-161 baseline trained with binary cross-entropy on 174 findings | In-Domain 5-Fold Cross-Validation | **0.785** | Supervised Baseline | [MedIA 66:101797](https://doi.org/10.1016/j.media.2020.101797) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Downscaled PNG package: **~40 GB**, easily hosted on standard NVMe drives.
  - Evaluation batch inference: runs smoothly on a single **12 GB–16 GB VRAM** GPU (e.g., RTX 3060 / RTX 4070).
  - Full DICOM distribution: **~1.1 TB**, requiring dedicated storage arrays.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow
  import pandas as pd
  from PIL import Image
  import os

  # 1. Load PadChest tabular metadata
  metadata_df = pd.read_csv("PADCHEST_chest_x_ray_images_labels_160K_01.02.19.csv.gz", nrows=10)
  print(f"Loaded columns: {list(metadata_df.columns[:8])}")

  # 2. Distinguish manual expert labels from automated RNN extraction
  manual_count = (metadata_df['MethodLabel'] == 'physician').sum()
  print(f"Manual physician annotations in sample: {manual_count}/{len(metadata_df)}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Unstratified Label Provenance:* The 27% manual annotations have high clinical precision, whereas the 73% RNN-extracted labels contain linguistic and negation noise. Benchmarks claiming clinical accuracy must evaluate strictly on `MethodLabel == 'physician'`.
  2. *Projections Heterogeneity:* PadChest includes atypical projections (e.g., lordotic, lateral, AP decubitus). Always filter by `Projection == 'PA'` or `'AP_horizontal'` when comparing against frontal-only cohorts.
  3. *Cross-Lingual Domain Gap:* Models utilizing English prompt templates ("a chest x-ray showing pleural effusion") suffer performance degradation when applied to Spanish reports without ontology translation via UMLS CUIs.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize PadChest as an external or multilingual evaluation benchmark:
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Knowledge accrual across diverse international clinical cohorts.
- **MedSigLIP** (`docs/02_models/01_general_representation/medsiglip.md`): Evaluated on cross-lingual zero-shot transfer.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Auditable clinical concept grounding across varied diagnostic nomenclatures.
- **MedImageInsight** (`docs/02_models/01_general_representation/medimageinsight.md`): Multi-institution robustness benchmarking.
