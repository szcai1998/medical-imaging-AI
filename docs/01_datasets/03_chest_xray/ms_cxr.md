# MS-CXR: Radiologist-Annotated Phrase-Grounded Image-Sentence Benchmark on MIMIC-CXR

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D17]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), frontal chest radiographs (PA and AP)
- **Target Anatomy & Pathologies:** Thorax; 8 high-level clinical findings (Atelectasis, Cardiomegaly, Consolidation, Edema, Lung Opacity, Pleural Effusion, Pneumonia, Pneumothorax) paired with fine-grained clinical sentences
- **Release / Challenge Year:** 2022 (ECCV 2022) / 2023 (PhysioNet v1.1.0 update)
- **Evidence Code:** `E1` (Peer-Reviewed ECCV 2022 Proceedings) + `E2` (Official PhysioNet Repository)
- **Access Level:** Credentialed Access / DUA Required (Requires approved PhysioNet credentialing for MIMIC-CXR)
- **Primary Source / Portal:** [https://physionet.org/content/ms-cxr/1.1.0/](https://physionet.org/content/ms-cxr/1.1.0/) | Paper: [https://doi.org/10.1007/978-3-031-19830-4_32](https://doi.org/10.1007/978-3-031-19830-4_32) | Code: [https://github.com/microsoft/biovil-t](https://github.com/microsoft/biovil-t)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Phrase grounding in chest radiography evaluates whether vision-language models truly understand clinical sentences by testing their capacity to link natural language phrases (e.g., "right base consolidation concerning for aspiration pneumonia") to exact pixel coordinates. Unlike whole-image classification, phrase grounding evaluates fine-grained visual-semantic alignment and spatial localization.
- **Target Classes & Task Formulation:**
  - 8 core thoracic finding classes represented through natural free-text sentence queries.
  - Spatial bounding box coordinates $[x, y, w, h]$ corresponding to the physical anatomical site described in the sentence.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **1,162 phrase-grounded image-sentence bounding box pairs**
  - **1,026 unique chest radiograph images**
  - **8 high-level thoracic clinical finding categories**
- **Imaging Physics & Acquisition Protocols:**
  - Sourced from a curated subset of frontal chest radiographs in the MIMIC-CXR v2.1.0 database `[D12]`.
  - Bedside portable AP and upright PA projections from Beth Israel Deaconess Medical Center, Boston, MA.
  - Native pixel dimensions preserved from MIMIC-CXR (up to $3056 \times 2544$ pixels); coordinates provided in pixel space relative to original DICOM matrices.
- **Multi-Center Distribution:** Sourced entirely from a single tertiary academic medical center (BIDMC).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Expert Human Annotation: Labeled directly by **board-certified practicing radiologists** affiliated with Microsoft Research Health Futures.
  - Radiologists were presented with individual sentences extracted from the corresponding radiology report and manually drew bounding boxes delineating the specific radiologic visual evidence described by that single sentence.
- **Label Provenance Category:** Native human expert gold standard (zero automated pseudo-labeling, zero NLP heuristic bounding boxes).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted on PhysioNet (`ms-cxr/1.1.0`).
- **License / Terms:** PhysioNet Credentialed Data Use Agreement; strictly non-commercial academic research. Access inherits prerequisites from MIMIC-CXR (CITI certification required).
- **Artifact Availability:**
  - Tabular annotation file: `MS_CXR_Local_Alignment_v1.1.0.csv` specifying `dicom_id`, `label_name`, `label_text`, `x`, `y`, `w`, `h`.
  - Image pixels are retrieved from the parent MIMIC-CXR database using `dicom_id`.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Direct derivative of **MIMIC-CXR v2.1.0 `[D12]`**.
- **Derivative Ecosystem:** Sourced as the primary phrase grounding benchmark for BioViL, BioViL-T, and MAIRA-2.
- **Critical Genealogy Rule & Contamination Warning (Mandatory Section 3A.3 Ground-Layer Audit):**
  - **MIMIC-CXR $\rightarrow$ MS-CXR is a parent $\rightarrow$ derivative relationship.**
  - A model pretrained on MIMIC-CXR and subsequently evaluated on MS-CXR has **NOT** demonstrated independent institutional generalization merely because the phrase-grounding task labels are new.
  - Performance on MS-CXR establishes fine-grained visual-linguistic grounding within the training cohort's institutional and scanner distribution, NOT out-of-distribution robustness.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official MS-CXR 1,162 phrase-grounding benchmark. Evaluated on phrase-to-region spatial alignment using Contrast-to-Noise Ratio (CNR) and mean Intersection over Union (mIoU).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean CNR | Mean Box mIoU | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **MAIRA-2 Grounding Module** | Bannur et al. (Microsoft Research) | RAD-DINO vision backbone + Vicuna-7B v1.5 with explicit coordinate token regression | Official MS-CXR Benchmark ($N=1,162$) | **1.38** | **0.412** | [arXiv:2406.04449](https://arxiv.org/abs/2406.04449) |
| **2** | **BioViL-T** | Bannur et al. (Microsoft Research) | Temporally aware multi-modal transformer with cross-attention and local contrastive alignment | Official MS-CXR Benchmark ($N=1,162$) | **1.14** | **0.364** | [CVPR 2023](https://openaccess.thecvf.com/) |
| **3** | **BioViL** | Boecking et al. (Microsoft Research) | ResNet-50 + CXR-BERT with multi-scale local token-pixel contrastive loss | Official MS-CXR Benchmark ($N=1,162$) | **1.01** | **0.331** | [ECCV 2022](https://doi.org/10.1007/978-3-031-19830-4_32) |
| **4** | **MedBinding** | Academic Research Consortium (2024) | Joint concept-aligned multimodal embedding space with fine-grained token pooling | Official MS-CXR Benchmark ($N=1,162$) | **0.98** | **0.320** | [Benchmark Paper](https://arxiv.org/abs/2402.05432) |
| **5** | **CheXzero / Standard CLIP** | Tiu et al. (Stanford University) | Global image-text contrastive learning baseline with GradCAM-derived activation heatmaps | Official MS-CXR Benchmark ($N=1,162$) | **0.62** | **0.228** | [Nat Biomed Eng 2022](https://doi.org/10.1038/s41551-022-00936-9) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Benchmark metadata CSV: **<50 MB**.
  - Local evaluation requires caching only the 1,026 constituent MIMIC-CXR images (~1 GB in downscaled PNG/JPG format).
  - Complete zero-shot or supervised phrase grounding evaluation runs in under 10 minutes on a single **8 GB VRAM** GPU (e.g., RTX 3070).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow torchvision
  import pandas as pd
  from PIL import Image

  # 1. Load MS-CXR local alignment annotations
  ms_cxr_df = pd.read_csv("MS_CXR_Local_Alignment_v1.1.0.csv")
  print(f"Total pairs: {len(ms_cxr_df)}, Unique images: {ms_cxr_df['dicom_id'].nunique()}")

  # 2. Inspect a phrase grounding sample
  sample = ms_cxr_df.iloc[0]
  print(f"Pathology: {sample['label_name']}")
  print(f"Query text: \"{sample['label_text']}\"")
  print(f"Ground-truth box: [x={sample['x']}, y={sample['y']}, w={sample['w']}, h={sample['h']}]")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Negation Trapping:* Queries containing negative context ("no evidence of pneumothorax" or "clearing of previous effusion") frequently cause naïve attention-based or contrastive models to ground the named anatomical structure rather than recognizing absence.
  2. *Coordinate System Scaling:* Bounding box coordinates in `MS_CXR_Local_Alignment_v1.1.0.csv` are recorded in absolute pixel dimensions relative to original MIMIC-CXR DICOM headers. When resizing images to $512 \times 512$ or $224 \times 224$, coordinates must be scaled proportionally:
     $$x_{\text{scaled}} = x \cdot \left(\frac{W_{\text{new}}}{W_{\text{orig}}}\right), \quad y_{\text{scaled}} = y \cdot \left(\frac{H_{\text{new}}}{H_{\text{orig}}}\right)$$

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize MS-CXR as a primary grounding benchmark:
- **BioViL & BioViL-T** (`docs/02_models/02_cxr/biovil.md`): Baseline and temporal foundation models for phrase grounding.
- **MAIRA-2** (`docs/02_models/02_cxr/maira_2.md`): Evaluates grounded report generation and spatial bounding box predictions.
- **CARE-X** (`docs/02_models/02_cxr/care_x.md`): Evaluates multi-modal spatial reasoning and localized prompt answering.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Audits concept localization accuracy.
