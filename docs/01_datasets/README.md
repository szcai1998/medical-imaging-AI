# Master Medical Imaging Datasets & Benchmark Cartography (`docs/01_datasets/`)

> Comprehensive, evidence-audited catalog of all **36 benchmark datasets and pretraining corpora** supporting the [Medical Imaging AI Ground Layer](../dossier_medical_imaging_AI_ground_layer_v4.1.0.md).

---

## 1. Executive Summary & Epistemic Principles

High-performance medical machine learning depends fundamentally on dataset integrity, annotation provenance, and leakage-free evaluation splits. This directory contains detailed dossier cards for all major open benchmarks and foundation model pretraining corpora, structured under the standardized **D-A-T-A-S + Leaderboard** schema.

### Core Evaluative Rules (from `AGENTS.md`):
1. **Counting-Unit Discipline**: Never conflate distinct clinical units:
   $$\text{Patient} \neq \text{Examination/Study} \neq \text{Series} \neq \text{Reconstructed 3D Volume} \neq \text{2D Slice} \neq \text{Voxel Mask}$$
2. **Canonical Evidence Tags**:
   - `[E1]`: Peer-reviewed version-of-record publication (e.g., *Nature*, *Lancet Digital Health*, *Radiology*, *Medical Image Analysis*).
   - `[E2]`: Official competitive challenge proceedings / registered model card (e.g., MICCAI Grand Challenge, Kaggle).
   - `[E5A]`: Independent multi-center benchmark or third-party stress test.
3. **Evaluation Split Transparency**: Leaderboards rigorously separate **blind held-out challenge test sets** from **cross-validation (CV)** or self-reported internal splits.
4. **Pretraining Contamination Doctrine**: Pervasive public cohorts (e.g., TCGA, MIMIC-CXR, CheXpert, MSD, TotalSegmentator) are assumed to have been seen by foundation models during pretraining. "Zero-shot" claims against these cohorts must be audited for data contamination.

---

## 2. Directory Architecture & Module Overview

The dataset library is partitioned into five specialized clinical and technological modules:

```text
docs/01_datasets/
├── 01_segmentation_3d/        # Volumetric CT/MRI segmentation, anatomical parsing, and lesion benchmarks (10 cards)
├── 02_radiology_ct_mri/       # Large-scale 3D CT/MRI radiology corpora and report-grounded datasets (6 cards)
├── 03_chest_xray/             # 2D projection radiography, bounding-box grounding, and scene graphs (8 cards)
├── 04_pathology_spatial/      # Whole-slide imaging (WSI), multi-center patch suites, and spatial biology (6 cards)
└── 05_specialty/              # Inverse k-space, echocardiography, ophthalmology, dermatology, endoscopy, and dental (6 cards)
```

---

## 3. Master Cross-Modality Comparative Matrix

