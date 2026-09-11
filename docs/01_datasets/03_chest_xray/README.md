# 03: Chest Radiography (CXR) Benchmark & Representation Datasets

Welcome to the **Chest Radiography (CXR) Benchmark & Representation Library**. This module provides evidence-audited dataset distillation cards following the **D-A-T-A-S + Leaderboard** schema. It extracts the clinical problem, acquisition scale, annotation truth provenance, access terms, and verified Top-5 SOTA leaderboards across the mature, highly derivative chest X-ray ecosystem.

---

## 1. Master Comparative Benchmark Matrix

| Canonical ID | Dataset / Benchmark | Primary Modality & Projections | Target Clinical Scope | Verified Scale & Counting Units | SOTA Winner / Landmark Architecture | Evaluation Setting | Evidence Code | Quick Link |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **`[D12]`** | **MIMIC-CXR v2.1.0** | Digital & Computed Radiography (PA, AP, Lateral) | Multimodal thoracic report generation, VQA, and pretraining | 377,110 DICOM images across 227,835 studies (65,379 patients) | MAIRA-2 (Microsoft) — RadGraph-F1 **0.388** / BLEU-4 **0.243** | Official Test Set ($N=3,858$ studies) | `E1+E2` | [Card](./mimic_cxr.md) |
| **`[D13]`** | **CheXpert** | Digital & Computed Radiography (PA, AP, Lateral) | Multi-label classification with explicit uncertainty modeling | 224,316 radiographs from 65,240 patients | DeepAUC-v1 / Hierarchical-V1 (VinBrain) — Mean AUROC **0.930** | Official Hidden Test ($N=500$ studies, 5-Rad consensus) | `E1+E2` | [Card](./chexpert.md) |
| **`[D14]`** | **PadChest** | Digital & Computed Radiography (PA, AP, Lat, Lordotic) | Fine-grained anatomical ontology & European/Spanish shift | 160,868 radiographs across 109,931 studies (67,625 patients) | Ark+ (ASU / Mayo) — Mean AUROC **0.869** | Radiologist-Annotated Subset ($N=39,053$) | `E1+E2` | [Card](./padchest.md) |
| **`[D15]`** | **NIH ChestX-ray14** | Digital & Computed Radiography (PA and AP frontal) | Historical weakly supervised classification benchmark | 112,120 frontal CXRs from 30,805 patients | Ark+ — Mean AUROC **0.865** / TorchXRayVision — **0.858** | Official Patient Split (`test_list.txt`, $N=25,596$) | `E1+E2` | [Card](./nih_chestxray14.md) |
| **`[D16]`** | **VinDr-CXR** | Digital & Computed Radiography (PA frontal) | Multi-reader consensus local detection (22 boxes) + 6 global diagnoses | 18,000 CXRs from 18,000 patients (15k train / 3k test) | Keep Going Ensemble — mAP@0.4 **0.332** | 5-Radiologist Consensus Test ($N=3,000$) | `E1+E2` | [Card](./vindr_cxr.md) |
| **`[D17]`** | **MS-CXR** | Digital & Computed Radiography (Frontal MIMIC subset) | Fine-grained phrase grounding (sentence-to-bounding box) | 1,162 phrase-box pairs across 1,026 CXR images | MAIRA-2 Grounding — Mean CNR **1.38** / Box mIoU **0.412** | Official Phrase Grounding Test ($N=1,162$) | `E1+E2` | [Card](./ms_cxr.md) |
| **`[D18]`** | **Chest ImaGenome** | Digital & Computed Radiography (Frontal MIMIC subset) | Structured scene graphs (29 regions) & longitudinal relations | 242,072 silver scene graphs + 500-patient gold consensus set | CoCa-CXR — Macro AUROC **0.884** / Temp Acc **65.0%** | Gold Standard Set ($N=500$ patients) | `E1+E2` | [Card](./chest_imagenome.md) |
| **`[D19A]`** | **BRAX** | Digital & Computed Radiography (PA, AP, Lateral) | Geographic, demographic, and Portuguese NLP domain shift | 40,967 radiographs across 24,959 studies (19,351 patients) | Ark+ — Mean AUROC **0.887** / In-domain DenseNet — **0.871** | Held-out BRAX Test Split ($N=5,000$) | `E1+E2` | [Card](./brax.md) |

---

## 2. Core Epistemic Lessons from the Ground Layer

When engineering or benchmarking models in chest radiography, four mandatory epistemic principles from the master dossier must be strictly observed:

### 1. The Parent $\rightarrow$ Derivative Genealogy Rule
Chest radiography possesses an unusually mature but **deeply derivative ecosystem**. Derivatives add new annotation layers, but they do **NOT** introduce independent patient cohorts:

```
MIMIC-CXR v2.1.0 [D12] (Parent clinical cohort: 377,110 images / 65,379 patients)
   ├── MS-CXR [D17]           [Radiologist phrase-grounding subset: 1,162 pairs / 1,026 images]
   └── Chest ImaGenome [D18]  [Automated scene graphs (242k) + 500-patient gold consensus set]
```

