# 04: Pathology & Spatial Biology Benchmark Datasets

Welcome to the **Pathology & Spatial Biology Benchmark Library**. This module provides evidence-audited dataset distillation cards following the standardized **D-A-T-A-S + Leaderboard** schema, synthesizing clinical target domains, acquisition physics, annotation truth provenance, access terms, and verified Top-5 SOTA leaderboards across gigapixel whole-slide imaging (WSI), multi-center patch suites, and multiplexed spatial biology.

---

## 1. Master Comparative Benchmark Matrix

| Canonical ID | Dataset / Benchmark | Primary Modality | Target Anatomical & Clinical Scope | Verified Scale & Counting Unit | SOTA Winner / Top System | Evaluation Setting | Evidence Code | Quick Link |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **`[D25]`** | **TCGA** | Brightfield WSI (H&E FFPE & Frozen) + Multi-Omics | Pan-cancer across 33 primary tumor sites; subtyping, mutation & survival prediction | 11,000+ patients; 30,071 WSIs; >2.5B tiles | Virchow2 + Attn-MIL — AUROC **0.912** / C-index **0.648** | 5-Fold CV / Pan-Cancer (In-Domain by default) | `E1+E2+E5A` | [Card](./tcga.md) |
| **`[D26]`** | **CPTAC** | Brightfield WSI (H&E FFPE & Frozen) + Mass-Spec Proteomics | Multi-cancer proteogenomics (10+ cohorts); protein abundance & molecular subtyping | 1,500+ patients; >5,000 WSIs; >500M tiles | Virchow2 + Attn-MIL — AUROC **0.884** / F1 **0.812** | Held-out CPTAC Validation Cohorts | `E1+E2+E5A` | [Card](./cptac.md) |
| **`[D27]`** | **CAMELYON16 / CAMELYON17** | Brightfield WSI (H&E FFPE Lymph Nodes) | Breast cancer sentinel lymph node metastasis detection & patient pN-staging | 200 patients; 1,000 WSIs (5 slides/pt) across 5 Dutch medical centres | Lunit Inc. Framework — Patient pN-Kappa **0.8993** *(post: **0.9203**)* | Official Grand Challenge Blind Test ($N=100$ pts / 500 WSIs) | `E1+E2+E5A` | [Card](./camelyon17.md) |
| **`[D28]`** | **PANDA** | Brightfield WSI (H&E FFPE Core Biopsies) | Prostate cancer diagnosis, Gleason scoring, and ISUP Grade Groups (0–5) | 10,616 dev WSIs; >1,000 hidden test WSIs (Radboud UMC + Karolinska) | Team PND — Quadratic Weighted Kappa ($k_w$) **0.9409** | Official Kaggle Private Test ($N > 1,000$) | `E1+E2+E5A` | [Card](./panda.md) |
| **`[D29]`** | **GigaTIME Benchmark** | Cross-Modal H&E $\rightarrow$ Virtual 21-Channel mIF | Pan-cancer tumor immune microenvironment (TIME) mapping across 24 cancers | 50 public paired test patches (vs. 14,256 proprietary training patients) | GigaTIME (Providence/MSFT) — Cell PCC **0.784** / SSIM **0.762** | Paired Multi-Center Test Patches ($N=50$) | `E1+E2` | [Card](./gigatime_benchmark.md) |
| **`[S29/S28]`** | **PathoROB Suite** | Multi-Center Patch Suites (H&E FFPE 2D Patches) | Multi-organ histology subtyping, tissue typing, and technical confound invariance | >165,000 patches across 34 centres (BRACS, BACH, UnitoPatho, SICAPv2, BreakHis, LC25000, MHIST, NCT-CRC-HE) | UNI2-h — Robustness Index **0.842** (Bio AUROC **0.914** / Confound **0.612**) | Standardized Multi-Center Robustness Protocol | `E1+E5A` | [Card](./pathorob_suite.md) |

---

## 2. Foundational Architectural Hierarchy: The Tile-to-Slide Pyramid

Whole-slide histopathology images present an extreme spatial scale problem unique in medical imaging: a single biopsy or surgical resection scan measures up to $100{,}000 \times 100{,}000$ pixels (several gigapixels uncompressed), containing tens of thousands of individual cells. 

To process this data without exceeding GPU memory boundaries, the field operates along a strict multi-scale spatial hierarchy:

```
[Sub-cellular Organelle] (Chromatin texture, nucleoli, mitotic spindles)
       │
       ▼
[Single Cell / Nucleus] (Segmented boundaries: StarDist, Mesmer, HoVer-Net)
       │
       ▼
[Patch / Tile Layer] (256x256 to 512x512 px @ 20x/40x: Virchow2, UNI2-h, Prov-GigaPath, H-Optimus-1)
       │
       ▼
[Adaptive Morphological Region / ROI] (Tissue clusters, tertiary lymphoid structures: CARE [S90])
       │
       ▼
[Whole-Slide Image (WSI)] (10,000 - 50,000 tiles pooled via AB-MIL, TransMIL, LongNet, nnMIL)
       │
       ▼
[Patient Examination / Case] (Aggregation across multiple slides, e.g., CAMELYON17 5 slides/pt)
       │
       ▼
[Clinical Cohort] (Multi-center epidemiological or trial cohort: TCGA, CPTAC)
```

### Mathematical & Computational Consequences
1. **The Counting-Unit Law:** Never conflate counting units:
   $$\text{Patient} \neq \text{Examination} \neq \text{Slide (WSI)} \neq \text{Tile / Patch} \neq \text{Segmented Nucleus}$$
   Reporting "a dataset of 500,000 images" when referring to 500,000 tiles extracted from 50 patients is a severe methodological violation.
