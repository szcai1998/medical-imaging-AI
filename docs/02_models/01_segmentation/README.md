# Module 01: 3D Segmentation, Spatial Parsing & Promptable Foundation Models

> Evidence-audited cartography of foundational segmentation architectures, interactive spatial parsers, and task-specific reference baselines supporting the [Medical Imaging AI Ground Layer](../../dossier_medical_imaging_AI_ground_layer_v4.1.0.md).

---

## 1. Executive Summary & The Core Landscape Tension

In medical image computing, segmentation has evolved from isolated, single-task U-Nets into two competing technological paradigms:
1. **The Self-Configuring Task-Specific Baseline Standard ([nnU-Net v2](./nnunet_v2.md))**: Fully supervised models that automatically configure network topology, patch sizes, and augmentations to match dataset physics. On fixed, closed benchmarks, this standard remains the gold-standard accuracy baseline.
2. **The Promptable & Multi-Modal Foundation Model Frontier**: Generalist models ([VISTA3D](./vista3d.md), [MedSAM2](./medsam2.md), [BiomedParse v2](./biomedparse_v2.md), [SAT3D](./sat3d.md), [Medical SAM3](./medical_sam3.md)) that provide interactive zero-shot delineation, 3D memory propagation, natural language prompt parsing, and uncertainty feedback.

### The Fundamental Field-State Trade-Off
As formalized in Section 4.2 of the Ground Layer Dossier:
> **The scientifically meaningful comparison is NOT "Foundation Model vs. U-Net."**  
> The core clinical question is:  
> *How much accuracy, calibration, annotation effort, robustness, and adaptation cost are exchanged when moving from a task-specific supervised system to a promptable generalist system?*

```mermaid
graph TD
    subgraph "The Segmentation Paradigm Spectrum"
        A["nnU-Net v2 [M04]<br/>(Self-Configuring Supervised Baseline)"] -->|Requires Full Labels<br/>Highest Closed-Benchmark Dice| B["VISTA3D [M05]<br/>(Automated + Interactive CT Generalist)"]
        B -->|Adds Multi-Modality & Video Propagation| C["MedSAM2 [M07]<br/>(3D-as-Video Memory Stream)"]
        B -->|Adds Text Prompting & Existence Head| D["BiomedParse v2 [M08]<br/>(BoltzFormer Multi-Modal Parser)"]
        B -->|Adds Uncertainty Guidance| E["SAT3D [M06]<br/>(Critic-Guided Whole-Body Tumour FM)"]
        C -->|Full Parameter Adaptation| F["Medical SAM3 [M09]<br/>(Universal Prompt Segmenter)"]
    end

    style A fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style B fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    style C fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style D fill:#ede7f6,stroke:#512da8,stroke-width:2px
    style E fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style F fill:#f9fbe7,stroke:#afb42b,stroke-width:2px
```

---

## 2. Master Module 01 Model Registry & Comparative Matrix

| Canonical ID | Model System | System Class | Scope | Primary Modalities | Pretraining Scale | Evidence Code | Availability Tier | Workstation VRAM | Model Card |
|:---:|---|:---:|:---:|---|---|:---:|:---:|:---:|:---:|
| **[M04]** | **nnU-Net v2** | `companion` | `workflow_specialist` | 3D/2D CT, MRI, PET, Microscopy | Task-specific training (Scratch) | `[E1]` | **Tier A** (Open) | 8–24 GB | [Card](./nnunet_v2.md) |
| **[M05]** | **VISTA3D / NV-Segment** | `core_fm` | `modality_generalist` | 3D CT (NV-Segment adds MRI) | 11,454 CT volumes (132 classes) | `[E1]` / `[E2]` | **Tier A** (Open) | 14–18 GB | [Card](./vista3d.md) |
| **[M06]** | **SAT3D** | `core_fm` | `workflow_specialist` | 3D CT & MRI (Pan-cancer) | 17,075 3D volume-mask pairs | `[E1]` | **Tier A** (Open) | 12–16 GB | [Card](./sat3d.md) |
| **[M07]** | **MedSAM2** | `core_fm` | `modality_generalist` | 3D CT/MRI/PET & 2D Video | >455k 3D scans + 76k video frames | `[E3]` / `[E2]` | **Tier A** (Open) | 8–12 GB | [Card](./medsam2.md) |
| **[M08]** | **BiomedParse v2** | `core_fm` | `generalist` | 9 Modalities (CT, MRI, WSI, US) | Million-scale image-mask triplets | `[E2]` (v1 `[E1]`)| **Tier A** (Open) | 6–10 GB | [Card](./biomedparse_v2.md) |
| **[M09]** | **Medical SAM3** | `core_fm` | `generalist` | 10 Modalities (2D & 3D) | 33 Datasets (>2M mask instances) | `[E3]` | **Tier A** (Open) | 12–16 GB | [Card](./medical_sam3.md) |