> [!IMPORTANT]
> **Genealogy Rule:** A model pretrained on MIMIC-CXR and subsequently evaluated on MS-CXR or Chest ImaGenome has **NOT demonstrated external institutional generalization**. The underlying anatomy, scanner calibration, and patient demographics are identical. These derivatives measure *fine-grained visual-linguistic alignment or structured relational competence*, not out-of-distribution robustness.

---

### 2. Weak NLP Report-Mined vs. Radiologist Consensus Ground Truth
Never treat all CXR labels as epistemically equivalent. The chest radiography ecosystem spans three distinct tiers of supervision:

| Supervision Tier | Representative Datasets | Scale | Label Provenance & Mechanism | Label Noise Margin | Appropriate Scientific Role |
| :--- | :--- | :--- | :--- | :---: | :--- |
| **Tier 1: Weak NLP Report-Mined** | NIH ChestX-ray14 `[D15]`, CheXpert Train `[D13]`, BRAX `[D19A]`, MIMIC CheXpert `[D12]` | 40k – 377k images | Automated regex / CheXpert / NegBio parsers applied to text reports | **10% – 30%** error rate | Large-scale pretraining, representation learning, coarse multi-label ranking |
| **Tier 2: Stratified / Bimodal Human-AI** | PadChest `[D14]` (27% physician / 73% RNN), Chest ImaGenome `[D18]` (Silver vs. Gold) | 160k – 242k images | Subset directly audited by physicians; remainder labeled via model/pipeline | Stratified by subset | Mixed-supervision learning; evaluation MUST strictly restrict to the human-validated split |
| **Tier 3: Multi-Radiologist Consensus & Spatial Grounding** | VinDr-CXR `[D16]`, MS-CXR `[D17]`, CheXpert Hidden Test `[D13]`, ImaGenome Gold `[D18]` | 500 – 18,000 cases | Multi-reader review (3–5 board-certified radiologists) with panel adjudication | **< 2%** error rate | Authoritative benchmark evaluation, object detection, phrase grounding, clinical audit |

---

### 3. Hard Numerical Counting-Unit Discipline
Always distinguish the physical and semantic hierarchy of radiographic data:
$$\text{patient} \neq \text{examination / study} \neq \text{series} \neq \text{reconstructed volume} \neq \text{2D image / projection} \neq \text{bounding box / annotation pair}$$

- **MIMIC-CXR `[D12]`:** 65,379 patients $\rightarrow$ 227,835 studies $\rightarrow$ 377,110 DICOM images. Images and studies are not interchangeable.
- **The Canonical NIH 112k vs. 108k Discrepancy `[D15]`:** The original CVPR 2017 paper reported 108,948 images from 32,717 patients (ChestX-ray8). The official expanded release contains **112,120 images from 30,805 patients** (ChestX-ray14). Deduplication merged split records (reducing patients), while temporal expansion added validated frontal films (increasing images). Citing the 108k figure for the 14-label release is a documented error.
- **Patient Leakage Hazard:** Patients with chronic thoracic disease undergo dozens of serial examinations across years. Random train/test splitting on image indexes leaks patient identity, anatomy, and scanner artifacts into the test set, creating heavily inflated AUROC figures. **Splitting must strictly occur at the patient level**.

---

### 4. Metric Evolution: Beyond BLEU to RadGraph-F1, GREEN, and RadFact
Early radiology report generation literature relied on machine translation metrics like BLEU-1 through BLEU-4 and ROUGE-L. In clinical practice, these metrics fail catastrophically:
- **Boilerplate Gaming:** A model can achieve a high BLEU score by repeatedly reciting generic negative templates ("The heart size is normal. The lungs are clear. No pneumothorax or pleural effusion.").
- **Clinical Inversion:** Reversing a single word ("pneumothorax" vs. "no pneumothorax") incurs a negligible BLEU penalty while representing a lethal clinical error.

Modern evaluation standards enforce **clinical factuality metrics**:
1. **RadGraph-F1:** Evaluates clinical entities and relations parsed into a structured factual graph.
2. **CheXbert-F1:** Evaluates agreement across 14 categorical thoracic diagnoses.
3. **RadFact & GREEN:** LLM-based clinical rubrics designed to penalize false positives, omissions, and spatial misattributions.

---

## 3. Recommended Study Sequence (The 5–15–60 Funnel)

1. **Tier 1 (5 Minutes):** Scan the Master Matrix above to identify datasets that match your operational regime (Pretraining, Spatial Grounding, Out-of-Distribution Stress Testing).
2. **Tier 2 (15 Minutes):** Open the target dataset card to inspect the **D-A-T-A-S** snapshot, access prerequisites, and verified Top-5 SOTA leaderboards.
3. **Tier 3 (60 Minutes):** Run the minimal verification Python snippet in Section 4 of each card to inspect metadata schemas, verify patient-disjoint splits, and audit view-position headers.
