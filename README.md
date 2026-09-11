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