2. **The Two-Stage Representation Paradigm:** Direct end-to-end training of backbones on whole slides is computationally intractable. State-of-the-art workflows decouple representation learning into:
   - **Stage 1 (Tile Encoder):** Self-supervised vision transformers (ViT-B to ViT-G) extracting dense feature vectors per tile.
   - **Stage 2 (Slide Aggregator):** Multiple Instance Learning (Attention-MIL, CLAM, TransMIL), sub-quadratic transformers (LongNet in Prov-GigaPath), or adaptive region aligners (CARE).

---

## 3. The TCGA Contamination Doctrine

A critical epistemic rule established in the Ground-Layer Master Dossier (`Section 3A.6` and `Section 17.3`):

> **The Cancer Genome Atlas (TCGA `[D25]`) must be treated as IN-DISTRIBUTION / CONTAMINATED by default for modern foundation models.**

### Why TCGA Cannot Prove Generalization
1. **Ubiquitous Pretraining Inclusion:** Because TCGA diagnostic and tissue slides have been openly available through the NCI Genomic Data Commons for over a decade, almost every modern pathology foundation model (Prov-GigaPath, Virchow, Virchow2, UNI, UNI2-h, CONCH, Phikon, CTransPath, PLIP, H-Optimus-1) ingested TCGA into its pretraining mixture.
2. **The In-Domain Illusion:** When a paper demonstrates high zero-shot or linear-probing AUROC on TCGA cohorts (e.g., predicting *TP53* mutation in TCGA-BRCA or subtyping TCGA-NSCLC), it is measuring **in-domain representation alignment and training memorization**, not out-of-distribution (OOD) generalization.
3. **Evidence Hygiene Standard:** True external generalization requires testing on genuinely independent clinical cohorts that were verifiably excluded from the pretraining manifest:
   - **CPTAC (`[D26]`):** Multi-center proteogenomic cohorts.
   - **CAMELYON17 Blind Test (`[D27]`):** Held-out slides from 5 Dutch centers.
   - **PANDA Hidden Test (`[D28]`):** Multi-scanner prostate biopsies.
   - **PathoROB External Suite (`[S29]`):** 8 multi-center international patch benchmarks.

As demonstrated in the 32-model *Nature Communications* benchmark (Bareja et al., 2026 `[S28]`), rigorous benchmarking protocols **explicitly separate TCGA evaluation tasks from external non-TCGA evaluation tasks**.

---

## 4. Ground-Layer Model-Selection Insights

Selecting a computational pathology foundation model is **not a parameter-count contest**. Empirical findings across 2026 landmark studies (*Nature Communications* 17, 9012 `[S28]`; *Nature Communications* 17, 5218 `[S29]`; *Scientific Reports* 2 Sept 2026 `[S47]`) establish five multi-objective selection rules:

### 1. The Leading Cluster Reality
No single foundation model universally dominates every task. In the 32-model PathBench evaluation, **Virchow2**, **Prov-GigaPath**, and **UNI2-h** formed a tightly clustered leading tier with overlapping confidence intervals across pan-cancer diagnostic tasks. Performance varies significantly across organ systems, stain intensities, and tissue preparation types.

### 2. Biological Utility vs. Technical Robustness (The PathoROB Metric)
Pathology foundation models strongly encode non-biological technical features (medical center identity, slide scanner optics, and staining reagent batches). In Kömen et al. (`[S29]`), medical center could be decoded from frozen embeddings with up to **$95\%$ accuracy**. 
- A model with higher in-domain AUROC may simply be exploiting acquisition shortcuts.
- Always inspect the **PathoROB Robustness Index ($R_{\text{idx}}$)** to verify that high accuracy is paired with technical confound invariance.

### 3. Throughput and Deployment Economics
Model selection depends heavily on inference throughput and compute budget:
- Massive gigapixel encoders (e.g., 1.1B H-Optimus-1 or 632M Virchow2) require high-end datacenter GPUs (A100/H100) and substantial VRAM for tile extraction.
- Compact and distilled student models (e.g., the 22M parameter **GigaPath-Flash** `[S33]` or ViT-B variants) deliver comparable linear-probing performance on routine classification tasks with a fraction of the inference latency and energy cost.

### 4. Virtual Morphology vs. Measured Spatial Proteomics
In spatial biology, maintain the fundamental epistemic boundary:
- **Virtual Staining (GigaTIME `[D29]`):** Synthesizes multiplex immunofluorescence markers (e.g., CD8, PD-L1) from standard H&E morphology. This is a generative statistical hypothesis conditioned on morphology, **not physical ground truth**.
- **Measured Spatial Foundation Models (VirTues `[S60]`):** Directly models multi-channel measured spatial-proteomic imaging across proteins, cells, and niches.

---

## 5. Recommended Study Sequence (The 5–15–60 Funnel)

1. **Tier 1 (5 Minutes):** Scan the Master Comparative Benchmark Matrix above to select the benchmark matching your clinical question (Pan-Cancer vs. Breast LN vs. Prostate vs. Spatial Multiplex vs. Multi-Center Robustness).
2. **Tier 2 (15 Minutes):** Open the dedicated dataset card to review the **D-A-T-A-S** dimensions, counting units, access terms, and verified Top-5 SOTA leaderboards.
3. **Tier 3 (60 Minutes):** Run the minimal local load snippet provided in Section 4 of each card using `openslide`, `tifffile`, or `PIL` to inspect multi-resolution pyramid levels, physical pixel spacing (MPP), and stain profiles.
