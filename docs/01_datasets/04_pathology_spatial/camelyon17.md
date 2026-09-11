# CAMELYON16/17: Cancer Metastases in Lymph Nodes Challenge

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D27]`
- **Domain & Modality:** Brightfield Whole-Slide Imaging (WSI, H&E Formalin-Fixed Paraffin-Embedded [FFPE] sections of sentinel lymph nodes)
- **Target Anatomy & Pathologies:** Breast Cancer Sentinel Lymph Node Metastases (Isolated Tumor Cells [ITC], Micrometastases, Macrometastases) & Patient-Level Pathologic N-Stage (pN0, pN0(i+), pN1mi, pN1, pN2)
- **Release / Challenge Year:** 2016 (CAMELYON16, ISBI) & 2017 (CAMELYON17, ISBI); ongoing benchmark platform
- **Evidence Code:** `E1` (Peer-Reviewed Challenge Reports: Ehteshami Bejnordi et al. JAMA 2017; Bandi et al. IEEE TMI 2019) + `E2` (Official Grand Challenge Portal) + `E5A` (Independent WILDS Benchmark)
- **Access Level:** Fully Open Access under Creative Commons Zero (CC0 1.0 Universal, Public Domain Dedication)
- **Primary Source / Portal:** [CAMELYON17 Grand Challenge Portal](https://camelyon17.grand-challenge.org/) | [CAMELYON17 Data Access](https://camelyon17.grand-challenge.org/Data/) | [CAMELYON16 Portal](https://camelyon16.grand-challenge.org/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Sentinel lymph node biopsy is the standard surgical staging procedure for determining breast cancer regional spread. Histopathological examination of multiple nodal sections is highly labor-intensive and prone to fatigue-induced diagnostic errors, particularly for microscopic foci. The challenge evaluates automated systems on two levels: lesion-level metastasis localization and patient-level overall pathologic nodal staging (**pN-stage**).
- **Target Classes & Staging Hierarchy:**
  1. *Lesion Classes:*
     - Isolated Tumor Cells (ITC): Single tumor cells or clusters $\le 0.2\text{ mm}$ or $<200$ cells.
     - Micrometastases: Lesions $>0.2\text{ mm}$ and $\le 2.0\text{ mm}$.
     - Macrometastases: Large metastases $>2.0\text{ mm}$.
  2. *Patient pN-Stages (5-Class Ordinal Scale):*
     - `pN0`: No regional lymph node metastasis histologically, no ITCs.
     - `pN0(i+)`: Malignant cells $\le 0.2\text{ mm}$ (ITCs only).
     - `pN1mi`: Micrometastases only ($>0.2\text{ mm}$ to $\le 2.0\text{ mm}$).
     - `pN1`: Metastases in 1 to 3 axillary lymph nodes (at least one $>2.0\text{ mm}$).
     - `pN2`: Metastases in 4 to 9 axillary lymph nodes.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  $$\text{200 Patients (CAMELYON17)} \neq \text{1,000 Whole-Slide Images (WSIs)} \neq \text{5 Slides / Patient} \neq \text{Hundreds of Millions of Patches}$$
  *(CAMELYON16 contributes an additional 400 WSIs: 270 training slides and 130 held-out test slides).*
  - **CAMELYON17 Scale:** Exactly **1,000 whole-slide images** across **200 breast cancer patients** (exactly 5 lymph-node WSIs per patient).
  - **Training Cohort:** 100 patients (500 WSIs, 100 per medical center) with slide-level and patient-level stage ground truth, plus detailed boundary contour XMLs for 50 positive training slides.
  - **Held-out Blind Test Cohort:** 100 patients (500 WSIs, 100 per medical center) with labels withheld for competitive benchmarking.
- **Multi-Center Imaging Physics & Scanner Distribution:**
  Scanned across **5 Dutch medical centres** with distinct optical architectures and stain chemistries:
  - *Center 0:* Radboud University Medical Center (3DHistech Pannoramic Flash II 250, optical resolution $0.243\,\mu\text{m/pixel}$).
  - *Center 1:* Canisius-Wilhelmina Hospital (CWZ) (3DHistech Pannoramic Flash II 250, optical resolution $0.243\,\mu\text{m/pixel}$).
  - *Center 2:* University Medical Center Utrecht (Philips Ultra Fast Scanner, optical resolution $0.250\,\mu\text{m/pixel}$).
  - *Center 3:* Rijnstate Hospital (Philips Ultra Fast Scanner, optical resolution $0.250\,\mu\text{m/pixel}$).
  - *Center 4:* Laboratory of Pathology East Netherlands (LabPON) (Philips Ultra Fast Scanner, optical resolution $0.250\,\mu\text{m/pixel}$).

### [T] Truth & Annotation Provenance
- **Reference Standard:** Multi-observer expert consensus. Sentinel node sections were independently evaluated by multiple expert pathologists. Equivocal lesions and ITCs were resolved by immunohistochemistry (cytokeratin IHC staining on adjacent tissue serial sections) serving as biological gold truth.
- **Label Provenance Category:** Native human expert consensus backed by IHC ground truth.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via Grand Challenge, Academic Torrents, and Google Drive / AWS mirror links.
- **License / Terms:** Dedicated to the public domain under **Creative Commons Zero (CC0 1.0 Universal)**. Completely open for unrestricted commercial and academic use.
- **Artifact Availability:** Pyramidal multi-resolution TIFF files (`.tif`), ASAP XML contour coordinates, patient-level pN-stage CSVs, and official Grand Challenge evaluation Docker containers.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Succeeded CAMELYON16 (which focused on single-slide binary metastasis classification and FROC lesion localization).
- **Derivative Ecosystem:** Sourced as the foundational real-world distribution shift benchmark in WILDS (Koh et al., ICML 2021) and adopted as standard transfer testing ground for digital pathology foundation models (UNI, Virchow, Prov-GigaPath).
- **Contamination Warning:** The 270 CAMELYON16 training slides and 500 CAMELYON17 training slides are frequently used in self-supervised patch pretraining. Zero-shot or out-of-domain transfer claims must explicitly verify that the 500-slide CAMELYON17 blind test set was omitted from pretraining corpora.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official CAMELYON17 held-out blind test set (100 patients / 500 WSIs from 5 medical centers). The primary official evaluation metric is the 5-class **Quadratic Weighted Cohen's Kappa** ($k_w$) evaluated at the patient pN-stage level. Ties are resolved by calculating kappa on individual pN-stages in reverse order starting with pN2.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Patient pN-Stage Kappa ($k_w$) | Slide-Level AUROC | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Lunit Inc. Framework** | Lunit Inc. (Seoul) | Multi-resolution ResNet ensemble with false-positive hard negative mining and topological metastasis feature aggregation | Official Challenge Blind Test ($N=100$ pts / 500 WSIs) | **0.8993** *(Post-challenge: **0.9203**)* | **0.985** | [IEEE TMI 38(6)](https://doi.org/10.1109/TMI.2018.2884500) / [arXiv:1812.08587](https://arxiv.org/abs/1812.08587) |
| **2** | **HMS-MGH-CCDS** | Harvard / Mass General Hospital | Deep cascaded CNN with multi-scale probability heatmap extraction and lesion diameter thresholding | Official Challenge Blind Test ($N=100$ pts / 500 WSIs) | **0.8806** | **0.976** | [IEEE TMI 38(6)](https://doi.org/10.1109/TMI.2018.2884500) |
| **3** | **VCA-TUe** | Eindhoven Univ. of Technology | Cascaded convolutional networks with stain-color augmentation and morphological lesion geometry modeling | Official Challenge Blind Test ($N=100$ pts / 500 WSIs) | **0.8729** | **0.969** | [IEEE TMI 38(6)](https://doi.org/10.1109/TMI.2018.2884500) |
| **4** | **DIAG Baseline** | Radboud University Medical Center | Multi-scale DenseNet / U-Net with tissue-mask candidate extraction and rule-based pN-stage staging | Official Challenge Blind Test ($N=100$ pts / 500 WSIs) | **0.8415** | **0.952** | [IEEE TMI 38(6)](https://doi.org/10.1109/TMI.2018.2884500) |
| **5** | **UNI2-h / Virchow2 + Attn-MIL** | Academic FM Reference (Harvard / Paige) | ViT-H (681M / 632M params) tile embeddings pooled with multi-head attention MIL | Post-hoc independent evaluation on CAMELYON17 test set | **0.915--0.932** | **0.988** | [Nature Medicine 30 (2024)](https://doi.org/10.1038/s41591-024-02857-3) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full CAMELYON17 dataset size: $\approx 2.8\text{--}3.2\text{ TB}$ of pyramidal TIFF files (1,000 WSIs, each 1.5 to 4 GB).
  - CAMELYON16 dataset size: $\approx 700\text{--}800\text{ GB}$ (400 WSIs).
  - Tile feature extraction with high-throughput models (e.g., Virchow2, UNI) requires $\approx 8\text{--}12\text{ hours}$ on 4x RTX 4090 / A5000 GPUs.
  - Slide-to-patient aggregation operates comfortably within **16 GB VRAM**.
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install openslide-python Pillow
import openslide

# Open a CAMELYON17 pyramidal TIFF slide
slide_path = "patient_004_node_0.tif"
slide = openslide.OpenSlide(slide_path)

# Verify multi-resolution structure and physical coordinate system
print(f"Level 0 Dimensions: {slide.dimensions}")
print(f"Level Count:        {slide.level_count}")
print(f"Downsample Factors: {slide.level_downsamples}")
mpp = float(slide.properties.get(openslide.PROPERTY_NAME_MPP_X, 0.25))
print(f"Resolution:         {mpp:.4f} microns/pixel")

# Read a high-power field (HPF) 512x512 tile
tile = slide.read_region((50000, 50000), level=0, size=(512, 512)).convert("RGB")
print(f"Tile dimensions:    {tile.size}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Isolated Tumor Cells vs. Benign Mimickers:* ITCs ($\le 0.2\text{ mm}$) are indistinguishable from sinus histiocytes, endothelial cells, or activated dendritic cells on morphology alone without cytokeratin IHC confirmation. Models frequently suffer from false-positive ITC predictions.
  2. *Single-Tile Cascading Staging Error:* A patient's pN-stage is determined by the maximum metastasis diameter across all 5 slides. A single false-positive macroscopic tile prediction will erroneously upgrade a healthy `pN0` patient to `pN1` or `pN2`.
  3. *Optical & Staining Inter-Center Shifts:* Clear optical shift exists between 3DHistech (Center 0/1) and Philips (Center 2/3/4) scanners, alongside institutional variation in hematoxylin counterstaining intensity. Cross-center validation drops sharply without stain normalization or color jitter.

---

## 5. Downstream Foundation Model Consumers
The following foundation model systems utilize CAMELYON16/17 as a primary transfer learning, out-of-domain robustness, or linear probing benchmark:
- **UNI / UNI2-h** (`docs/02_models/04_pathology/uni2_h.md`): Benchmark for zero-shot and linear-probing metastasis detection.
- **Virchow2 / Virchow** (`docs/02_models/04_pathology/virchow2.md`): Slide-level classification and external center transfer.
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): LongNet tile-to-slide aggregation benchmark.
- **TITAN** (`docs/02_models/04_pathology/titan.md`): Slide-level metastasis detection and reporting.
- **PRISM2** (`docs/02_models/04_pathology/prism2.md`): Breast lymph node metastasis clinical staging evaluation.
- **CRISP** (`docs/02_models/04_pathology/crisp.md`): Rapid intraoperative margin and nodal metastasis validation.
