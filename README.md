# Medical Imaging AI — Ground-Layer Frontier Landscape

This repository maintains the evidence-audited **Ground Layer for Medical Image Computing and Medical AI**.

## Core Reference Document

* 📄 **[Technical Dossier: Medical Imaging AI — Ground-Layer Frontier Landscape (v4.1.0)](./docs/dossier_medical_imaging_AI_ground_layer_v4.1.0.md)**
  * **Document ID**: `DOSSIER-MED-02-IMAGING`
  * **Snapshot Date**: 11 September 2026
  * **Lifecycle**: Living, evidence-audited reference

## Scope and Structure

The ground layer provides a bias-resistant, evidence-audited map of medical-imaging AI across three orthogonal axes:
1. **Imaging Problem Stack**: Acquisition & reconstruction $\rightarrow$ Quality control $\rightarrow$ Registration $\rightarrow$ Segmentation/Detection $\rightarrow$ Representation $\rightarrow$ Reasoning & Reporting $\rightarrow$ Clinical translation.
2. **Learning & Data Paradigms**: Task-specific supervised baselines, self-supervised representations (SSL), promptable segmentation, vision-language foundation models, label-efficient learning, test-time adaptation, and generative/synthetic modeling.
3. **Clinical Tasks & Use-Roles**: Triage, detection, diagnostic assistance, quantitative imaging biomarkers, opportunistic screening, surgical guidance, and auditable reporting.

## Key Coverage Areas
- **3D CT/MRI/fMRI**: Merlin, CT-RATE/CT-CLIP, Decipher-MR, Prima, NeuroVFM, SAT3D, NeuroSTORM, MedGemma 1.5.
- **Chest Radiography**: RAD-DINO, Ark/Ark+, CLEAR, MAIRA-2, CARE-X.
- **Segmentation**: nnU-Net v2, VISTA3D, MedSAM2, BiomedParse v2, Medical SAM3.
- **Specialty Modalities**: Ultrasound & Echocardiography (EchoPrime/EchoCLIP), Ophthalmology (OCTCube-M, RETFound Plus), Dermatology (PanDerm), Dental (PanoFM, DentVLM), PET/CT.
- **Pathology & Spatial Biology**: Prov-GigaPath, Virchow2, UNI2-h, H-Optimus-1, TITAN, PRISM2, CRISP, VirTues, GigaTIME.
- **Benchmarking & Translation Doctrine**: Multi-dimensional evidence profiles, dataset genealogy/contamination audits, domain-shift stress tests, and lifecycle validation frameworks.

## Modular Dataset Documentation Library (`docs/01_datasets/`)

Structured according to the **D-A-T-A-S + Leaderboard** schema, extracting clinical domain, acquisition physics, annotation truth provenance, access terms, and verified Top-5 SOTA leaderboards:

1. 📂 **[01: 3D Segmentation & Anatomical Benchmarks](./docs/01_datasets/01_segmentation_3d/README.md)**:
   * [KiTS23](./docs/01_datasets/01_segmentation_3d/kits23.md) — Kidney and renal tumor segmentation.
   * [AMOS22](./docs/01_datasets/01_segmentation_3d/amos22.md) — 15 abdominal multi-organ CT & MRI benchmark.
   * [TotalSegmentator](./docs/01_datasets/01_segmentation_3d/totalsegmentator.md) — 104/117 whole-body anatomical structures.
   * [CVPR-BiomedSegFM](./docs/01_datasets/01_segmentation_3d/cvpr_biomedsegfm.md) — 200k+ multi-modal text-prompted 3D foundation model benchmark.
   * [FLARE PanCancerCTSeg](./docs/01_datasets/01_segmentation_3d/flare_pancancer_ctseg.md) — Pan-cancer lesion & organ segmentation under resource constraints.
   * [AbdomenAtlas 1.0/2.0](./docs/01_datasets/01_segmentation_3d/abdomen_atlas.md) — 20k multi-center CT volumes from 112 hospitals.
   * [AbdomenAtlas 3.0](./docs/01_datasets/01_segmentation_3d/abdomen_atlas_3.md) — 3D CT image–mask–report triplets via RadGPT.
   * [Medical Segmentation Decathlon (MSD)](./docs/01_datasets/01_segmentation_3d/msd_decathlon.md) — 10 diverse challenge tasks (nnU-Net origin).
   * [LUNA16](./docs/01_datasets/01_segmentation_3d/luna16.md) — Thoracic CT lung nodule detection and FROC benchmark.
   * [autoPET / autoPET II](./docs/01_datasets/01_segmentation_3d/autopet.md) — Whole-body $^{18}\text{F-FDG}$ PET/CT metabolic lesion segmentation.
