# 05: Specialty Imaging Modalities & Longitudinal Frontier Datasets

Welcome to the **Specialty Modalities & Longitudinal Frontier Dataset Library**. This module catalogs evidence-audited benchmark datasets and pretraining corpora across six high-impact clinical specialties beyond conventional volumetric CT/MRI and CXR: **Inverse MRI Reconstruction**, **Echocardiography Video**, **Ophthalmic 3D OCT & Longitudinal Fundus**, **Multimodal Dermatology**, **Procedural Endoscopy**, and **Dental Panoramic Radiography**.

Every card in this directory strictly adheres to the repository's canonical **D-A-T-A-S + Leaderboard** schema, extracting the clinical bottleneck, acquisition physics, annotation provenance, license terms, and verified Top-5 SOTA leaderboards.

---

## 1. Master Comparative Benchmark Matrix

| Canonical ID | Dataset / Benchmark | Primary Modality | Target Clinical & Pathological Scope | Verified Scale & Counting Unit | SOTA Winner / Landmark Architecture | Evaluation Setting | Evidence Code | Quick Link |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **`[D24]`** | **fastMRI** | Raw $k$-Space & Clinical MRI | Accelerated MRI Reconstruction ($4\times, 8\times$) | 1,598 raw knee scans + 10k DICOMs; 6,970 raw brain scans | AIRS Medical Unrolled Net — SSIM **0.942** | Official Blind Test ($8\times$ Brain, $N=565$) | `E1+E2` | [Card](./fastmri.md) |
| **`[D-ECHO]`** | **EchoNet-Dynamic** | 2D B-mode TTE Video (A4C) | Beat-to-Beat Cardiac Function (LVEF, EDV, ESV) | 10,030 videos from 10,030 unique patients (~1.8M frames) | EFNet (Li et al.) — LVEF MAE **3.70%**, $R^2 = 0.82$ | Official Held-out Test Split ($N=1,277$) | `E1+E2` | [Card](./echonet_dynamic.md) |
| **`[D-RETINA]`** | **OCTCube & RETFound Cohorts** | 3D SD/SS-OCT & Color Fundus | Retinal Layer Biomarkers, GA Growth, Oculomics | 26.6k 3D OCT volumes (OCTCube); 1.3M longitudinal photos (RETFound+) | OCTCube-M (Liu et al.) — Mean AUROC **0.962** | Multi-Cohort Test Split ($N=5\text{ cohorts}$) | `E1+E2` | [Card](./octcube_retfound_cohorts.md) |
| **`[D-DERM]`** | **PanDerm Corpus** | Multimodal Derm (TBP, Dermpath, Clinical, Dermoscopy) | Cutaneous Neoplasms (>128 diseases) & Longitudinal Lesions | 2,149,706 images across 11 institutional sources | PanDerm-L (Yan et al.) — Macro AUROC **0.938** | Held-out 28-Benchmark Suite ($N=28$) | `E1+E2` | [Card](./panderm_corpus.md) |
| **`[D-ENDO]`** | **GastroNet-5M** | HD WLE & NBI Video Frames | Colorectal Polyps, Barrett's Neoplasia, Early Gastric Cancer | 4,820,653 frames from ~500,000 procedures across 8 hospitals | GastroNet-5M ViT-B (ThetaVision) — Mean Dice **0.918** | Downstream Multi-Center Benchmark | `E1+E2` | [Card](./gastro_net_5m.md) |
| **`[D-DENT]`** | **Dental Panoramic Cohorts** | Panoramic OPG, CBCT & Multimodal Oral | Universal Tooth Enumeration, Caries, Periodontitis | 127k OPGs (PanoFM); >101k patients (DentFound); 110k images (DentVLM) | Radboud_ISMI (van Nistelrooij) — Multi-Task F1 **0.784** | Official DENTEX Blind Test ($N=240$) | `E1+E2` | [Card](./dental_panoramic_cohorts.md) |

---

## 2. Specialty Acquisition Physics & Counting Unit Disciplines

Medical AI in specialty domains is uniquely vulnerable to unit-of-analysis confusion. In this module, the following strict counting hierarchies must be enforced:

### 1. MRI Inverse Imaging
$$\text{patient} \neq \text{examination} \neq \text{pulse sequence} \neq \text{reconstructed volume} \neq \text{slice} \neq \text{receiver coil} \neq k\text{-space measurement line}$$
*Crucial Distinction:* fastMRI provides raw frequency-domain data ($k$-space). Reporting numbers on post-reconstruction JPEG/DICOM images is image restoration/enhancement, not genuine inverse-problem accelerated reconstruction.

### 2. Echocardiography Video
$$\text{patient} \neq \text{study / encounter} \neq \text{video acquisition} \neq \text{cardiac cycle / beat} \neq \text{acoustic view (A4C/A2C/PLAX)} \neq \text{video frame}$$
*Crucial Distinction:* EchoNet-Dynamic is an apical-4-chamber (A4C) single-view benchmark. Models trained across full studies (e.g. EchoPrime) must classify and pool dozens of videos across standard standardized views.

### 3. Ophthalmology & Retinal Imaging
$$\text{patient} \neq \text{eye (OD / OS)} \neq \text{visit / longitudinal encounter} \neq \text{3D OCT volume / CFP} \neq \text{2D B-scan slice}$$
*Crucial Distinction:* 3D OCT scans are coherent volumetric interference measurements ($512 \times 496 \times 64$). Flattening volumes into single central B-scans throws away out-of-plane pathology (e.g., peripheral geographic atrophy margins).

