# GigaTIME: Paired H&E to Virtual Multiplex Immunofluorescence Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D29]`
- **Domain & Modality:** Cross-Modal Virtual Spatial Biology: Brightfield H&E $\longrightarrow$ Virtual Multiplex Immunofluorescence (mIF, 21 protein channels)
- **Target Anatomy & Pathologies:** Pan-Cancer Tumor Immune Microenvironment (TIME) across 24 primary cancer types and 306 histological subtypes
- **Release / Milestone Year:** 2026 (Xu et al., *Cell*, Vol 189, Issue 2, pp. 433–452, 22 Jan 2026; DOI `10.1016/j.cell.2025.11.016`)
- **Evidence Code:** `E1` (Peer-Reviewed *Cell* Paper `[S32]`) + `E2` (Official GitHub Code/Data Repository `[D29]`)
- **Access Level:** Asymmetric Hybrid Open/Closed (Model weights & inference code are Open Source; Benchmark evaluation release comprises **50 paired H&E/mIF test patches**; the 14,256-patient pretraining corpus is **Closed / Proprietary Clinical Data**)
- **Primary Source / Portal:** [Providence/Microsoft GigaTIME Repository](https://github.com/prov-gigatime/GigaTIME) | [Cell Paper DOI 10.1016/j.cell.2025.11.016](https://doi.org/10.1016/j.cell.2025.11.016)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Multiplex immunofluorescence (mIF) stains specific immune cell subsets and immune checkpoints (e.g., CD3, CD4, CD8, CD20, CD68, FOXP3, PD-1, PD-L1, Pan-CK) to resolve the spatial organization of the tumor immune microenvironment, a critical determinant of immunotherapy efficacy. However, physical mIF is cost-prohibitive ($>\$1{,}500\text{--}\$3{,}000$ per slide), destroys tissue sections, and demands specialized multispectral imaging hardware. GigaTIME addresses this barrier via deep cross-modal generative translation, synthesizing 21-channel virtual mIF panels directly from routine, ubiquitous H&E histology.
- **Target Channels & Prediction Tasks:**
  1. *Virtual 21-Channel Multiplex Synthesis:* Generating high-resolution fluorescent intensity maps for 21 key immune and structural markers directly from an H&E input patch.
  2. *Single-Cell Marker Quantification:* Predicting single-cell mean fluorescence intensity (MFI) and cell-phenotype proportions (e.g., cytotoxic T-cell infiltration, Treg density, macrophage polarization).
  3. *Spatial Microenvironment Mapping:* Reconstructing tumor-immune spatial proximity, cell-cell contact networks, and tertiary lymphoid structures (TLS).

### [A] Acquisition Physics & Scale
- **Exact Counting Units (The Critical Openness Asymmetry):**
  $$\text{50 Public Paired Test Patches} \ll \text{14,256 Proprietary Patients} \neq \text{299,376 Virtual Slides} \neq \text{40 Million Cells} \neq \text{21 Channels}$$
  - **Public Benchmark Artifact:** Exactly **50 paired test patches** ($1024 \times 1024$ and $2048 \times 2048$ pixels at $20\times$ magnification) with co-registered H&E and physical 21-channel ground-truth mIF imaging.
  - **Closed Pretraining Corpus:** 14,256 cancer patients from 51 hospitals and >1,000 clinics within the Providence Health System. The model generated 299,376 whole-slide virtual mIF images across 24 cancer types.
- **Biochemical & Optical Acquisition Protocol:**
  - Physical multiplex immunofluorescence acquired using the **Akoya PhenoImager** (formerly Vectra Polaris) platform with Opal 7-color / multispectral fluorophores across consecutive rounds of antibody stripping and restaining.
  - Paired H&E imaging obtained by destaining the tissue and performing standard hematoxylin & eosin counterstaining on the identical physical section (or consecutive $4\,\mu\text{m}$ serial sections followed by non-rigid elastic registration).

### [T] Truth & Annotation Provenance
- **Reference Standard:** Measured physical fluorescence intensity from validated primary antibodies against 21 proteins, followed by spectral unmixing and background autofluorescence subtraction. Single-cell boundaries segmented via DeepCell/Mesmer and StarDist algorithms.
- **Label Provenance Category:** Native physical biochemical ground truth (physical antibody binding).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Public code and sample evaluation data hosted on GitHub (`prov-gigatime/GigaTIME`).
- **License / Terms:** Model code released under MIT/Apache terms; pretrained model checkpoints and sample test data governed by non-commercial research licensing.
- **Artifact Availability:** Inference Python scripts, model weights (`.pt`), and multi-channel TIFF test patches.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Jointly developed by Providence Health & Services and Microsoft Research as an extension of the Prov-GigaPath computational pathology program.
- **Contrasting Spatial Biology Paradigms (Ground-Layer Red-Team Rule):**
  - **GigaTIME:** Morphology (H&E) $\longrightarrow$ Inferred virtual multiplex protein signal (generative cross-modal prediction).
  - **VirTues (*Nature* 2026 `[S60]`):** Direct foundation modeling of *measured* multi-scale spatial proteomics data across proteins, cells, niches, and tissues.
  - *Epistemic Lesson:* **Virtual staining is morphological estimation, NOT measured physical reality.** A model predicts what markers *typically* correlate with a given nuclear or cytoplasmic texture; it cannot substitute for physical assays when identifying novel biologically decoupled phenotypes or rare resistance mutations.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the paired H&E to 21-channel mIF translation benchmark reported in Xu et al., Cell 189(2), 2026 [S32]. Primary evaluation metrics: Single-Cell Protein Expression Pearson Correlation Coefficient (PCC), Structural Similarity Index (SSIM), Peak Signal-to-Noise Ratio (PSNR in dB), and Mean Absolute Error (MAE).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Mean Cell PCC | Image SSIM | PSNR (dB) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **GigaTIME** | Providence / Microsoft | Multi-scale conditional latent diffusion + cross-modal transformer conditioned on GigaPath tile embeddings | Paired Multi-Center Test Patches ($N=50$ patches, 21 channels) | **0.784** | **0.762** | **24.8** | [*Cell* 189(2), 2026](https://doi.org/10.1016/j.cell.2025.11.016) / [Code](https://github.com/prov-gigatime/GigaTIME) |
| **2** | **pix2pixHD** | NVIDIA Baseline | Multi-scale conditional GAN with coarse-to-fine generator and VGG perceptual loss | Paired Multi-Center Test Split | **0.621** | **0.684** | **21.5** | [*Cell* 189(2), 2026](https://doi.org/10.1016/j.cell.2025.11.016) |
| **3** | **Pix2Pix** | Isola et al. (Standard) | U-Net generator with PatchGAN discriminator and combined $L_1$ + adversarial loss | Paired Multi-Center Test Split | **0.512** | **0.618** | **19.4** | [*Cell* 189(2), 2026](https://doi.org/10.1016/j.cell.2025.11.016) |
| **4** | **CycleGAN** | Zhu et al. (Unpaired) | Dual-generator cycle-consistent adversarial network (unpaired translation baseline) | Multi-Center Test Split | **0.485** | **0.591** | **18.9** | [*Cell* 189(2), 2026](https://doi.org/10.1016/j.cell.2025.11.016) |
| **5** | **Linear Morphology Baseline** | Standard Baseline | Classical cell morphology features (area, circularity, intensity) + Ridge regression | Paired Multi-Center Test Split | **0.380** | **--** | **--** | [*Cell* 189(2), 2026](https://doi.org/10.1016/j.cell.2025.11.016) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Public test repository size: $\approx 2.5\text{ GB}$ containing the 50 multi-channel TIFF test patches and precomputed segmentations.
  - Generating 21-channel virtual mIF patches using the full GigaTIME diffusion pipeline requires **24 GB VRAM** (e.g., RTX 3090 / 4090 / A5000) due to memory requirements during multi-step iterative denoising over 21 feature channels.
  - GigaTIME-Flash (`[S33]`) uses knowledge distillation to reduce per-tile inference latency from $\sim 4.2\text{ s}$ to $<0.3\text{ s}$.
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install tifffile numpy Pillow
import tifffile as tiff
import numpy as np

# Open a sample paired GigaTIME public evaluation patch
# Channel 0-2: RGB H&E; Channels 3-23: 21-channel mIF multiplex intensity
data_path = "sample_he_mif_paired_patch.tiff"
paired_data = tiff.imread(data_path)

print(f"Paired Tensor Shape: {paired_data.shape}")  # e.g., (24, 1024, 1024)
he_image = paired_data[0:3, :, :].transpose(1, 2, 0)
mif_channels = paired_data[3:, :, :]

print(f"H&E Resolution:      {he_image.shape}")
print(f"mIF Protein Count:   {mif_channels.shape[0]} channels")
print(f"CD8 Channel Range:   min={mif_channels[0].min()}, max={mif_channels[0].max()}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Immunological Hallucination:* GigaTIME synthesizes CD8+ or PD-1+ signals based on lymphocyte morphology. In inflamed or scarred stroma with morphologically identical non-functional or exhausted immune cells, the model frequently generates false-positive cytotoxic signals.
  2. *Channel Covariance & Leakage:* Highly correlated biological markers in the training cohort (e.g., CD3 and CD4) suffer from synthetic covariance collapse, where the generator blindly co-expresses both markers even in biological anomalies where one is downregulated.
  3. *The Data Asymmetry Bottleneck:* Because the 14,256-patient pretraining dataset remains closed, academic researchers cannot retrain or adapt GigaTIME to new hospital scanners, customized antibody panels, or non-Providence staining protocols.

---

## 5. Downstream Foundation Model Consumers
The following systems connect directly to the GigaTIME spatial biology benchmark:
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): Upstream tile encoder providing morphological embeddings for GigaTIME conditioning.
- **VirTues** (`[S60]`): Foundation model for *measured* spatial proteomics imaging (the physical spatial biology counterweight to virtual staining).
- **CARE** (`[S90]`): Cross-modal molecularly guided pathology aggregation framework.
