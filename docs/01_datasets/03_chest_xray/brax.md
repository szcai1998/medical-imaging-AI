# BRAX: Brazilian Labeled Chest Radiograph Dataset for Multi-Center & Geographic Domain Shift

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D19A]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), Posteroanterior (PA), Anteroposterior (AP), and Lateral projections
- **Target Anatomy & Pathologies:** Thoracic cavity and cardiopulmonary anatomy; 14 radiological findings aligned with the CheXpert ontology (Atelectasis, Cardiomegaly, Consolidation, Edema, Enlarged Cardiomediastinum, Fracture, Lung Lesion, Lung Opacity, No Finding, Pleural Effusion, Pleural Other, Pneumonia, Pneumothorax, Support Devices)
- **Release / Challenge Year:** 2022 (Hospital Israelita Albert Einstein / PhysioNet v1.1.0; Nature Scientific Data)
- **Evidence Code:** `E1` (Peer-Reviewed Nature Scientific Data Descriptor) + `E2` (Official PhysioNet v1.1.0 Release)
- **Access Level:** Credentialed Access / DUA Required (PhysioNet Credentialed Data Use Agreement)
- **Primary Source / Portal:** [https://physionet.org/content/brax/1.1.0/](https://physionet.org/content/brax/1.1.0/) | Paper: [https://doi.org/10.1038/s41597-022-01581-2](https://doi.org/10.1038/s41597-022-01581-2)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Geographic, demographic, and institutional domain shift evaluation. The vast majority of open CXR datasets originate from North America (MIMIC-CXR, CheXpert, NIH-14) or Western Europe (PadChest). BRAX introduces a large-scale South American clinical cohort, enabling rigorous out-of-distribution (OOD) stress testing across distinct patient demographics, equipment calibration standards, and non-English (Brazilian Portuguese) clinical reporting cultures.
- **Target Classes:** 14 canonical observation categories mapped to match CheXpert labels, capturing acute cardiopulmonary conditions, chronic degenerative changes, and postoperative lines/hardware.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **40,967 chest radiograph images**
  - **24,959 radiographic studies / examinations**
  - **19,351 unique patients**
  - **14 standardized binary/uncertain finding labels**
- **Imaging Physics & Acquisition Protocols:**
  - Acquired at Hospital Israelita Albert Einstein in São Paulo, Brazil, between 2018 and 2021.
  - Sourced from clinical digital radiography systems (GE Healthcare, Siemens Healthineers, Philips).
  - High spatial resolution preserved in 16-bit uncompressed DICOM format and high-quality 8-bit PNG images.
- **Multi-Center Distribution:** Sourced from a major private hospital network in South America spanning outpatient imaging, inpatient wards, and emergency departments.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Portuguese NLP Extraction: Extracted from routine clinical free-text radiology reports written in Brazilian Portuguese. The authors developed and validated a domain-adapted NLP pipeline combining a Portuguese translation of CheXpert rules and NegBio negation detection.
  - Clinical Verification: A subset of reports was independently audited by Brazilian board-certified radiologists to evaluate extraction accuracy, demonstrating high precision (>0.90) across major thoracic pathologies.
- **Label Provenance Category:** Automated rule-based NLP extraction from non-English clinical reports, with physician audit validation.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted on PhysioNet (`brax/1.1.0`).
- **License / Terms:** PhysioNet Credentialed Data Use Agreement; non-commercial academic research use only. Requires CITI research certification.
- **Artifact Availability:**
  - Full DICOM volume archives and high-resolution PNG packages (~120 GB).
  - Metadata CSV (`brax2021.csv`) detailing `image_id`, `study_id`, `patient_id`, `ViewPosition`, `PatientAge`, `PatientSex`, and the 14 finding labels.
  - Original de-identified Portuguese report text files.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Completely independent clinical hospital cohort; zero patient, scanner, or institutional overlap with MIMIC-CXR, CheXpert, or NIH-14.
- **Derivative Ecosystem:** Serves as a gold-standard benchmark for out-of-distribution (OOD) robustness and cross-dataset transfer learning studies.
- **Contamination & Overlap Warning:** Because BRAX was released in 2022, older foundation models are untainted by it. However, multi-corpus aggregators (such as Ark+) incorporate BRAX into multi-dataset knowledge accrual training. When reporting BRAX as a test of "zero-shot" or "unseen external" generalization, researchers must audit the model's training manifesto to ensure BRAX was strictly excluded.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the BRAX multi-label thoracic classification benchmark across 14 findings. Evaluated using Mean Area Under the ROC Curve (AUROC) under supervised in-domain and external zero-shot transfer settings.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean AUROC (14 Findings) | Generalization Regime | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Ark+** | Ma et al. (ASU / Mayo Clinic) | Heterogeneous-supervision foundation model accrued across multi-national CXR corpora | Held-out BRAX Test Split ($N=5,000$) | **0.887** | Supervised Multi-Cohort | [MedIA 2026](https://doi.org/10.1016/j.media.2025.103828) |
| **2** | **In-Domain Supervised Baseline** | Reis et al. (Hospital Albert Einstein) | 121-layer DenseNet trained directly on Brazilian cohort with binary cross-entropy | 5-Fold In-Domain Cross-Validation | **0.871** | In-Domain Supervised | [Scientific Data 2022](https://doi.org/10.1038/s41597-022-01581-2) |
| **3** | **MedSigLIP Zero-Shot** | Google Health AI (2025) | Generalist medical vision-language encoder with multi-lingual concept embeddings | Zero-Shot Transfer on BRAX | **0.846** | Zero-Shot OOD | [Model Card / Paper](https://developers.google.com/) |
| **4** | **CheXzero / CLIP Adapter** | Tiu et al. (Stanford University) | Zero-shot image-text contrastive model using translated Portuguese prompt templates | Zero-Shot Transfer on BRAX | **0.828** | Zero-Shot OOD | [Nat Biomed Eng 2022](https://doi.org/10.1038/s41551-022-00936-9) |
| **5** | **NIH-14 Direct Transfer** | Standard DenseNet-121 Baseline | Supervised DenseNet-121 trained on NIH ChestX-ray14 and transferred without fine-tuning | Direct Out-of-the-Box OOD Transfer | **0.792** | Unadapted External OOD | Baseline Comparison |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Full PNG distribution: **~120 GB**.
  - Evaluation batch inference: runs smoothly on a single **12 GB–16 GB VRAM** GPU (e.g., RTX 3060 / RTX 4070) within 15 minutes for the entire test cohort.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow
  import pandas as pd
  from PIL import Image

  # 1. Load BRAX metadata master file
  df = pd.read_csv("brax2021.csv", nrows=10)
  print(f"Loaded {len(df)} records across {len(df.columns)} columns.")
  print(f"Sample findings: {list(df.columns[6:12])}")

  # 2. Check demographic distribution
  print(f"Gender distribution: {df['PatientSex'].value_counts().to_dict()}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *OOD Performance Drop from US Models:* Off-the-shelf models trained on North American datasets (NIH ChestX-ray14, CheXpert) drop **5% to 8% in mean AUROC** when evaluated on BRAX without fine-tuning, demonstrating domain shift caused by demographic variation, scanner calibration differences, and differing thresholding in clinical practice.
  2. *Language Barrier in Zero-Shot Prompting:* Direct application of English CLIP prompts ("a chest x-ray of cardiomegaly") suffers from prompt misalignment when fine-tuning against Portuguese report semantics. Multi-lingual prompt translation or Portuguese concept prompts are necessary to achieve optimal zero-shot transfer.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize BRAX as a geographic robustness and multi-center generalization benchmark:
- **Ark / Ark+** (`docs/02_models/02_cxr/ark.md`): Knowledge accrual across multi-continental cohorts.
- **MedSigLIP** (`docs/02_models/01_general_representation/medsiglip.md`): Evaluated on cross-country and multi-lingual zero-shot transfer.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Validates auditable clinical concept stability across geographic distributions.
- **MedImageInsight** (`docs/02_models/01_general_representation/medimageinsight.md`): Geographic transfer evaluation.
