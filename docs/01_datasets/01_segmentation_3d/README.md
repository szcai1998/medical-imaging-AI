# 01: 3D Segmentation & Anatomical Benchmark Datasets

Welcome to the **3D Segmentation & Anatomical Benchmark Library**. This module provides evidence-audited dataset distillation cards following the **D-A-T-A-S + Leaderboard** schema, extracting the clinical problem, acquisition scale, annotation truth, access terms, and verified Top-5 SOTA leaderboards from official challenge portals.

---

## 1. Master Comparative Benchmark Matrix

| Canonical ID | Dataset / Benchmark | Primary Modality | Target Anatomical & Pathological Scope | Verified Scale & Counting Unit | SOTA Winner / Landmark Architecture | Evaluation Setting | Evidence Code | Quick Link |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **`[D9]`** | **KiTS23** | 3D CECT | Kidneys, Renal Tumors, and Renal Cysts | 599 patients (489 train, 110 held-out test) | Auto3DSeg Ensemble (NVIDIA) — DSC **0.835** | Official Blind Test ($N=110$) | `E2+E1` | [Card](./kits23.md) |
| **`[D10]`** | **AMOS22** | 3D CT & MRI | 15 Abdominal Multi-Organs | 600 patients (500 CT, 100 MRI) | Metric-Aware nnU-Net (MIC-DKFZ) — DSC **0.908** | Official Blind Test ($N=140$) | `E2+E1` | [Card](./amos22.md) |
| **`[D6]`** | **TotalSegmentator** | 3D Whole-Body CT | 104 structures (v1) / 117+ structures (v2) | 1,204 CT examinations | TotalSegmentator v2 (nnU-Net Cascade) — DSC **0.943** | Held-out Test Split ($N=168$) | `E1+E2` | [Card](./totalsegmentator.md) |
| **`[D1/D2]`** | **CVPR-BiomedSegFM** | Multi-Modal 3D (CT, MRI, PET, US, Micro) | Text-Prompted 3D Universal Segmentation (200+ classes) | >200,000 3D image–mask pairs | BiomedParse v2 (Microsoft) — DSC **0.864** | Blind Codabench Test | `E2` | [Card](./cvpr_biomedsegfm.md) |
| **`[D3]`** | **FLARE PanCancerCTSeg** | 3D CT | 13 Organs + Pan-Cancer Lesions | >17,000 labeled cancer CT scans | GF-Screen Framework — DSC **0.912** (Org) / **0.648** (Tumor) | Blind Challenge Test ($N=400$) | `E2` | [Card](./flare_pancancer_ctseg.md) |
| **`[D4]`** | **AbdomenAtlas** | 3D CT | 22 Abdominal Organs & Vasculature | 20,460 CT volumes across 112 hospitals | ResEnc-L nnU-Net v2 — DSC **0.918** | Multi-Center External Test | `E1` | [Card](./abdomen_atlas.md) |
| **`[D5]`** | **AbdomenAtlas 3.0** | 3D CT + Reports | Multimodal 3D Image–Mask–Report Triplets | 9,262 CT studies (3,955 tumor-positive) | RadGPT + 3D ResEnc Head — DSC **0.684** / RadGraph F1 **0.628** | Held-out Test Split ($N=1,850$) | `E1` | [Card](./abdomen_atlas_3.md) |
| **`[D7]`** | **MSD (Decathlon)** | 3D CT & MRI | 10 Diverse Anatomical & Oncological Tasks | 2,633 multi-modality 3D scans | nnU-Net (MIC-DKFZ) — Decathlon Winner | Blind Mystery Test Phase | `E1+E2` | [Card](./msd_decathlon.md) |
| **`[D8]`** | **LUNA16** | 3D Thoracic CT | Pulmonary Nodules ($\ge 3\text{ mm}$) | 888 thoracic CT scans (1,186 nodules) | DeepLung 3D ResNet — CPM **0.958** | 10-Fold Cross-Validation | `E1+E2` | [Card](./luna16.md) |
| **`[D11A]`** | **autoPET / autoPET II** | 3D $^{18}\text{F-FDG}$ PET/CT | Whole-Body Metabolic Cancer Lesions | 1,014 PET/CT studies (900 patients) | nnU-Net Dual-Stream (MIC-DKFZ) — DSC **0.784** | Official Blind Test ($N=150$) | `E1+E2` | [Card](./autopet.md) |

---

## 2. Core Epistemic Lessons from the Ground Layer

When studying or designing research on 3D segmentation, keep three mandatory principles from the master dossier in mind:

### 1. The nnU-Net Principle
Foundation models have **not** made self-configuring task-specific models obsolete. Across competitive benchmarks (MSD, AMOS22, KiTS23, TotalSegmentator), optimized variants of **nnU-Net v2** continue to win or serve as the mandatory reference comparator. A paper claiming SOTA that does not benchmark against a tuned nnU-Net baseline is methodologically incomplete.

### 2. Numerical Counting Hygiene
Never equate different counting units. In this directory:
$$\text{patient} \neq \text{examination / study} \neq \text{series} \neq \text{reconstructed volume} \neq \text{2D slice} \neq \text{mask}$$
For example, AbdomenAtlas's "673K masks" represents individual anatomical segmentations across 20,460 scans—it is not 673,000 independent examinations.

### 3. Circular Validation & Contamination Traps
Because mega-benchmarks like **CVPR-BiomedSegFM `[D1]`** and **FLARE PanCancerCTSeg `[D3]`** aggregate dozens of public datasets (KiTS, AMOS, MSD, TotalSegmentator), general foundation models (such as MedSAM, VISTA3D, BiomedParse) have seen their training cases during pretraining. Evaluating these models on constituent benchmarks without explicit data exclusions measures *memorization and in-domain transfer*, **not zero-shot generalization**.

---

## 3. Recommended Study Sequence (The 5–15–60 Funnel)

1. **Tier 1 (5 Minutes):** Scan the master table above to classify the benchmark into your clinical target (Organ vs. Tumor vs. Whole-Body vs. Multimodal).
2. **Tier 2 (15 Minutes):** Open the specific dataset card to study the **D-A-T-A-S** dimensions, hardware requirements, and Top-5 SOTA leaderboards.
3. **Tier 3 (60 Minutes):** Run the minimal local load snippet provided in Section 4 of each card to inspect scan geometries, voxel spacings, and label distributions.