2. 📂 **[02: Volumetric CT & MRI Radiology Datasets](./docs/01_datasets/02_radiology_ct_mri/README.md)**:
   * [CT-RATE](./docs/01_datasets/02_radiology_ct_mri/ct_rate.md) — 3D chest CT paired with clinical reports (21k patients, 25k studies, 50k reconstructed volumes).
   * [RadGenome-ChestCT](./docs/01_datasets/02_radiology_ct_mri/radgenome_chestct.md) — Grounded 3D chest CT with 197 mask categories, 665k grounded reports, and 1.2M/1.3M VQA pairs.
   * [PatchChestCT](./docs/01_datasets/02_radiology_ct_mri/patch_chestct.md) — 3D patch-level spatial annotations across 2,201 reviewed CT studies for 9 abnormalities.
   * [MR-RATE](./docs/01_datasets/02_radiology_ct_mri/mr_rate.md) — Health-system-scale multimodal brain and spine MRI (83k patients, 98k studies, 705k series/volumes).
   * [Merlin Abdominal CT](./docs/01_datasets/02_radiology_ct_mri/merlin_abdominal_ct.md) — Multi-task abdominal & pelvic CT dataset with paired reports (25,494 scans from 18,317 patients).
   * [RAD-ChestCT](./docs/01_datasets/02_radiology_ct_mri/rad_chestct.md) — Duke University chest CT cohort for out-of-distribution external validation (3,630 open scans).
3. 📂 **[03: Chest Radiography (CXR) Benchmark & Representation Datasets](./docs/01_datasets/03_chest_xray/README.md)**:
   * [MIMIC-CXR v2.1.0](./docs/01_datasets/03_chest_xray/mimic_cxr.md) — `[D12]` 377,110 DICOM images across 227,835 studies with clinical reports.
   * [CheXpert](./docs/01_datasets/03_chest_xray/chexpert.md) — `[D13]` 224,316 radiographs from 65,240 patients with 14-observation uncertainty labels.
   * [PadChest](./docs/01_datasets/03_chest_xray/padchest.md) — `[D14]` 160,868 radiographs across 109,931 studies with 174 radiographic findings.
   * [NIH ChestX-ray14](./docs/01_datasets/03_chest_xray/nih_chestxray14.md) — `[D15]` 112,120 frontal CXRs from 30,805 patients with report-mined labels.
   * [VinDr-CXR](./docs/01_datasets/03_chest_xray/vindr_cxr.md) — `[D16]` 18,000 CXRs with radiologist-annotated bounding boxes and global labels.
   * [MS-CXR](./docs/01_datasets/03_chest_xray/ms_cxr.md) — `[D17]` 1,162 phrase-bounding-box pairs across 1,026 CXR images (MIMIC-CXR subset).
   * [Chest ImaGenome](./docs/01_datasets/03_chest_xray/chest_imagenome.md) — `[D18]` Anatomical scene graphs across 242k CXRs with 500-patient gold subset.
   * [BRAX](./docs/01_datasets/03_chest_xray/brax.md) — `[D19A]` 40,967 radiographs across 24,959 studies with Portuguese NLP labels.
