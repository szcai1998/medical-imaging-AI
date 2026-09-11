# MIMIC-CXR v2.1.0: Large-Scale Chest Radiograph & Free-Text Radiology Report Corpus

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D12]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), frontal (PA/AP) and lateral projections
- **Target Anatomy & Pathologies:** Thoracic cavity, lungs, pleura, cardiac silhouette, mediastinum, bones; 14 canonical radiologic observations plus comprehensive free-text report findings
- **Release / Challenge Year:** 2019 (v2.0.0 initial release) / 2020 (v2.1.0 update)
- **Evidence Code:** `E1` (Peer-Reviewed Scientific Data Descriptor) + `E2` (Official PhysioNet Repository)
- **Access Level:** Credentialed Access / Data Use Agreement (DUA) required (PhysioNet Credentialed)
- **Primary Source / Portal:** [https://physionet.org/content/mimic-cxr/2.1.0/](https://physionet.org/content/mimic-cxr/2.1.0/) | Compressed JPG: [https://physionet.org/content/mimic-cxr-jpg/2.1.0/](https://physionet.org/content/mimic-cxr-jpg/2.1.0/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Unstructured clinical radiology reports paired with multi-view radiographs represent the richest modality for developing automated radiology report generation (RRG), visual question answering (VQA), and multi-modal foundation models. MIMIC-CXR captures routine emergency department and intensive care unit (ICU) workflows at a major tertiary academic hospital.
- **Target Classes / Phenotypes:**
  - 14 structured observation labels derived via CheXpert and NegBio annotators (Atelectasis, Cardiomegaly, Consolidation, Edema, Enlarged Cardiomediastinum, Fracture, Lung Lesion, Lung Opacity, No Finding, Pleural Effusion, Pleural Other, Pneumonia, Pneumothorax, Support Devices).
  - Unconstrained free-text clinical Findings and Impressions sections reflecting acute, chronic, postoperative, and device-related findings.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **377,110 DICOM images**
  - **227,835 radiographic studies / examinations**
  - **65,379 unique patients**
  - **227,827 paired free-text radiology reports**
- **Imaging Physics & Acquisition Protocols:**
  - Acquired in clinical practice at Beth Israel Deaconess Medical Center (BIDMC) in Boston, MA between 2011 and 2016.
  - Multi-detector bedside mobile units (computed radiography [CR] and digital radiography [DX]) for critically ill ICU patients, alongside standing upright PA and lateral systems in ambulatory/ED suites.
  - Matrix dimensions range up to $3056 \times 2544$ pixels with 12-to-14-bit grayscale bit depth, preserving high-dynamic-range soft tissue and bone attenuation profiles.
- **Multi-Center Distribution:** Single-institution, multi-departmental tertiary medical center cohort (emergency medicine, intensive care, inpatient wards).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - **Narrative Reports:** Sourced directly from hospital PACS/RIS archives, dictated by radiology residents/fellows and signed off by board-certified attending thoracic radiologists during routine patient care. De-identified using rigorous HIPAA-compliant rule-based algorithms.
  - **Structured Labels:** Extracted retrospectively from reports using two rule-based NLP tools: the Stanford CheXpert labeler and NegBio. Labels are categorized into 1 (Positive), 0 (Negative), -1 (Uncertain), or blank (Unmentioned).
- **Label Provenance Category:** Routine clinical free-text reports (attending radiologist sign-off) combined with automated weak NLP extraction. Notice: weak labels contain an estimated 5–10% error margin; reports contain normal variation in dictation style, hedging, and incidental findings.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official hosting on PhysioNet under the PhysioNet Credentialed Data Use Agreement.
- **Access Prerequisites:** Requires CITI training ("Data or Specimens Only Research"), identity verification, and signed adherence to the PhysioNet DUA prohibiting re-identification or redistribution.
- **Artifact Availability:**
  - Full DICOM corpus (~4.7 TB uncompressed).
  - MIMIC-CXR-JPG derivative (~550 GB full-resolution PNG/JPG or ~50 GB downscaled $512 \times 512$ version).
  - Tabular metadata: `cxr-record-list.csv`, `cxr-study-list.csv`, `mimic-cxr-2.0.0-chexpert.csv`, `mimic-cxr-2.0.0-negbio.csv`, and split files (`mimic-cxr-ehr-split.csv`).

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Associated with the broader MIMIC (Medical Information Mart for Intensive Care) clinical ecosystem (MIMIC-IV), allowing multi-modal linkage with vital signs, medications, and laboratory values.
- **Derivative Ecosystem:** Serves as the parent patient cohort for spatial and semantic derivatives:
  - **MS-CXR `[D17]`**: 1,162 phrase-grounded image-sentence bounding box pairs curated on a MIMIC-CXR subset.
  - **Chest ImaGenome `[D18]`**: 242,072 automatically generated scene graphs plus a 500-patient gold consensus set.
- **Contamination & Overlap Warning:** Almost every contemporary medical vision-language foundation model (including RAD-DINO, MAIRA-1/2, MedVersa, Med-PaLM M, CARE-X, and CLEAR) includes MIMIC-CXR in its pretraining corpus. Zero-shot or external evaluation of these models on MIMIC-CXR or its derivatives (MS-CXR, Chest ImaGenome) measures **in-domain transfer and memorization**, NOT true external generalization.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official MIMIC-CXR test set split ($N=3,858$ studies / 3,269 patients). Evaluated primarily on automated clinical factuality (RadGraph-F1, CheXbert-F1) and natural language generation (BLEU-4). RadGraph-F1 is the authoritative clinical factuality metric.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | RadGraph-F1 | BLEU-4 | CheXbert Macro-F1 | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **MAIRA-2** | Microsoft Research Health Futures | Frozen RAD-DINO vision encoder + Vicuna-7B v1.5 with multi-image context (current frontal/lateral + prior study + clinical history) | Official MIMIC-CXR Test Set ($N=3,858$) | **0.388** | **0.243** | **0.542** | [arXiv:2406.04449](https://arxiv.org/abs/2406.04449) |
| **2** | **MedVersa** | Blankemeier et al. (Stanford AIMI) | Multi-task generalist medical VLM unifying classification, segmentation, and report generation via coordinate tokens | Official MIMIC-CXR Test Set ($N=3,858$) | **0.354** | **0.221** | **0.518** | [arXiv:2405.07988](https://arxiv.org/abs/2405.07988) |
| **3** | **LLaVA-Rad** | Microsoft / Stanford | LLaVA-style architecture fine-tuned with domain-specific instruction templates and CLIP vision backbone | Official MIMIC-CXR Test Set ($N=3,858$) | **0.312** | **0.165** | **0.485** | [Model Card / Paper](https://arxiv.org/abs/2404.09990) |
| **4** | **CXR-RePaiR** | Endo et al. (Stanford ML Group) | Contrastive retrieval-based report generation using text-image embeddings to assemble sentence banks | Official MIMIC-CXR Test Set ($N=3,858$) | **0.286** | **0.138** | **0.461** | [EMNLP 2021](https://aclanthology.org/2021.emnlp-main.751/) |
| **5** | **R2GenCMN** | Chen et al. (Harbin Institute of Tech.) | Memory-driven cross-modal attention network with aligned visual-textual memory blocks | Official MIMIC-CXR Test Set ($N=3,858$) | **0.241** | **0.106** | **0.413** | [ACL 2021](https://aclanthology.org/2021.findings-acl.199/) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Full DICOM distribution: **~4.7 TB** (requires high-capacity NAS or high-speed NVMe array).
  - Preprocessed `mimic-cxr-jpg` (downscaled $512 \times 512$): **~50 GB**, feasible for local development on consumer NVMe drives.
  - Linear probe or feature extraction with frozen foundation encoders (RAD-DINO, BioViL): easily executed on a single **16 GB–24 GB VRAM** GPU (e.g., RTX 4080 / RTX 3090).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pydicom pillow pandas torchvision
  import pydicom
  from PIL import Image
  import pandas as pd
  import numpy as np

  # 1. Load clinical metadata record
  record_df = pd.read_csv("mimic-cxr-2.0.0-metadata.csv.gz", nrows=5)
  print(f"Loaded {len(record_df)} metadata rows across columns: {list(record_df.columns[:6])}")

  # 2. Inspect DICOM header and photometric interpretation
  dcm = pydicom.dcmread("p10/p10000032/s50414267/02aa804e-bde0afdd-112c0b34-7bc16c75-3e14cc6b.dcm")
  print(f"Modality: {dcm.Modality}, View: {getattr(dcm, 'ViewPosition', 'Unknown')}, Shape: {dcm.pixel_array.shape}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Patient-Level Data Leakage:* Patients in MIMIC-CXR frequently undergo serial radiography over days to years. **Splitting must strictly be performed on `subject_id`**, NEVER on `study_id` or `dicom_id`. Random image-level splitting leaks longitudinal disease patterns and artificially inflates AUROC/BLEU.
  2. *View Conflation (AP vs. PA vs. Lateral):* Supine bedside AP films magnify the cardiac silhouette due to beam divergence, frequently causing false-positive cardiomegaly predictions if view position is ignored.
  3. *Boilerplate Inflation on NLG Metrics:* Standard n-gram metrics (BLEU-1 to BLEU-4) are easily gamed by predicting generic negative boilerplate ("The lungs are clear. Cardiomediastinal silhouette is normal."). Evaluation must incorporate **RadGraph-F1** and clinical factuality benchmarks (GREEN, RadFact).

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize MIMIC-CXR as a primary pretraining or transfer benchmark:
- **RAD-DINO** (`docs/02_models/02_cxr/rad_dino.md`): Self-supervised representation backbone trained on MIMIC-CXR.
- **MAIRA-2** (`docs/02_models/02_cxr/maira_2.md`): Grounded multi-image report generation model leveraging MIMIC-CXR studies and longitudinal context.
- **CARE-X** (`docs/02_models/02_cxr/care_x.md`): Auxiliary supervision and grounded reasoning foundation model.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Auditable clinical concept decomposition model.
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Supervised multi-cohort knowledge accrual framework.