---

## 3. Interactive Prompt Paradigms & Mechanism Comparison

The promptable models in this module employ fundamentally distinct prompting mechanisms to condition feature decoding:

| Model | Prompt Interface | Internal Conditioning Mechanism | 3D Volumetric Handling | Unique Operational Advantage |
|---|---|---|---|---|
| **nnU-Net v2** | None (Deterministic) | Self-configuring dynamic heuristics + empirical ensembling | Full native 3D sliding window | Maximum boundary fidelity; zero prompt ambiguity |
| **VISTA3D** | Spatial Clicks (Pos/Neg) + Class Indices | 3D supervoxel clustering + 2D feature distillation | Native 3D convolutional transformer | Unifies automated 132-class parsing with fast click refinement |
| **SAT3D** | Clicks, Boxes & Critic Maps | Swin-ViT + auxiliary uncertainty critic loss feedback | 3D shifted-window transformer | Guided by predictive uncertainty; excels on infiltrative tumours |
| **MedSAM2** | Keyframe Bounding Boxes / Points | SAM 2 streaming memory attention bank | 3D-as-video slice propagation | >85% reduction in user annotation effort across 100+ slices |
| **BiomedParse v2**| Natural Language Queries | Text projection + Boltzmann attention sampling | 2.5D multi-slice context | Built-in existence classifier prevents false-positive hallucinations |
| **Medical SAM3** | Mixed (Text + Bounding Boxes) | Full SAM3 transformer fine-tuning across 33 datasets | 2D/3D contextual chunking | Bridges the medical domain gap without requiring manual boxes |

---

## 4. Ground-Layer Pretraining Contamination Audit

When evaluating models in this module against benchmarks in [`docs/01_datasets/01_segmentation_3d/`](../../01_datasets/01_segmentation_3d/README.md), observe the **Contamination Doctrine**:

1. **In-Distribution Overlap**: VISTA3D, MedSAM2, and Medical SAM3 were pretrained on large aggregations of public biomedical datasets. Their evaluations on **TotalSegmentator, AMOS22, KiTS23, and MSD** are strictly **in-distribution evaluations**. Superiority over baselines on these datasets reflects dataset familiarity rather than zero-shot generalization.
2. **True External Validation**: Generalization must be audited against external, multi-center cohorts withheld from pretraining (e.g., [RAD-ChestCT](../../01_datasets/02_radiology_ct_mri/rad_chestct.md), private hospital PACS, or blinded competitive challenge test sets).
3. **The nnU-Net Baseline Requirement**: Any foundation model paper that compares its performance only against a generic vanilla 2D U-Net or an un-tuned SwinUNETR—while omitting properly tuned **nnU-Net v2 ResEnc**—must be treated with extreme skepticism.

---

## 5. Verification Protocol

All model cards in this module include executable verification scripts adhering to the **Tier A (Fully Open)** specification:
- Run syntactically verified architectural tests:
  ```bash
  python docs/02_models/01_segmentation/nnunet_v2.md # extract code snippet
  ```
- All dependencies are clearly annotated via `# Requirements: pip install ...`.
