# Merlin Abdominal CT: Multi-Task Abdominal & Pelvic CT Vision-Language Dataset

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D30]`
- **Domain & Modality:** 3D Abdominal & Pelvic Computed Tomography (Contrast & Non-Contrast CT) + Paired Free-Text Clinical Radiology Reports + Structured EHR Diagnosis Codes
- **Target Anatomy & Pathologies:** Whole Abdomen and Pelvis (Liver, Gallbladder, Biliary Tree, Spleen, Kidneys, Pancreas, Adrenals, Gastrointestinal Tract, Peritoneum, Retroperitoneum, Pelvic Organs, Abdominal Vasculature, Musculoskeletal Structures); benchmarked across 752 clinical diagnostic, prognostic, and quality tasks
- **Release / Publication Year:** March 2026 (*Nature* 652, 1318–1328, DOI: 10.1038/s41586-026-10181-8)
- **Evidence Code:** `E1` (Peer-Reviewed Journal Version-of-Record in *Nature*)
- **Access Level:** Gated Access via Stanford Center for Artificial Intelligence in Medicine & Imaging (Stanford AIMI); Data Use Agreement (DUA) required
- **Primary Source / Portal:** [Nature 2026 Paper (Blankemeier et al.)](https://doi.org/10.1038/s41586-026-10181-8) | [Stanford AIMI Dataset Portal](https://aimi.stanford.edu/) | [Hugging Face: stanfordmimi/Merlin](https://huggingface.co/stanfordmimi/Merlin) | [GitHub: stanfordmimi/Merlin](https://github.com/stanfordmimi/Merlin)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Most large-scale 3D vision-language initiatives concentrate on chest CT (e.g., CT-RATE). However, abdominal and pelvic CT scans represent a massive fraction of emergency and oncology radiology workloads, demanding multi-organ reasoning across complex, soft-tissue contrast phases. Merlin provides the primary anatomical and site counterweight to chest-only cohorts, establishing an image–text–code foundation for abdominal diagnostics.
- **Target Scope:** 752 diagnostic, opportunistic screening, and prognostic prediction tasks formulated across all abdominal organ systems.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **18,317 unique patients**
  - **25,494 released abdominal/pelvic CT scans**
  - **25,494 paired clinical radiology reports**
  - *Counting Unit Discipline:* In the *Nature* 2026 publication, the authors distinguish between:
    1. The **released open dataset cohort** (25,494 CT scans from 18,317 unique patients),
    2. The model's internal pretraining corpus (15,331 CT scans / >6M slices paired with >1.8M diagnosis codes and >6M report tokens),
    3. The internal test cohort (5,137 scans), and
    4. The multi-source external validation cohorts (44,098 scans).
    These numbers must **never** be conflated or used interchangeably.
- **Imaging Physics & Acquisition Protocols:**
  - Multi-detector helical CT scanners (GE, Siemens, Philips, Canon).
  - Contrast-enhanced (CECT) arterial, portal venous, and delayed phases, alongside non-contrast scans.
  - Slice thickness variable ($0.625\text{ mm}$ to $5.0\text{ mm}$), axial matrix $512 \times 512$. Standard preprocessed inputs resampled to isotropic volumes.
- **Multi-Center Distribution:** Sourced from Stanford Health Care and affiliated outpatient diagnostic networks.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - Full-text clinical narrative radiology reports dictated by board-certified academic abdominal radiologists, fellows, and residents during routine clinical encounters.
  - Paired with structured ICD-9 and ICD-10 diagnostic billing codes extracted from electronic health records (EHR).
- **Label Provenance Category:** Native human expert clinical reports and EHR billing/diagnostic codes (zero synthetic or LLM-generated reports).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Available via Stanford AIMI upon execution of a Data Use Agreement (DUA).
- **License / Terms:** Stanford AIMI Research Data Use Agreement (non-commercial academic research only).
- **Artifact Availability:** 3D CT volumes (DICOM/NIfTI), de-identified narrative reports (Findings, Impression), structured ICD diagnostic labels, and evaluation task split definitions.

### [S] Systemic Lineage & Genealogy
- **Institutional Independence:** Originates from Stanford Health Care, completely independent of the Turkish cohort in CT-RATE `[D20]` and the Duke cohort in RAD-ChestCT `[D31]`.
- **Systemic Role:** Serves as an anatomical and institutional counterweight for validating whether 3D foundation models transfer beyond thoracic boundaries.
- **Contamination Warning:** When evaluating foundation models on public abdominal segmentation benchmarks (e.g., AMOS22, KiTS23, AbdomenAtlas), researchers must verify whether any constituent cohorts overlap with Stanford clinical archives.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the Merlin multi-task evaluation protocol and the independent comparative CT foundation model benchmark from Tagscherer et al. (August 2026) [S81]. Evaluated on Mean AUROC across organ-level diagnostic tasks.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean Diagnostic AUROC | Balanced Accuracy | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Merlin 3D VLM (Fine-Tuned)** | Blankemeier et al. (Stanford University) | 3D Volumetric Vision Transformer + ClinicalBERT text encoder | Internal Test Cohort ($N=5,137$ scans) | **0.884** | **81.2%** | [Nature 652 (2026)](https://doi.org/10.1038/s41586-026-10181-8) |
| **2** | **Merlin 3D VLM (Zero-Shot)** | Blankemeier et al. (Stanford University) | Contrastive 3D vision-language representation with prompt engineering | External Validation Cohort ($N=44,098$ scans) | **0.832** | **77.4%** | [Nature 652 (2026)](https://doi.org/10.1038/s41586-026-10181-8) |
| **3** | **SPECTRE (Frozen Probe)** | Tagscherer et al. (Independent Audit) | 3D CT foundation model evaluated as frozen feature extractor | Independent Comparative Benchmark ($N=1,200$) | **0.826** | **76.8%** | [Int. J. CARS 2026](https://doi.org/10.1007/s11548-026-03786-x) |
| **4** | **TAP-CT (Frozen Probe)** | Tagscherer et al. (Independent Audit) | Volumetric CT masked autoencoder representation backbone | Independent Comparative Benchmark ($N=1,200$) | **0.814** | **75.1%** | [Int. J. CARS 2026](https://doi.org/10.1007/s11548-026-03786-x) |
| **5** | **2D Slice-Averaged ResNet Baseline** | Stanford Baseline Reference | Standard 2D ResNet-50 with mean slice pooling across axial volume | Internal Test Cohort ($N=5,137$ scans) | **0.761** | **69.8%** | [Nature 652 (2026)](https://doi.org/10.1038/s41586-026-10181-8) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Released cohort dataset footprint: ~2.2 TB.
  - Frozen feature extraction with Merlin: highly feasible on a single **16 GB to 24 GB GPU** (RTX 4080 / RTX 3090).
  - Volumetric pretraining across 752 tasks requires high-performance multi-node compute clusters (minimum 8 $\times$ 80 GB A100/H100).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install SimpleITK pandas
  import SimpleITK as sitk
  import pandas as pd

  # Load metadata and read an abdominal CT scan volume
  df = pd.read_csv("merlin/released_cohort_metadata.csv")
  image = sitk.ReadImage("merlin/scans/patient_18317_scan_1.nii.gz")
  print(f"Volume Size: {image.GetSize()}, Spacing: {image.GetSpacing()}")
  print(f"Primary Indication: {df.loc[0, 'clinical_indication']}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Focal vs. Diffuse Lesion Performance Gap:* Independent comparative evidence (Tagscherer et al., 2026 [S81]) demonstrated that frozen CT foundation representations struggle significantly more with small focal abnormalities (e.g. solitary liver cysts, subcentimeter adrenal nodules) than with diffuse organ pathology (e.g. hepatomegaly, steatosis).
  2. *Contrast Phase Discrepancy:* Classifying hypervascular liver lesions (e.g. focal nodular hyperplasia) requires arterial phase scans; attempting classification on portal venous or non-contrast phases degrades diagnostic sensitivity.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize or benchmark against the Merlin dataset:
- **Merlin** (`docs/02_models/02_volumetric_ct_mri/merlin.md`): Landmark 3D abdominal CT vision-language foundation model [S12].
- **Comparative CT FMs (SPECTRE / TAP-CT / CT-FM / UMedPT)** (`docs/02_models/02_volumetric_ct_mri/ct_comparators.md`): Evaluated in frozen feature extractor comparisons across organ abnormality benchmarks [S81].
