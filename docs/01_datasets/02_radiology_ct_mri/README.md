# 02: Volumetric CT & MRI Radiology Datasets

Welcome to the **Volumetric CT & MRI Radiology Benchmark Library**. This module provides evidence-audited dataset distillation cards following the **D-A-T-A-S + Leaderboard** schema for 3D computed tomography (CT) and magnetic resonance imaging (MRI) corpora. These datasets pair volumetric imaging with free-text radiology reports, dense spatial groundings, and multi-label clinical matrices.

---

## 1. Master Comparative Benchmark Matrix

| Canonical ID | Dataset / Benchmark | Primary Modality | Target Anatomical & Pathological Scope | Verified Scale & Counting Unit | SOTA Winner / Landmark Architecture | Evaluation Setting | Evidence Code | Quick Link |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **`[D20]`** | **CT-RATE** | 3D Non-Contrast Chest CT + Reports | Thoracic Cavity (18 clinical abnormality categories + narrative text) | 21,304 patients, 25,692 studies, 50,188 reconstructed volumes | CT-CLIP (Fine-Tuned / LoRA) — AUROC **0.947**, Macro-F1 **0.762** | Official Held-out Test Split ($N=3,260$) | `E1+E2` | [Card](./ct_rate.md) |
| **`[D21]`** | **RadGenome-ChestCT** | 3D Chest CT + Grounded Reports & VQA | 197 anatomical & pathological classes with 3D masks and grounded text | 25,692 CT volumes, 665k grounded reports, 1.2M (V.O.R.) / 1.3M (Live) VQA pairs | RadGenome-LLaMA3-8B — GVQA Acc **78.4%**, RadGraph F1 **0.612** | Official Held-out Test Split ($N=2,500$) | `E1+E2` | [Card](./radgenome_chestct.md) |
| **`[D22]`** | **PatchChestCT** | 3D Chest CT + Spatial Patches | 9 clinically critical chest abnormalities on a $24 \times 12 \times 12$ 3D grid | 2,201 physician-reviewed studies, 3,456 patches/vol (>7.6M patch targets) | Patch-Supervised 3D ViT-Base — Mean Patch DSC **0.422**, AUROC **0.892** | Held-out Test Split ($N=440$) | `E1+E2` | [Card](./patch_chestct.md) |
| **`[D23]`** | **MR-RATE** | 3D Multimodal MRI (Brain/Spine) + Reports | Neuroimaging & Spine (T1, T2, FLAIR, SWI, MRA; multi-disease scope) | 83,425 patients, 98,334 clinical studies, 705,254 series/volumes | Multi-Sequence 3D Contrastive ViT — AUROC **0.894**, R@5 **48.2%** | Official Held-out Test Split ($N=9,800$) | `E2` | [Card](./mr_rate.md) |
| **`[D30]`** | **Merlin Abdominal CT** | 3D Abdominal/Pelvic CT + Reports/EHR | Abdomen & Pelvis (Multi-organ diagnostic, prognostic, and screening; 752 tasks) | 18,317 patients, 25,494 released CT scans and paired narrative reports | Merlin 3D VLM (Fine-Tuned) — Mean AUROC **0.884** (Ext. **0.832**) | Internal Test ($N=5,137$) / External ($N=44,098$) | `E1` | [Card](./merlin_abdominal_ct.md) |
| **`[D31]`** | **RAD-ChestCT** | 3D Chest CT + Multi-Label Matrix | Thoracic Cavity (84 abnormalities across 52 anatomical locations) | 19,661 adult patients, 35,747 full cohort, 3,630 openly released scans | CT-CLIP (Zero-Shot Transfer) — Mean AUROC **0.874** (±0.006) | External Validation Split ($N=1,344$) | `E1+E2+E5A` | [Card](./rad_chestct.md) |

---

## 2. Genealogy & Contamination Architecture

The volumetric CT/MRI landscape is structured by clear parent-to-derivative hierarchies and distinct institutional cohorts:

```
                          [Volumetric Radiology Landscape]
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        │                                │                                │
  [Thoracic CT Branch]          [Abdominal CT Branch]            [Neuro/Spine MRI Branch]
        │                                │                                │
  CT-RATE [D20] (Istanbul)      Merlin [D30] (Stanford)          MR-RATE [D23] (Forithmus)
  (25,692 Studies / 50k Vols)   (25,494 Scans / Reports)         (83k Patients / 705k Series)
        │                                │                                │
   ┌────┴────────────────────────┐       │ (Independent                   │ (Emerging E2 Live
   │                             │          Institutional                   Dataset Resource)
   ▼                             ▼          Counterweight)                ▼
RadGenome-ChestCT [D21]   PatchChestCT [D22]                     ├── MR-RATE-coreg
(Model-Assisted 197 Masks  (Physician-Reviewed                   ├── MR-RATE-atlas
 + 1.2M/1.3M VQA Pairs)    Patch Grid 24x12x12)                  └── MR-RATE-nvseg-ctmr
                                                                     (Model Predictions!)

──────────────────────────────────────────────────────────────────────────────────────────
[Independent Out-of-Distribution External Validation Cohort]
   RAD-ChestCT [D31] (Duke University: 3,630 Open Scans / 35,747 Full Cohort) [E5A]
   -> Zero-shot transfer benchmark establishing genuine cross-institution generalizability.
```

