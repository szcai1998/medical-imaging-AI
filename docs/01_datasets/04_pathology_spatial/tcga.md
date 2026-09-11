# TCGA: The Cancer Genome Atlas Pan-Cancer Pathology & Genomic Ecosystem

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D25]`
- **Domain & Modality:** Brightfield Whole-Slide Imaging (WSI, H&E Formalin-Fixed Paraffin-Embedded [FFPE] & Snap-Frozen Cryosections) paired with Multi-Omics & Clinical Records
- **Target Anatomy & Pathologies:** Pan-cancer across 33 primary tumor sites (breast, lung, prostate, colon, kidney, brain, skin, ovary, bladder, etc.)
- **Release / Milestone Year:** 2006–present (NCI Genomic Data Commons Pan-Cancer Atlas landmark release 2018; live GDC repository)
- **Evidence Code:** `E1` (Peer-Reviewed Pan-Cancer Atlas) + `E2` (Official NCI GDC Portal) + `E5A` (Independent PathBench Benchmark `[S28]`)
- **Access Level:** Two-Tier Access (Diagnostic & Tissue WSIs, somatic mutations, and clinical tables are Open Access; raw germline sequencing reads are Controlled Access via dbGaP)
- **Primary Source / Portal:** [NCI Genomic Data Commons (GDC)](https://portal.gdc.cancer.gov/) | [NCI TCGA Data Types](https://www.cancer.gov/ccg/research/genome-sequencing/tcga/using-tcga-data/types)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Comprehensive multi-dimensional profiling of human oncogenesis. In computational pathology, TCGA serves as the international reference benchmark for computational subtyping, histological grading, molecular alteration prediction from tissue morphology (e.g., microsatellite instability [MSI], homologous recombination deficiency [HRD], *TP53*, *EGFR*, *KRAS*, *BRAF* mutations), and prognostic risk stratification.
- **Target Classes & Prediction Tasks:**
  1. *Pan-Cancer Diagnostic Subtyping:* 33 primary cancer types and >100 distinct histological classifications.
  2. *Molecular & Genetic Alteration Prediction:* Binary and multi-class classification of somatic driver mutations and transcriptomic subtypes directly from H&E.
  3. *Survival & Outcome Prognostication:* Overall Survival (OS), Progression-Free Interval (PFI), and Disease-Specific Survival (DSS) via concordance index (C-index) modeling.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  $$\text{11,000+ Patients} \neq \text{30,071 Whole-Slide Images (WSIs)} \neq \text{>2.5 Billion Patches/Tiles} \neq \text{Single-Cell Nuclei}$$
  - **~11,000+ unique cancer patients** across 33 project codes (e.g., TCGA-BRCA, TCGA-LUAD, TCGA-GBM).
  - **30,071 digitized whole-slide images**, subdivided into diagnostic slides (`.svs`, FFPE surgical resections) and tissue slides (snap-frozen cryosections).
  - Billions of individual non-overlapping tiles (typically extracted at $256 \times 256$ or $512 \times 512$ pixels at $20\times$ [$\approx 0.50\,\mu\text{m/px}$] or $40\times$ [$\approx 0.25\,\mu\text{m/px}$] magnification).
- **Imaging Physics & Scanner Distribution:**
  - Multi-institutional slide scanning across Aperio ScanScope, Hamamatsu NanoZoomer, and Leica SCN400 platforms.
  - Multi-center international distribution spanning 33+ contributing Tissue Source Sites (TSS), producing severe inter-laboratory variations in section thickness ($3\text{--}5\,\mu\text{m}$), hematoxylin/eosin staining protocols, and color balance.

### [T] Truth & Annotation Provenance
- **Reference Standard:** Institutional pathology reports, histological consensus reviews by TCGA disease-specific expert working groups, and molecular assays (whole-exome sequencing, RNA-seq, DNA methylation, reverse-phase protein arrays).
- **Label Provenance Category:** Native human expert clinical adjudication paired with biochemical ground truth.
- **Critical Annotation Caveat:** Canonical TCGA releases provide **slide-level and patient-level metadata only**. Dense pixel-level tumor or nuclear segmentations were *not* included in the original release; dense masks must be sourced from downstream secondary projects (e.g., NuCLS, TCGA-TIL, or manual pathologist markups).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Accessible via the official NCI `gdc-client` tool or the GDC REST API using project-specific manifests.
- **License / Terms:** Open Access under NIH/NCI data-sharing regulations. Unrestricted academic and commercial use permitted for open-tier files (diagnostic WSIs, clinical metadata, derived somatic variants), subject to standard NIH citation requirements.
- **Artifact Availability:** Full-resolution multi-gigabyte pyramidal TIFF/SVS files, manifest files, clinical TSVs, and Mutation Annotation Format (MAF) files.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Initiated by the National Cancer Institute (NCI) and National Human Genome Research Institute (NHGRI).
- **Derivative Ecosystem:** Parent cohort for hundreds of computational pathology sub-datasets, including NuCLS (nuclear segmentation), TCGA-TIL (tumor-infiltrating lymphocytes), and standard MIL evaluation splits.
- **Contamination & Overlap Doctrine (MANDATORY):**
  - **TCGA must be treated as IN-DISTRIBUTION / CONTAMINATED by default for modern pathology foundation models.**
  - Because TCGA has been open and ubiquitous for over a decade, almost all leading pathology foundation models (Prov-GigaPath, Virchow, Virchow2, UNI, UNI2-h, CONCH, Phikon, CTransPath, PLIP, H-Optimus-1) incorporated TCGA slides or tiles into their pretraining mixtures.
  - Zero-shot or linear-probing performance claims evaluated on TCGA reflect **in-domain representation alignment and memorization**, not out-of-distribution (OOD) generalization.
  - The 32-model *Nature Communications* benchmark (Bareja et al., 2026 `[S28]`) explicitly isolates TCGA tasks from external non-TCGA cohorts, warning that public dataset overlap cannot be excluded for TCGA.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standardized pan-cancer evaluation across slide-level subtyping, genetic biomarker prediction, and survival prognostication, drawing from the 32-model PathBench benchmark (Bareja et al., Nature Communications 17, 9012, 2026 [S28]) and authoritative foundation model publications.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Pan-Cancer Mean AUROC | Pan-Cancer C-Index | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Virchow2 + Attn-MIL** | Paige AI / MSKCC | ViT-H (632M params) tile encoder trained on 3.1M WSIs; slide-level attention MIL pooling | 5-Fold Cross-Validation on TCGA Pan-Cancer cohorts | **0.912** | **0.648** | [arXiv:2408.00738](https://arxiv.org/abs/2408.00738) / [PathBench](https://pathbench.stanford.edu/) |
| **2** | **Prov-GigaPath** | Providence / Microsoft | ViT-G (1.38B tiles) tile encoder + whole-slide LongNet architecture over tens of thousands of tiles | 5-Fold Cross-Validation across 18 pan-cancer subtyping tasks | **0.908** | **0.642** | [Nature 630, 476–483 (2024)](https://doi.org/10.1038/s41586-024-07441-w) |
| **3** | **UNI2-h + CLAM** | Mahmood Lab / Harvard | Custom ViT-H (681M params) trained on >200M tiles from >350k slides; CLAM slide-level aggregation | Standard held-out pan-cancer patient split | **0.904** | **0.639** | [arXiv:2410.02700](https://arxiv.org/abs/2410.02700) |
| **4** | **H-Optimus-1 + TransMIL** | Bioptimus | 1.1B parameter ViT trained on >1M WSIs from >800k patients; correlated transformer MIL aggregation | Standard held-out pan-cancer patient split | **0.898** | **0.635** | [H-Optimus-1 Card](https://huggingface.co/bioptimus/H-optimus-1) |
| **5** | **CTransPath + CLAM-SB** | Independent Baseline | Hybrid CNN-Swin Transformer (Swin-T, 28M params) supervised by contrastive learning; CLAM-SB head | 5-Fold Cross-Validation baseline comparator | **0.842** | **0.598** | [Medical Image Analysis 86 (2023)](https://doi.org/10.1016/j.media.2023.102759) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full diagnostic slide corpus: $>5\text{ TB}$ of compressed `.svs` files; uncompressed raw pixel grids reach hundreds of terabytes.
  - Storing pre-extracted tile embeddings (e.g., 768-d or 1024-d float16 vectors for all tiles across TCGA) requires approximately $150\text{--}300\text{ GB}$ of fast NVMe storage (HDF5 or zarr format).
  - Slide-level MIL inference requires **12--16 GB VRAM** (e.g., RTX 3080 / 4080); training whole-slide long-sequence models over $30{,}000$ tiles requires **48--80 GB VRAM** (A100/H100).
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install openslide-python Pillow
import openslide

# Open an SVS whole-slide image from TCGA
slide_path = "tcga_sample_diagnostic.svs"
slide = openslide.OpenSlide(slide_path)

# Inspect pyramidal structure and physical coordinate metadata
print(f"Dimensions at Level 0: {slide.dimensions}")
print(f"Pyramid Level Count:   {slide.level_count}")
print(f"Downsample Factors:    {slide.level_downsamples}")
print(f"Microns Per Pixel X:   {slide.properties.get(openslide.PROPERTY_NAME_MPP_X, 'N/A')}")

# Read a 512x512 tile at highest resolution (level 0)
tile = slide.read_region((10000, 10000), level=0, size=(512, 512)).convert("RGB")
print(f"Tile Shape: {tile.size}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Frozen Section vs. FFPE Incompatibility:* Cryosection slides (`tissue_slide`) contain extensive freeze-thaw retraction gaps, nuclear freezing distortion, and knife chatter. Models trained solely on FFPE (`diagnostic_slide`) drop $15\text{--}25\%$ AUROC on frozen sections.
  2. *Pen Marks, Air Bubbles & Tissue Folds:* Retrospective clinical slides frequently contain pathologist pen annotations, air pockets under coverslips, and folded tissue edges that trigger spurious high-confidence tile activations. Automated tissue mask generation (e.g., Otsu thresholding + edge detection) is mandatory.
  3. *Technical Confounders (Site Shortcuts):* Tissue Source Site (TSS) codes heavily correlate with slide staining recipes and patient survival. Classifiers easily exploit scanner/stain signatures rather than genuine neoplastic morphometry.

---

## 5. Downstream Foundation Model Consumers
The following foundation model systems utilize TCGA as a core pretraining, fine-tuning, or benchmark dataset:
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): Pretraining and whole-slide evaluation.
- **Virchow2 / Virchow** (`docs/02_models/04_pathology/virchow2.md`): Massive-scale evaluation benchmark.
- **UNI2-h / UNI** (`docs/02_models/04_pathology/uni2_h.md`): High-resolution transfer testing.
- **H-Optimus-1** (`docs/02_models/04_pathology/h_optimus_1.md`): Pan-cancer subtyping comparator.
- **TITAN** (`docs/02_models/04_pathology/titan.md`): Multimodal slide-level vision-language alignment.
- **PRISM2** (`docs/02_models/04_pathology/prism2.md`): Clinical slide diagnostic validation.