### 4. Multimodal Dermatology
$$\text{patient} \neq \text{clinical visit} \neq \text{anatomical lesion site} \neq \text{imaging modality} \neq \text{image / TBP tile}$$
*Crucial Distinction:* A single patient may have hundreds of total body photography (TBP) tiles, alongside paired dermoscopic and histopathological slices of the same excised lesion.

### 5. Procedural Video Endoscopy
$$\text{patient} \neq \text{endoscopic procedure / encounter} \neq \text{video stream} \neq \text{frame / still crop}$$
*Crucial Distinction:* GastroNet-5M's "4.8M images" are extracted video frames across ~500k procedures. Frame-level splitting produces massive temporal data leakage.

### 6. Dental & Maxillofacial Imaging
$$\text{patient} \neq \text{encounter} \neq \text{jaw quadrant} \neq \text{tooth (FDI 11–48)} \neq \text{imaging view} \neq \text{radiograph}$$
*Crucial Distinction:* Panoramic radiographs project a 3D dental arch into a 2D focal trough. Tooth instances are nested within patients; splitting at the tooth level violates patient independence.

---

## 3. Cross-Cutting Specialty Leakage & Pretraining Contamination Hazards

Before publishing or claiming state-of-the-art results on specialty benchmarks, audit these four pervasive leakage pathways:

``` text
┌─────────────────────────────────────────────────────────────────────────────┐
│                       SPECIALTY LEAKAGE RISK TAXONOMY                       │
├─────────────────────────────────────────────────────────────────────────────┤
│ 1. Bilateral Organ Correlation    │ Left vs. right eye (retina), breast,    │
│    (Genetic & Systemic Leakage)   │ or bilateral dentition split into train │
│                                   │ and test sets leaks systemic signals.   │
├───────────────────────────────────┼─────────────────────────────────────────┤
│ 2. Dense Temporal Frame Leakage   │ Consecutive video frames (echo cines,   │
│    (Near-Duplicate Exploitation)  │ endoscopy) share 99% background; splits │
│                                   │ must strictly occur at patient level.   │
├───────────────────────────────────┼─────────────────────────────────────────┤
│ 3. Shared Institutional Lineages  │ Cedars-Sinai archives link EchoCLIP and │
│    (Hidden Pretraining Overlap)   │ EchoPrime; UK Biobank links RETFound,   │
│                                   │ RETFound+, and dozens of eye models.    │
├───────────────────────────────────┼─────────────────────────────────────────┤
│ 4. Multimodal Cross-Leakage       │ Pretraining on macroscopic TBP tiles    │
│    (Lesion Identity Retention)    │ while evaluating on dermoscopy of the   │
│                                   │ identical physical lesion site.         │
└─────────────────────────────────────────────────────────────────────────────┘
```

1. **Bilateral Eye Correlation (Ophthalmology):** Because human left (OS) and right (OD) eyes share genetic code, blood pressure, systemic glycemic exposure, and chronological age, assigning one eye to training and the other to testing leaks the reference target for systemic risk prediction.
2. **Dense Video Frame Temporal Correlation (Endoscopy & Ultrasound):** Video frames sampled milliseconds apart within the same withdrawal or cardiac cycle differ only by subtle probe jitter. Random frame-level cross-validation yields near-perfect accuracy (Dice >0.98) that collapses completely when deployed on a new patient.
3. **Shared Institutional Pretraining Lineage (Echocardiography & Ophthalmology):**
   - *Echocardiography:* EchoCLIP (`[S84]`) and EchoPrime (`[S83]`) both draw from Cedars-Sinai Medical Center PACS archives; they are derivative model architectures on a common institutional lineage, not independent evidence cohorts.
   - *Ophthalmology:* UK Biobank is ubiquitous across retinal foundation models (RETFound, RETFound Plus). Testing on UK Biobank holdouts does not constitute external geographic validation.
4. **Modality Cross-Leakage (Dermatology):** In corpora like PanDerm (`[S88]`), an individual lesion may have both a macro clinical photo and a dermoscopic micro-photograph. Evaluating cross-modal transfer requires patient-level and lesion-level splitting.

---

## 4. Epistemic Lessons from the Ground Layer

1. **Physics-Aware Data Consistency vs. Perceptual Hallucination:** In accelerated MRI reconstruction (fastMRI), models evaluated purely on perceptual or image-domain loss functions (e.g., L1, perceptual loss) generate photorealistic trabecular bone and vascular anatomy that are complete hallucinations. Ground-truth measurement-domain data consistency (DC) layers in $k$-space are non-negotiable.
2. **The Single-View vs. Multi-View Dilemma:** Early echocardiography AI was restricted to the apical 4-chamber view (EchoNet-Dynamic). However, clinical cardiologists never diagnose systolic dysfunction from a single view; regional wall-motion abnormalities (e.g., left anterior descending vs. right coronary artery infarctions) require multi-view triangulation (A4C, A2C, PLAX, PSAX).
3. **Native 3D/Volumetric Representations:** In ophthalmic OCT, transitioning from 2D B-scans to native 3D volumes (OCTCube-M, Volumetric V-JEPA) yields statistically significant diagnostic improvements (AUROC $0.940$ vs. $0.900$, $p < 0.001$), proving that 3D spatial context is essential for sub-surface disease characterization.

---

## 5. Recommended Study Sequence (The 5–15–60 Funnel)

1. **Tier 1 (5 Minutes):** Scan the Master Comparative Benchmark Matrix above to identify your specialty domain, acquisition modality, and official blind test metric.
2. **Tier 2 (15 Minutes):** Review the corresponding dataset card to inspect the **D-A-T-A-S** snapshot, counting-unit discipline, and verified Top-5 SOTA leaderboards.
3. **Tier 3 (60 Minutes):** Run the minimal verification Python snippet in Section 4 of each card to verify data loaders, array structures, and image dimensions on your local workstation.