### Critical Genealogy Rules & Contamination Warnings
1. **Parent-to-Derivative Overlap:** **CT-RATE $\rightarrow$ RadGenome-ChestCT / PatchChestCT** represents a single clinical patient cohort with differing layers of annotation. A model pretrained on CT-RATE that is subsequently evaluated on RadGenome-ChestCT or PatchChestCT has **not** demonstrated institutional generalizability; it demonstrates supervision transfer on previously seen patient anatomies.
2. **The External Validation Imperative (`E5A`):** True generalizability requires testing on strictly independent health-system cohorts. **RAD-ChestCT `[D31]`** (Duke University) and **Merlin `[D30]`** (Stanford Health Care) serve as canonical independent external anchors.
3. **Supervision Provenance Separation:**
   - **Native human clinical reports:** CT-RATE, Merlin, MR-RATE.
   - **Physician-reviewed spatial patches:** PatchChestCT.
   - **Model-assisted / synthetic grounding:** RadGenome-ChestCT (TotalSegmentator + LLM extraction), MR-RATE derived segmentations (NV-Segment-CTMR).

---

## 3. Hard Numerical Counting Hygiene Rules

Never conflate counting units in volumetric radiology. In this module, the fundamental law of data accounting applies strictly:

$$\text{patient} \neq \text{examination / study} \neq \text{series} \neq \text{reconstructed volume} \neq \text{2D slice} \neq \text{voxel mask}$$

### Audited Case Studies:
- **CT-RATE `[D20]`:** Contains 21,304 patients and 25,692 studies, expanded to **50,188 reconstructed volumes**. The volume expansion occurs because multiple reconstruction filters (e.g., sharp lung kernel vs. soft-tissue kernel) are generated from single CT acquisitions. Reporting 50,188 independent patient examinations is factually false.
- **MR-RATE `[D23]`:** Reports 83,425 patients, 98,334 clinical studies, and **705,254 MRI series/volumes**. Multi-parametric MRI acquires an average of 7.2 pulse sequences per exam. These 705,254 series are not 705,254 independent patient encounters.
- **Merlin `[D30]`:** The *Nature* data-availability statement specifies a **released cohort of 25,494 scans from 18,317 unique patients**. This must be clearly distinguished from the internal pretraining corpus (15,331 scans), the internal test set (5,137 scans), and the external validation cohorts (44,098 scans).
- **RAD-ChestCT `[D31]`:** The full hospital archive contains 35,747 scans from 19,661 adults, but the **open Zenodo release contains exactly 3,630 scans**. Never report evaluations on the open release as having evaluated 35,747 scans.

---

## 4. Core Epistemic Lessons from the Ground Layer

1. **Native 3D vs. 2D Slice Pooling:**
   Native volumetric 3D modeling (e.g., 3D Vision Transformers in CT-CLIP, Merlin, and Triad) consistently outperforms slice-averaged 2D baselines across chest and abdominal abnormality detection. However, 3D models require careful attention to z-axis slice thickness and spacing anisotropy.
2. **Focal vs. Diffuse Pathology Gap:**
   As demonstrated in independent comparative CT foundation model audits (Tagscherer et al., 2026 [S81]), frozen 3D representations perform well on diffuse, organ-level pathologies (e.g., cardiomegaly, hepatomegaly) but struggle significantly with small focal lesions (e.g., subcentimeter pulmonary nodules, adrenal adenomas).
3. **The Emerging Resource Boundary (`E2` vs. `E1`):**
   A live open dataset card is an emerging data engineering contribution (`E2`), not peer-reviewed proof of foundation model superiority. For example, MR-RATE is an indispensable data resource, but its paper and foundation model remain marked "Coming Soon"; claiming peer-reviewed MR-RATE model SOTA is scientifically invalid.

---

## 5. Recommended Study Sequence (The 5–15–60 Funnel)

1. **Tier 1 (5 Minutes):** Review the Master Comparative Matrix above to match your clinical question to anatomical region (Thorax vs. Abdomen vs. Neuro/Spine) and supervision type (Report-paired vs. Patch-annotated vs. Mask-grounded).
2. **Tier 2 (15 Minutes):** Open the specific dataset card to inspect the **D-A-T-A-S** dimensions, hardware footprint, and verified Top-5 SOTA leaderboards.
3. **Tier 3 (60 Minutes):** Execute the minimal local verification Python snippet provided in Section 4 of each card to inspect scan geometries, DICOM/NIfTI headers, and label representations.
