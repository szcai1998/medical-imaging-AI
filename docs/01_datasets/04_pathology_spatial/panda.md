# PANDA: Prostate cANcer graDe Assessment Challenge

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D28]`
- **Domain & Modality:** Brightfield Whole-Slide Imaging (WSI, H&E Formalin-Fixed Paraffin-Embedded [FFPE] Core Needle Biopsies)
- **Target Anatomy & Pathologies:** Prostate Adenocarcinoma Diagnosis, Gleason Scoring (Patterns 3, 4, 5), and International Society of Urological Pathology (ISUP) Grade Groups (Grades 0 to 5)
- **Release / Challenge Year:** 2020 (Kaggle Competition / MICCAI 2020) & 2022 (Consortium Landmark Paper: Bulten et al., *Nature Medicine* 28, 72–81, 2022)
- **Evidence Code:** `E1` (Consortium Nature Medicine Paper) + `E2` (Official Kaggle Challenge Portal) + `E5A` (Independent Pathology Foundation Model Benchmarks)
- **Access Level:** Gated Challenge & Academic Research Access (Kaggle Competition terms; non-commercial academic research use)
- **Primary Source / Portal:** [Kaggle PANDA Challenge](https://www.kaggle.com/c/prostate-cancer-grade-assessment) | [Computational Pathology Group Nijmegen PANDA](https://www.computationalpathologygroup.eu/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Prostate cancer is among the most diagnosed malignancies worldwide. Grading core needle biopsy tissue using the Gleason grading system (and modern ISUP Grade Groups 0–5) dictates patient management (ranging from active surveillance to radical prostatectomy, radiation, or androgen deprivation therapy). However, manual Gleason grading suffers from profound inter-observer and intra-observer discordance ($30\text{--}50\%$ disagreement between pathologists). The objective of PANDA is automated, reproducible ISUP grade group assignment.
- **Target Classes & Grading Scale:**
  - `ISUP Grade 0`: Non-neoplastic / benign tissue.
  - `ISUP Grade 1`: Gleason score $3+3=6$.
  - `ISUP Grade 2`: Gleason score $3+4=7$ (predominantly well-formed glands with minor cribriform/poorly formed components).
  - `ISUP Grade 3`: Gleason score $4+3=7$ (predominantly cribriform/poorly formed glands).
  - `ISUP Grade 4`: Gleason score $4+4=8$, $3+5=8$, or $5+3=8$.
  - `ISUP Grade 5`: Gleason score $4+5=9$, $5+4=9$, or $5+5=10$ (solid sheets, comedonecrosis, or single infiltrating cells).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  $$\text{10,616 Open Development WSIs} + \text{1,000+ Blind Test WSIs} \neq \text{Individual Biopsy Cores} \neq \text{Extracted Patches/Tiles}$$
  - **10,616 digitized prostate biopsy whole-slide images** in the public development corpus.
  - **1,000+ hidden evaluation slides** on the Kaggle private test set and independent external clinical validation cohorts.
- **Multi-Center Imaging Physics & Optical Discrepancies:**
  Sourced from two major European academic medical institutions with intentionally contrasting hardware and optical specifications:
  - *Radboud University Medical Center (Nijmegen, Netherlands):* 5,160 biopsy slides scanned on a **3DHistech Pannoramic Flash II 250** scanner at high optical resolution ($0.24\,\mu\text{m/pixel}$).
  - *Karolinska Institute (Stockholm, Sweden):* 5,456 biopsy slides scanned on a **Hamamatsu NanoZoomer** scanner at standard optical resolution ($0.46\,\mu\text{m/pixel}$).
  - *Extreme Spatial Sparsity:* Core needle biopsies represent narrow, elongated tissue strips occupying only $2\text{--}5\%$ of the total glass slide area, leaving $>95\%$ empty white background.

### [T] Truth & Annotation Provenance
- **Reference Standard & Provenance Asymmetry:**
  - *Radboud UMC Sub-Cohort:* Ground truth established by an expert international panel of subspecialist uropathologists, accompanied by semi-automated pixel masks delineating benign glands, Gleason pattern 3, Gleason pattern 4, and Gleason pattern 5.
  - *Karolinska Institute Sub-Cohort:* Labels sourced directly from routine historical electronic health records (single-pathologist diagnosis per case), introducing substantial real-world clinical label noise.
- **Label Provenance Category:** Mixed expert panel consensus (Radboud) and routine clinical EHR labels (Karolinska).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via the Kaggle API (`kaggle competitions download -c prostate-cancer-grade-assessment`).
- **License / Terms:** Kaggle Competition License restricting usage to non-commercial academic research and algorithmic development.
- **Artifact Availability:** Multi-resolution pyramidal TIFF files, train/test metadata CSVs, and pixel-level Gleason mask files.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Expanded from the Nijmegen Gleason 2019 MICCAI challenge.
- **Derivative Ecosystem:** Standard evaluation benchmark for multiple instance learning (MIL) and foundation model transfer in computational uropathology.
- **Contamination Warning:** The 10,616 public development slides are frequently incorporated into open pathology pretraining datasets. Model developers claiming out-of-domain transfer on PANDA must strictly verify whether their pretraining corpus absorbed the Kaggle development set.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official Kaggle PANDA Private Test Leaderboard ($N > 1,000$ hidden biopsy cases). Evaluated using the official challenge metric: **Quadratic Weighted Cohen's Kappa** ($k_w$) across the 6-class ISUP Grade Group ordinal scale (Grades 0 to 5).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Private Test Kappa ($k_w$) | Public Test Kappa | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Team PND** | Kentaroy47 & yukkyo (Japan) | Multi-model EfficientNet ensemble with out-of-fold label noise filtering (discarding noisy Karolinska labels) | Official Hidden Private Test ($N > 1,000$) | **0.9409** | **0.9125** | [Nature Medicine 28](https://doi.org/10.1038/s41591-021-01620-2) / [Kaggle Solution](https://www.kaggle.com/c/prostate-cancer-grade-assessment/discussion) |
| **2** | **Save The Prostate** | deepflash2 (Germany) | Compact tile-grid concatenation ($4 \times 4$ or $6 \times 6$ patches) + EfficientNet-B0/B1 with ordinal loss | Official Hidden Private Test ($N > 1,000$) | **0.9377** | **0.9102** | [Nature Medicine 28](https://doi.org/10.1038/s41591-021-01620-2) / [Grand Challenge](https://grand-challenge.org/) |
| **3** | **rähmä.ai / iafoss** | Independent (Finland) | Adaptive tissue-patch extraction (36-patch grid) + ResNeXt-50/EfficientNet with heavy stain jitter | Official Hidden Private Test ($N > 1,000$) | **0.9351** | **0.9084** | [Nature Medicine 28](https://doi.org/10.1038/s41591-021-01620-2) |
| **4** | **NS Pathology** | Independent Consortium Team | Hierarchical multi-resolution feature aggregation with rank-consistent ordinal regression (CORAL) | Official Hidden Private Test ($N > 1,000$) | **0.9312** | **0.9045** | [Nature Medicine 28](https://doi.org/10.1038/s41591-021-01620-2) |
| **5** | **Pathology FM (Prov-GigaPath / UNI)** | Academic Foundation Models | Frozen ViT tile representations pooled via linear probing / Attention-MIL | Standard test split evaluation | **0.928--0.934** | **--** | [Nature 630 (2024)](https://doi.org/10.1038/s41586-024-07441-w) |

*(Note: In the Bulten et al. Nature Medicine 2022 landmark analysis, the top algorithmic systems achieved $k_w = 0.862\text{--}0.93$ across independent external validation cohorts from the United States and Europe, successfully matching subspecialist uropathologists who achieved $k_w = 0.91$ and outperforming general pathologists).*

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full PANDA training set size: $\approx 420\text{--}450\text{ GB}$ of TIFF files.
  - Because needle biopsies have very small tissue footprints ($2\text{--}5\%$ of the slide), extracting and packing the top 16 to 36 tissue tiles into a single composite mosaic ($1024 \times 1024$ or $1536 \times 1536$ pixels) allows training standard 2D CNNs/ViTs on consumer GPUs (**8--12 GB VRAM**, e.g., RTX 3070 / 4070) without requiring full MIL graph infrastructure.
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install openslide-python Pillow numpy
import openslide
import numpy as np

# Open a PANDA biopsy TIFF slide
slide_path = "0005f7aa033e16e1ec7ad5c4da4a279e.tiff"
slide = openslide.OpenSlide(slide_path)

print(f"Slide Dimensions: {slide.dimensions}")
print(f"Level Count:      {slide.level_count}")
print(f"Downsamples:      {slide.level_downsamples}")

# Read thumbnail to detect biopsy tissue regions (discarding >95% white background)
thumb = slide.get_thumbnail((512, 512)).convert("L")
thumb_np = np.array(thumb)
tissue_mask = thumb_np < 220  # Threshold out white glass
print(f"Tissue Area Fraction: {tissue_mask.mean() * 100:.2f}%")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Gleason 3 vs. Gleason 4 Boundary:* Tangentially sectioned benign glands or crowded, well-formed Gleason pattern 3 glands mimic poorly formed or fused Gleason pattern 4 glands, creating the primary source of algorithm-pathologist discordance.
  2. *Scanner Optical Mismatch ($0.24\,\mu\text{m}$ vs. $0.46\,\mu\text{m}$):* Radboud slides have twice the linear pixel density of Karolinska slides. Algorithms that process fixed pixel patch sizes without physical spacing normalization ($0.5\,\mu\text{m/pixel}$) fail catastrophically across sites.
  3. *Routine Clinical Label Noise:* Routine EHR pathology reports (Karolinska) contain high noise due to subjective grading cutoffs. Top challenge models filtered out or down-weighted high-loss cases during training.

---

## 5. Downstream Foundation Model Consumers
The following foundation model systems benchmark on PANDA for prostate cancer grading and ordinal regression:
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): Prostate biopsy subtyping benchmark.
- **Virchow2 / Virchow** (`docs/02_models/04_pathology/virchow2.md`): Gleason grading and multi-center robustness.
- **UNI2-h / UNI** (`docs/02_models/04_pathology/uni2_h.md`): External cancer biopsy grading validation.
- **TITAN** (`docs/02_models/04_pathology/titan.md`): Vision-language alignment on prostate pathology.
- **PRISM2** (`docs/02_models/04_pathology/prism2.md`): Prostate biopsy diagnostic validation.