| Canonical ID | Dataset / Cohort | Modality | Target Anatomy / Clinical Domain | Scale (Exact Units) | Annotation Provenance | Access Tier | Evidence Code | Dossier Card |
|---|---|---|---|---|---|---|:---:|:---:|
| **[D9]** | **KiTS23** | 3D Contrast CT | Kidney, renal tumor, and cyst | 599 CT cases (489 train, 110 test) | Expert urologist consensus | Challenge (Open Train) | `E2` | [Card](./01_segmentation_3d/kits23.md) |
| **[D8]** | **AMOS22** | 3D CT & MRI | 15 abdominal organs | 600 multi-center scans (500 CT, 100 MRI) | Radiologist manual voxel masks | Challenge (Open Train) | `E2` | [Card](./01_segmentation_3d/amos22.md) |
| **[D6]** | **TotalSegmentator** | 3D CT & MRI | 104/117 anatomical structures | 1,204 CT volumes (v1) / 1,478 + 3,842 MRI (v2) | Human-in-the-loop active learning | Open (CC BY 4.0 / Academic) | `E1` | [Card](./01_segmentation_3d/totalsegmentator.md) |
| **[D1/D2]** | **CVPR-BiomedSegFM** | Multi-modal 3D | General promptable 3D anatomy & tumors | 200,000+ volumes, 10+ imaging modalities | Heterogeneous multi-challenge aggregate | Open Benchmark | `E2` | [Card](./01_segmentation_3d/cvpr_biomedsegfm.md) |
| **[D3]** | **FLARE PanCancerCTSeg** | 3D CT | 13 organs + pan-cancer lesions | ~4,000 multi-center CT cases | Multi-institutional semi-supervised | Challenge (Constrained) | `E2` | [Card](./01_segmentation_3d/flare_pancancer_ctseg.md) |
| **[D4]** | **AbdomenAtlas 1.0/2.0** | 3D Contrast CT | 9–25 abdominal organs & vessels | 20,000 CT volumes across 112 hospitals | Non-random iterative active learning | Open (Apache 2.0) | `E1` | [Card](./01_segmentation_3d/abdomen_atlas.md) |
| **[D5]** | **AbdomenAtlas 3.0** | 3D Contrast CT | Multi-organ masks + structured text | 3.2M CT volumes / 1.1M triplets | VLM-assisted (RadGPT) + radiologist audit | Open (CC BY-NC 4.0) | `E1` | [Card](./01_segmentation_3d/abdomen_atlas_3.md) |
| **[D7]** | **MSD (Decathlon)** | 3D CT & MRI | 10 distinct anatomical challenge tasks | 2,633 3D volumes | Multi-institutional expert consensus | Open Challenge | `E1` | [Card](./01_segmentation_3d/msd_decathlon.md) |
| **[D10]** | **LUNA16** | 3D Chest CT | Pulmonary lung nodules | 888 thoracic CT scans (LIDC-IDRI subset) | 4-radiologist consensus | Open Benchmark | `E1` | [Card](./01_segmentation_3d/luna16.md) |
| **[D11]** | **autoPET / autoPET II** | 3D PET/CT | FDG-avid metabolic lesions & tumors | 1,014 patients / 1,200+ exam studies | Manual radiologist thresholded masks | Challenge (MICCAI) | `E2` | [Card](./01_segmentation_3d/autopet.md) |
| **[D20]** | **CT-RATE** | 3D Chest CT | Thoracic abnormalities & reporting | 21,304 patients, 25,692 studies, 50,188 volumes | Paired free-text radiology reports | Open (CC BY-NC 4.0) | `E1` | [Card](./02_radiology_ct_mri/ct_rate.md) |
| **[D21]** | **RadGenome-ChestCT** | 3D Chest CT | Grounded 3D anatomy, lesions & VQA | 665,000 grounded reports, 1.2M VQA pairs | VLM-grounded segmentations on CT-RATE | Open (CC BY-NC 4.0) | `E1` | [Card](./02_radiology_ct_mri/radgenome_chestct.md) |
| **[D22]** | **PatchChestCT** | 3D Chest CT | Localized 3D thoracic pathology | 2,201 CT studies, 9 abnormal patch classes | Board-certified radiologist verified | Open (CC BY-NC-SA 4.0) | `E1` | [Card](./02_radiology_ct_mri/patch_chestct.md) |
| **[D23]** | **MR-RATE** | 3D/2D MRI | Brain & spine MRI abnormalities | 83,000 patients, 98,000 studies, 705,000 series | Paired clinical reports | Open (CC BY-NC-SA 4.0) | `E1` | [Card](./02_radiology_ct_mri/mr_rate.md) |
| **[D30]** | **Merlin Abdominal CT** | 3D CT | Abdominal & pelvic organ findings | 18,317 patients, 25,494 scans | Clinical radiology report pairings | Open (Research DUA) | `E1` | [Card](./02_radiology_ct_mri/merlin_abdominal_ct.md) |
| **[D31]** | **RAD-ChestCT** | 3D Chest CT | External thoracic generalization testing | 3,630 open CT scans (Duke University cohort) | Curated radiologist clinical labels | Open (CC BY-NC 4.0) | `E1` | [Card](./02_radiology_ct_mri/rad_chestct.md) |
| **[D12]** | **MIMIC-CXR v2.1.0** | 2D Chest X-Ray | 14 thoracic findings & full reports | 377,110 DICOM images, 227,835 studies | Semi-automated CheXpert/NegBio labels + text | Credentialed PhysioNet DUA | `E1` | [Card](./03_chest_xray/mimic_cxr.md) |
| **[D13]** | **CheXpert** | 2D Chest X-Ray | 14 radiographic observations | 224,316 radiographs, 65,240 patients | Rule-based report labeler + gold test set | Academic DUA | `E1` | [Card](./03_chest_xray/chexpert.md) |
| **[D14]** | **PadChest** | 2D Chest X-Ray | 174 radiographic findings | 160,868 images, 109,931 studies, 67k patients | 27% radiologist manual + 73% Spanish NLP | Academic DUA | `E1` | [Card](./03_chest_xray/padchest.md) |
| **[D15]** | **NIH ChestX-ray14** | 2D Chest X-Ray | 14 thoracic disease categories | 112,120 frontal CXRs, 30,805 patients | Fully automated NLP report mining | Open Public | `E1` | [Card](./03_chest_xray/nih_chestxray14.md) |
| **[D16]** | **VinDr-CXR** | 2D Chest X-Ray | 22 local findings + 6 global diagnoses | 18,000 released PA CXRs | 17 expert radiologists (consensus bboxes) | PhysioNet Open | `E1` | [Card](./03_chest_xray/vindr_cxr.md) |
| **[D17]** | **MS-CXR** | 2D Chest X-Ray | Phrase-grounded sentence bounding boxes | 1,162 grounded pairs across 1,026 CXRs | Board-certified radiologist annotations | PhysioNet DUA | `E1` | [Card](./03_chest_xray/ms_cxr.md) |
| **[D18]** | **Chest ImaGenome** | 2D Chest X-Ray | Anatomical scene graphs & relations | 242,072 scene graphs + 500 gold cases | Automated NLP pipeline + expert clinical gold | PhysioNet DUA | `E1` | [Card](./03_chest_xray/chest_imagenome.md) |
| **[D19A]** | **BRAX** | 2D Chest X-Ray | Brazilian demographic generalization | 40,967 images, 24,959 studies, 19k patients | Portuguese NLP labeler (CheXpert-adapted) | PhysioNet Open | `E1` | [Card](./03_chest_xray/brax.md) |
| **[D25]** | **TCGA** | Whole-Slide WSI | Pan-cancer histopathology (33 types) | 30,000+ WSIs, ~11,000 patients | Multi-institutional diagnostic consensus | Open (GDC Portal) | `E1` | [Card](./04_pathology_spatial/tcga.md) |
| **[D26]** | **CPTAC** | Whole-Slide WSI | Proteogenomic cancer pathology | >10,000 WSIs paired with proteogenomics | Expert pathology review | Multi-tier (GDC/PDC) | `E1` | [Card](./04_pathology_spatial/cptac.md) |
| **[D27]** | **CAMELYON16/17** | Whole-Slide WSI | Breast cancer lymph-node metastases | 1,000 WSIs across 5 Dutch medical centres | Expert pathologist pixel-level delineations | Open Challenge (CC0) | `E2` | [Card](./04_pathology_spatial/camelyon17.md) |
| **[D28]** | **PANDA** | Whole-Slide WSI | Prostate biopsy ISUP grading | 10,616 digitized prostate biopsies | Expert international consensus Gleason grading | Challenge (Open Train) | `E2` | [Card](./04_pathology_spatial/panda.md) |
| **[D29]** | **GigaTIME Benchmark**| Histopathology/mIF| H&E to 21-channel virtual multiplex IF | 50 public test patches (vs private corpus) | Co-registered physical mIF staining | Open Benchmark | `E1` | [Card](./04_pathology_spatial/gigatime_benchmark.md) |
| **[S29/S28]**| **PathoROB Suite** | Patch Histology | Biological performance vs technical bias | 8 benchmark cohorts across 34 medical centres | Expert histological classification | Multi-dataset Open | `E5A` | [Card](./04_pathology_spatial/pathorob_suite.md) |
| **[D24]** | **fastMRI** | Raw k-Space / MRI | Accelerated MR image reconstruction | 1,500+ raw k-space scans, 10k DICOMs, 6.9k brain | Acquisition sensor truth (fully-sampled coils) | Open Research DUA | `E1` | [Card](./05_specialty/fastmri.md) |
| **—** | **EchoNet-Dynamic** | Video Ultrasound | Left-ventricular ejection fraction (LVEF)| 10,030 echocardiogram video clips | Registered sonographer tracings & clinician LVEF| Open Academic DUA | `E1` | [Card](./05_specialty/echonet_dynamic.md) |
| **—** | **OCTCube & RETFound**| 3D OCT & Fundus | Retinal disease and systemic phenomics | 200k+ OCT volumes (OCTCube) / UK Biobank | Board-certified ophthalmologist adjudication | Tiered / Application | `E1` | [Card](./05_specialty/octcube_retfound_cohorts.md) |
| **—** | **PanDerm Corpus** | Multimodal Derm | Skin lesion diagnosis across 4 modalities | 2.15M multimodal dermatology images | Histopathological biopsy & clinical expert gold | Multi-source Tiered | `E1` | [Card](./05_specialty/panderm_corpus.md) |
| **—** | **GastroNet-5M** | Video Endoscopy | Multicenter endoscopic video understanding| 4.82M frames from ~500,000 procedures | Procedure video metadata & clinical reports | Open Research | `E1` | [Card](./05_specialty/gastro_net_5m.md) |
| **—** | **Dental Panoramic** | Panoramic X-Ray | Odontogenic lesions, caries & teeth | 127,000 panoramic scans, >101,000 patients | Dual board-certified dentist consensus | Open / Institutional | `E1` | [Card](./05_specialty/dental_panoramic_cohorts.md) |

