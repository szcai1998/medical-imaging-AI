# Retinal OCT & Longitudinal Fundus Cohorts: OCTCube-M & RETFound Plus

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D-RETINA]` / Retinal OCT & Longitudinal Fundus Cohorts
- **Domain & Modality:** 3D Spectral-Domain & Swept-Source Optical Coherence Tomography (SD-OCT / SS-OCT), Paired Infrared (IR) / En-Face (EF) Reflectance, and Longitudinal Color Fundus Photography (CFP)
- **Target Anatomy & Pathologies:** Neurosensory retina, Retinal Pigment Epithelium (RPE), choroid, optic nerve head, and foveal microvasculature. Ophthalmic conditions: Age-Related Macular Degeneration (AMD; Dry/Wet AMD, Geographic Atrophy [GA]), Diabetic Retinopathy (DR), Diabetic Macular Edema (DME), Primary Open-Angle Glaucoma (POAG), Retinal Vein Occlusion (RVO). Systemic conditions ("Oculomics"): Incident myocardial infarction, stroke, heart failure, Parkinson's disease, and Alzheimer's disease.
- **Release / Publication Year:** 2023 (RETFound / *Nature*), 2026 (OCTCube-M / *Nat Biomed Eng* [S45], RETFound Plus / *npj Dig Med* [S61], Volumetric V-JEPA Benchmark / *npj Dig Med* [S46])
- **Evidence Code:** `E1` (Peer-Reviewed Versions-of-Record in *Nature*, *Nat Biomed Eng*, and *npj Digital Medicine*) + `E2` (Official Model & Dataset Portals)
- **Access Level:** Controlled / Governance-Gated (UK Biobank requires approved MTA/Application; Moorfields Eye Hospital NHS Trust Research Governance; public evaluation subsets available via Zenodo/Mendeley)
- **Primary Source / Portal:** [https://www.ukbiobank.ac.uk/](https://www.ukbiobank.ac.uk/) | RETFound: [https://github.com/rmaphoh/RETFound_MAE](https://github.com/rmaphoh/RETFound_MAE) | OCTCube: [https://github.com/OCTCube/OCTCube](https://github.com/OCTCube/OCTCube)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** The retina is the only accessible visualization of human neurosensory tissue and microcirculation in vivo. Conventional ophthalmic AI historically flattened 3D OCT volumes into single 2D B-scans or color fundus photographs, discarding crucial 3D volumetric morphology needed to assess geographic atrophy progression and retinal layer integrity. Furthermore, longitudinal temporal changes (disease progression vs. stable chronicity) were conflated into static cross-sectional snapshots.
- **Task Scope:** 3D retinal disease classification, geographic atrophy growth rate regression, retinal layer segmentation, and systemic oculomic incident risk stratification.

### [A] Acquisition Physics & Scale
- **Exact Counting Unit Discipline:**
  $$\text{patient} \neq \text{eye (OD / OS)} \neq \text{encounter / visit} \neq \text{3D OCT volume / CFP} \neq \text{2D B-scan slice}$$
- **Verified Scale Across Anchor Cohorts:**
  - **OCTCube-M Pretraining Cohorts `[S45]`:**
    - **OCTCube:** **26,605 3D OCT volumes** (~1,620,000 2D B-scans) for 3D self-supervised masked autoencoding with FlashAttention.
    - **OCTCube-IR:** Bimodal pairing of 3D OCT with 2D confocal Scanning Laser Ophthalmoscopy (cSLO) Infrared reflectance.
    - **OCTCube-EF:** **>4,000,000 2D OCT slices and ~400,000 en-face images** dedicated to modeling geographic atrophy progression across clinical trials.
  - **RETFound & RETFound Plus Cohorts `[S61]`:**
    - **RETFound (2023):** **1,600,000 retinal images** (900k color fundus photos, 700k OCT B-scans) from UK Biobank (~37k participants) and Moorfields Eye Hospital (~38k participants).
    - **RETFound Plus (2026):** **1,304,292 longitudinal fundus photographs** from **304,345 participants** with time-aware and identity-aware pretraining for incident outcome forecasting.
  - **Public Benchmark Suites:** OLIVES (9,410 OCT/CFP pairs, 1,268 patients), Kermany OCT2017 (84,484 B-scans, 4,686 patients), Duke SD-OCT (38,400 B-scans across 115 AMD patients).
- **Acquisition Physics:** Low-coherence near-infrared interferometry ($\lambda \approx 840\text{ nm}$ or $1050\text{ nm}$ swept-source). High axial optical resolution ($3\text{--}7\,\mu\text{m}$) with volumetric scan matrices spanning $512 \times 496 \times 64$ to $1024 \times 512 \times 128$ voxels.

### [T] Truth & Annotation Provenance
- **Human Expert Consensus:** Moorfields Reading Centre certified double-gradings and consensus adjudication by fellowship-trained vitreoretinal specialists and medical retina ophthalmologists.
- **Linked EHR & Registry Outcomes:** UK Biobank linked National Health Service (NHS) Hospital Episode Statistics (HES), primary care records, and mortality registers with verified ICD-10 diagnostic codes for systemic endpoints.

### [A] Access, Terms & Artifacts
- **Repository / Download:** UK Biobank Access Management System (Application approval required); open-source model checkpoints on GitHub and Hugging Face.
- **License / Terms:** UK Biobank Material Transfer Agreement; model weights released under CC BY-NC 4.0 / Apache-2.0.
- **Artifact Availability:** Pretrained ViT-Large checkpoints, FlashAttention 3D OCT feature extractors, and evaluation pipelines.

### [S] Systemic Lineage & Genealogy
- **Genealogy Progression:** 2D Slice ResNets (Kermany 2018) $\rightarrow$ RETFound (Nature 2023, 2D MAE on UKB/Moorfields) $\rightarrow$ Volumetric V-JEPA (npj Dig Med 2026, video-FM on OCT volumes) $\rightarrow$ OCTCube-M (Nat Biomed Eng 2026, 3D multimodal foundation model) & RETFound Plus (npj Dig Med 2026, longitudinal temporal FM).
- **Contamination & Overlap Warning:**
  1. *UK Biobank Omnipresence:* UK Biobank retinal data is ubiquitous across ophthalmic AI. Evaluating external generalization on UK Biobank subsets when pretraining weights were exposed to UK Biobank represents circular validation.
  2. *Bilateral Eye Leakage:* The right eye (OD) and left eye (OS) of the same patient share genetic architecture, systemic vasculature, and biological age. Models splitting OD and OS across train and test sets experience severe artificial performance inflation.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings across official ophthalmic benchmark evaluations (Multi-Disease Ocular AUROC and Geographic Atrophy growth prediction, Liu et al. 2026 [S45], Judkiewicz et al. 2026 [S46], Wang et al. 2026 [S61]).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Multi-Disease Mean AUROC | Volumetric OCT AUROC | Longitudinal Incident AUROC | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **OCTCube-M** | Liu et al. (WUSTL / Stanford) | 3D Multimodal Vision Transformer with FlashAttention and Cross-Modal Contrastive Alignment (COEP) | Multi-Cohort Test Split ($N=5\text{ cohorts}$) | **0.962** | **0.958** | — | [Nat Biomed Eng 2026](https://doi.org/10.1038/s41551-026-01662-2) |
| **2** | **Volumetric V-JEPA** | Judkiewicz et al. (Technion) | Transformer-based video foundation model treating 3D OCT volumes as temporal video sequences | Held-out 5-Dataset Benchmark ($N=5\text{ datasets}$) | — | **0.940** (95% CI: 0.80–0.99) | — | [npj Dig Med 2026](https://doi.org/10.1038/s41746-026-02496-7) |
| **3** | **RETFound Plus** | Wang et al. (Tsinghua / UCL) | Time- and person-sensitive longitudinal foundation model with temporal masking | Held-out Multi-Ethnic Cohorts ($N=304\text{k participants}$) | — | — | **0.914** (Ocular) / **0.825** (Systemic) | [npj Dig Med 2026](https://doi.org/10.1038/s41746-026-02524-6) |
| **4** | **Standard RETFound (2D)** | Zhou et al. (UCL / Moorfields) | Masked Autoencoder (MAE) ViT-Large trained on 1.6M 2D retinal slices and fundus photos | Multi-Cohort Test Split (Moorfields / UKB) | **0.908** | **0.900** (Slice-based) | **0.785** | [Nature 2023](https://doi.org/10.1038/s41586-023-06555-x) |
| **5** | **DINOv2 Baseline** | Judkiewicz et al. comparator | ViT-L/14 generalist vision foundation model evaluated via linear probing on central B-scans | Held-out 5-Dataset Benchmark ($N=5\text{ cohorts}$) | **0.865** | **0.862** | — | [npj Dig Med 2026](https://doi.org/10.1038/s41746-026-02496-7) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full volumetric pretraining requires enterprise hardware (8x H100/A100 GPUs) due to 3D volumetric token counts ($128 \times 512 \times 512$).
  - Inference with pre-extracted features or FlashAttention-2 enabled models runs comfortably on **16–24 GB VRAM** (RTX 4080 / RTX 3090 / RTX 4090).
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install numpy pillow
import numpy as np
from PIL import Image

# Simulate loading a 3D OCT volume (e.g. 64 B-scans of 512x512)
# In production: OCT volumes are stored as multi-frame TIFFs, NIfTI (.nii.gz), or DICOM (.dcm)
dummy_oct_volume = np.random.randint(0, 255, size=(64, 512, 512), dtype=np.uint8)
print(f"OCT Volume shape (B-scans, Height, Width): {dummy_oct_volume.shape}")

# Inspect central foveal B-scan slice
central_bscan = dummy_oct_volume[dummy_oct_volume.shape[0] // 2]
print(f"Central B-scan slice shape: {central_bscan.shape}, Dynamic range: [{central_bscan.min()}, {central_bscan.max()}]")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Bilateral Contamination Trap:* If patient ID is not tracked at splitting, right-eye images in the test set will be contaminated by left-eye images in the training set.
  2. *Media Opacity & Signal Attenuation:* Cataracts, vitreous floaters, and corneal scars attenuate the incident laser beam, producing low-signal dark bands across B-scans that confound layer segmentation.

---

## 5. Downstream Foundation Model Consumers
The following models build directly on these retinal cohorts:
- **OCTCube-M** (`[S45]`): Multimodal 3D OCT foundation model for disease classification and geographic atrophy prediction.
- **RETFound Plus** (`[S61]`): Longitudinal temporal foundation model for systemic risk prediction.
- **RETFound** (Nature 2023): Landmark 2D retinal foundation model.
- **VisionFM**: Generalist ophthalmic foundation encoder.