4. 📂 **[04: Pathology & Spatial Biology Datasets](./docs/01_datasets/04_pathology_spatial/README.md)**:
   * [TCGA](./docs/01_datasets/04_pathology_spatial/tcga.md) — `[D25]` Pan-cancer WSIs across 33 cancer types (in-distribution baseline doctrine).
   * [CPTAC](./docs/01_datasets/04_pathology_spatial/cptac.md) — `[D26]` Multi-tier proteogenomic cancer pathology & imaging ecosystem across GDC/PDC/TCIA.
   * [CAMELYON16/17](./docs/01_datasets/04_pathology_spatial/camelyon17.md) — `[D27]` 1,000 lymph-node WSIs across 5 Dutch medical centres for metastasis detection & pN-staging (CC0).
   * [PANDA](./docs/01_datasets/04_pathology_spatial/panda.md) — `[D28]` 10,600+ prostate biopsy WSIs from Radboud UMC and Karolinska Institute for ISUP grading.
   * [GigaTIME Benchmark](./docs/01_datasets/04_pathology_spatial/gigatime_benchmark.md) — `[D29]` Paired H&E to 21-channel virtual mIF benchmark (50 public test patches vs proprietary pretraining).
   * [PathoROB Suite](./docs/01_datasets/04_pathology_spatial/pathorob_suite.md) — `[S29/S28]` External multi-center patch suite evaluating biological performance vs technical confound invariance across 34 centres.
5. 📂 **[05: Specialty Imaging Modalities & Longitudinal Frontier Datasets](./docs/01_datasets/05_specialty/README.md)**:
   * [fastMRI](./docs/01_datasets/05_specialty/fastmri.md) — Raw measurement-domain k-space and accelerated clinical MRI reconstruction (>1,500 raw knee scans, 10k DICOMs, 6.9k raw brain scans).
   * [EchoNet-Dynamic](./docs/01_datasets/05_specialty/echonet_dynamic.md) — Beat-to-beat cardiac function and LV segmentation on 10,030 echocardiography videos.
   * [OCTCube & RETFound Cohorts](./docs/01_datasets/05_specialty/octcube_retfound_cohorts.md) — 3D retinal OCT volumes, multimodal cSLO/en-face imaging, and longitudinal fundus cohorts (OCTCube-M, RETFound Plus).
   * [PanDerm Corpus](./docs/01_datasets/05_specialty/panderm_corpus.md) — Multimodal clinical dermatology corpus (2.15M images across TBP tiles, dermoscopy, clinical photos, and dermatopathology).
   * [GastroNet-5M](./docs/01_datasets/05_specialty/gastro_net_5m.md) — Multicenter self-supervised endoscopy pretraining corpus (4.82M frames from ~500k procedures).
   * [Dental Panoramic Cohorts](./docs/01_datasets/05_specialty/dental_panoramic_cohorts.md) — Panoramic radiography and multimodal oral imaging cohorts (PanoFM 127k scans, DentFound >101k patients, DentVLM).

## Modular Model Documentation Library (`docs/02_models/`)

Structured according to the **P-A-B-H-A** snapshot framework ([P] Clinical Problem, [A] Architectural Core, [B] Benchmark & Delta, [H] Hardware Footprint, [A] Access & Artifacts) with tier-calibrated availability and supervision stratification:

1. 📂 **[01: 3D Segmentation & Spatial Parsing](./docs/02_models/01_segmentation/README.md)**:
   * [nnU-Net v2](./docs/02_models/01_segmentation/nnunet_v2.md) — `[M04]` The self-configuring supervised benchmark standard and reference comparator.
   * [VISTA3D / NV-Segment-CT](./docs/02_models/01_segmentation/vista3d.md) — `[M05]` 132-class automated whole-body parsing and interactive click segmentation.
   * [SAT3D](./docs/02_models/01_segmentation/sat3d.md) — `[M06]` Uncertainty-aware whole-body pan-cancer tumour segmentation with critic feedback.
   * [MedSAM2](./docs/02_models/01_segmentation/medsam2.md) — `[M07]` Promptable 3D-as-video volumetric mask propagation via streaming memory bank.
   * [BiomedParse v2](./docs/02_models/01_segmentation/biomedparse_v2.md) — `[M08]` BoltzFormer text-guided multi-modal parser with built-in existence gating.
   * [Medical SAM3](./docs/02_models/01_segmentation/medical_sam3.md) — `[M09]` Universal text- and prompt-driven segmentation adapted across 10 modalities.