---

## 4. Key Takeaways & Empirical Lessons

1. **The Self-Configuring Baseline Theorem**: Across fixed, closed benchmarks (e.g., KiTS23, AMOS22, TotalSegmentator, autoPET, MSD), well-tuned task-specific baselines (**nnU-Net v2**) consistently match or outperform massive zero-shot foundation models unless the foundation models are fine-tuned on the specific target domain.
2. **Lineage Traps**: Evaluating derivative datasets (e.g., assessing an algorithm on RadGenome-ChestCT or Chest ImaGenome after training on CT-RATE or MIMIC-CXR) measures internal feature transfer rather than real-world generalizability, as the underlying patient demographics and scanner acoustics are identical.
3. **Weak vs. Strong Supervision**: NLP report-mined datasets (NIH ChestX-ray14, PadChest NLP subset) exhibit label noise rates between 10% and 30%. High AUROC scores on noisy labels can be deceptive; validation against curated human consensus test sets (VinDr-CXR, CheXpert 500) is mandatory.

---

## 5. Next Steps: Foundation Model Cartography (`docs/02_models/`)

With the dataset landscape mapped, the next phase distills the foundation models and task-specific architectures that consume these benchmarks. Model documentation follows the **P-A-B-H-A** snapshot format:
- **[P] Clinical Problem**: Clinical bottleneck or task scope.
- **[A] Architectural Core**: Tokenization, attention mechanism, loss formulation, or backbone.
- **[B] Benchmark & Delta**: Head-to-head empirical superiority against established baselines (nnU-Net, ViT, ResNet).
- **[H] Hardware Footprint**: Training profile, inference VRAM requirements (FP16/quantized), and local workstation feasibility.
- **[A] Access & Artifacts**: Open-weights checkpoints, GitHub source repository, license terms, and canonical papers.
