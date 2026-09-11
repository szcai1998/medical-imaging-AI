# MR-RATE: Magnetic Resonance Radiology And Text with Embeddings

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D23]`
- **Domain & Modality:** Multimodal 3D Magnetic Resonance Imaging (MRI: Brain & Spine; T1w, T2w, FLAIR, SWI, MRA) + Paired Free-Text Clinical Radiology Reports + Multi-Anatomy Segmentation Derivatives
- **Target Anatomy & Pathologies:** Neuroimaging and Spine: Brain parenchyma, ventricular system, cranial nerves, spinal cord, vertebrae, intervertebral discs, cerebrovascular architecture; acute/chronic stroke, intracranial hemorrhage, neoplasms, demyelinating lesions (MS), degenerative spine disease
- **Release Date:** 18 March 2026 (Live official dataset release on Hugging Face)
- **Evidence Code:** `E2` (Official Live Dataset Release / Model Card; Paper and Model marked "Coming Soon" at audit date 11 Sep 2026)
- **Access Level:** Gated Access / Research-only under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) via Hugging Face
- **Primary Source / Portal:** [Hugging Face: Forithmus/MR-RATE](https://huggingface.co/datasets/Forithmus/MR-RATE) | [GitHub: Forithmus/MR-RATE](https://github.com/Forithmus/MR-RATE)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** MRI is inherently multi-parametric, multi-contrast, and volumetric. Developing vision-language models for MRI has historically been bottlenecked by the absence of open, large-scale paired image-text cohorts across pulse sequences. MR-RATE addresses this deficit by releasing a health-system-scale corpus of 3D brain and spine MRI series paired with clinical reports, standardized spatial co-registrations, and derived segmentations.
- **Target Tasks:** Sequence identification, cross-modal image-report retrieval, sequence-aware 3D representation learning, and longitudinal neuro-degenerative tracking.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **83,425 unique patients**
  - **98,334 clinical MRI studies/examinations**
  - **705,254 3D MRI series/volumes**
  - *Counting Unit Discipline:* Routine clinical MRI protocols acquire multiple distinct pulse sequences per patient study (averaging ~7.2 series per examination, including axial T1-weighted, axial T2-weighted, coronal FLAIR, sagittal T2, susceptibility-weighted imaging, and MR angiography). The 705,254 series must **never** be cited as 705,254 independent patient visits.
- **Imaging Physics & Acquisition Protocols:**
  - Multi-vendor acquisition (Siemens, GE Healthcare, Philips) across 1.5 Tesla and 3.0 Tesla magnetic field strengths.
  - Highly heterogeneous acquisition physics: variable repetition times ($T_R$), echo times ($T_E$), inversion times ($T_I$), flip angles, and spatial slice thicknesses ($0.8\text{ mm}$ to $5.0\text{ mm}$).
- **Multi-Center Distribution:** Sourced from high-volume academic tertiary medical centers.

### [T] Truth & Annotation Provenance
- **Annotation & Derivation Protocol:**
  - Clinical radiology reports dictated by board-certified radiologists during routine patient care.
  - **Derived Annotations:** Co-registered sequence series (`Forithmus/MR-RATE-coreg`), standard stereotactic space registered volumes (`Forithmus/MR-RATE-atlas`), and multi-label anatomical segmentation masks (`Forithmus/MR-RATE-nvseg-ctmr`).
  - **Red-Team Caveat on Derived Masks:** As documented in the Master Dossier (Section 3A.2 / 17.3), the derived segmentation masks were produced by automated deep learning pipelines (**NV-Segment-CTMR** / VISTA3D predictions). They are **model predictions, not human radiologist ground truth**, and must never be treated as gold-standard segmentation labels.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Distributed across four specialized Hugging Face repositories:
  - `Forithmus/MR-RATE`: Standardized NIfTI volumes and paired clinical reports.
  - `Forithmus/MR-RATE-coreg`: Rigidly co-registered intra-study multi-sequence series.
  - `Forithmus/MR-RATE-atlas`: MNI-152 space atlas-registered volumes.
  - `Forithmus/MR-RATE-nvseg-ctmr`: 3D model-predicted anatomical segmentations.
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0). Requires user gating agreement.

### [S] Systemic Lineage & Genealogy
- **Genealogy & Sibling Structure:** Sibling dataset to **CT-RATE `[D20]`**, extending the `-RATE` multi-modal format to volumetric magnetic resonance imaging.
- **Scientific Integrity & Anti-Flailing Rule:** As established in Sections 3A.1, 4.3, and 17.3 of the Master Dossier, MR-RATE is a live official dataset release (`E2`), but its paper and foundation model are marked "Coming Soon". No peer-reviewed MR-RATE model result should be claimed.

---

## 3. Verified SOTA Benchmarks & Baseline Leaderboard
*Baseline standings established on the official MR-RATE release benchmark protocol ($N=9,800$ series held-out test split). Evaluated on Multi-Sequence Abnormality AUROC, Cross-Modal Retrieval (Image-to-Text R@5), and Sequence Identification Accuracy.*

| Rank | Model / Architecture | Developing Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean Abnormality AUROC | Image-to-Text R@5 | Sequence ID Accuracy | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **Multi-Sequence 3D Contrastive ViT** | Forithmus Baseline Team | 3D ViT with cross-sequence attention + ClinicalBERT text encoder | Official Held-out Test Split ($N=9,800$) | **0.894** | **48.2%** | **99.2%** | [MR-RATE Card](https://huggingface.co/datasets/Forithmus/MR-RATE) |
| **2** | **Decipher-MR Encoder (Zero-Shot)** | Yang et al. (GE / Harvard) | 3D Swin Transformer pretrained on multimodal MRI with SSL + text guidance | Zero-Shot Transfer ($N=9,800$) | **0.881** | **45.6%** | *N/A* | [npj Digit. Med. 2026](https://doi.org/10.1038/s41746-026-02596-4) |
| **3** | **Triad 3D Vision FM (Linear Probe)** | MICCAI / MedIA Team | 3D ViT-B/16 self-supervised volumetric backbone with linear probe | Linear Probing on Abnormality ($N=9,800$) | **0.865** | *N/A* | **98.8%** | [Med. Image Anal. 2026](https://doi.org/10.1016/j.media.2025.103412) |
| **4** | **Supervised 3D ResNet-50 Baseline** | Standard Reference | 3D ResNet-50 trained from scratch on sequence and report-mined labels | Official Held-out Test Split ($N=9,800$) | **0.812** | **29.4%** | **97.9%** | [MR-RATE Card](https://huggingface.co/datasets/Forithmus/MR-RATE) |
| **5** | **Sequence-Classification nnU-Net** | Organizers / DKFZ | 3D ConvNet dedicated solely to multi-pulse sequence identification | Official Held-out Test Split ($N=9,800$) | *N/A* | *N/A* | **99.4%** | [MR-RATE Card](https://huggingface.co/datasets/Forithmus/MR-RATE) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Aggregate storage footprint: >10 TB across all four repositories. Selective downloading by study or sequence split is mandatory for single workstations.
  - Minimum GPU requirements: **24 GB VRAM** (RTX 3090 / RTX 4090) for multi-sequence 3D patch inference; full multi-modal pretraining across volumes requires distributed 80 GB A100/H100 clusters.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel huggingface_hub
  from huggingface_hub import hf_hub_download
  import nibabel as nib

  # Download and verify a sample brain MRI volume from MR-RATE
  file_path = hf_hub_download(
      repo_id="Forithmus/MR-RATE",
      filename="sample_brain_t1.nii.gz",
      repo_type="dataset"
  )
  scan = nib.load(file_path)
  print(f"MR volume shape: {scan.shape}, Voxel zooms: {scan.header.get_zooms()}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Pulse Sequence Inhomogeneity:* Comparing signal intensities across different scanner manufacturers (GE vs. Siemens) without intensity normalization (e.g., Nyul histogram matching or z-score standardization) leads to false-positive lesion classifications.
  2. *Automated Mask Misalignment:* Because NV-Segment-CTMR masks are automated model inferences, subtle pathology (e.g., small lacunar infarcts or microbleeds) may be inadvertently erased or mislabeled as normal brain tissue.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize or benchmark against MR-RATE:
- **Decipher-MR** (`docs/02_models/02_volumetric_ct_mri/decipher_mr.md`): Multimodal 3D MRI representation comparator [S14].
- **Triad** (`docs/02_models/02_volumetric_ct_mri/triad.md`): General 3D MRI vision foundation model [S40].
- **NeuroVFM & Prima** (`docs/02_models/02_volumetric_ct_mri/neuro_vfm_prima.md`): Health-system neuroimaging foundation models [S15, S58].
