# AbdomenAtlas: Multi-Center Detailed-Annotated CT Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D4]`
- **Domain & Modality:** 3D Contrast and Non-Contrast Abdominal Computed Tomography (CT)
- **Target Anatomy:** 22 Detailed Abdominal Organs and Vascular Structures
- **Release / Publication:** 2024 (Medical Image Analysis 97:103285) | Continuous extensions (2.0/3.0)
- **Access Level:** Open Multicenter Research Dataset via Hugging Face & GitHub
- **Primary Source / Portal:** [MedIA Paper](https://doi.org/10.1016/j.media.2024.103285) | [GitHub: AbdomenAtlas](https://github.com/MrGiovanni/AbdomenAtlas) | [PubMed: 39116766](https://pubmed.ncbi.nlm.nih.gov/39116766/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Generalization failure in medical image segmentation is primarily caused by narrow, single-hospital training datasets. AbdomenAtlas solves this by creating a massive, diverse multi-center benchmark to evaluate transfer learning, domain generalization, and zero-shot organ segmentation across diverse clinical acquisition settings.
- **Target Classes:** 22 anatomical targets including parenchymal organs (liver, spleen, kidneys, pancreas), gastrointestinal tract (stomach, duodenum, colon), and retroperitoneal vasculature (aorta, inferior vena cava, portal veins).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **20,460 3D CT examination volumes**
  - Sourced from **112 independent hospitals** worldwide
  - **673,000 individual anatomical segmentation masks**
- **Imaging Physics & Acquisition Protocols:**
  - Represents real-world hospital heterogeneity: contrast-enhanced (arterial, venous, delayed) and non-contrast CTs.
  - Diverse slice thicknesses ($0.5\text{ mm}$ to $5.0\text{ mm}$), varied reconstruction filters (soft tissue, bone kernels), and multi-vendor scanners.
- **Multi-Center Distribution:** True multinational distribution across North America, Europe, Asia, and Australia.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - **5,246 CT scans** feature fully manual annotations by medical professionals.
  - The remaining ~15,200 scans were labeled via an active-learning human-in-the-loop pipeline: high-confidence deep learning predictions were interactively reviewed, refined, and verified by board-certified radiologists.
- **Label Provenance Category:** Hybrid: large human-manual subset ($>5\text{k}$ scans) + radiologist-corrected semi-automatic pipeline.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted via Hugging Face and dedicated institutional servers.
- **License / Terms:** Open research license for academic and scientific evaluation. Check specific data-use terms for commercial derivation.
- **Artifact Availability:** Voxel masks in NIfTI format, train/val/test multi-center split files, and baseline training scripts.

### [S] Systemic Lineage & Genealogy
- **Parent / Successor:** Predecessor to AbdomenAtlas 3.0 `[D5]` (which introduced RadGPT-generated report triplets).
- **Contamination Alert:** Due to its open availability and massive scale ($>20\text{k}$ CTs), AbdomenAtlas is a primary pretraining corpus for contemporary foundation models (e.g., VISTA3D, BiomedParse). Zero-shot evaluations must ensure testing is conducted on strictly held-out hospital centers.

---

## 3. Verified SOTA Benchmarks & Transfer Rankings
*Performance evaluated across the 22 abdominal structures on multi-center external test hospitals, measuring out-of-domain transfer via mean Dice Similarity Coefficient (DSC) and Normalized Surface Distance (NSD).*

| Rank | Model / Framework | Backbone Architecture | In-Domain Mean DSC | Multi-Center Transfer DSC | HD95 Boundary Error | Reference |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **ResEnc-L nnU-Net v2** | 3D Residual Encoder nnU-Net (Large) | **0.918** | **0.887** | **3.82 mm** | [MedIA 2024](https://doi.org/10.1016/j.media.2024.103285) |
| **2** | **VISTA3D / NV-Segment-CT** | MONAI Foundation Model (SegResNet) | **0.912** | **0.881** | **4.05 mm** | [CVPR 2025](https://github.com/Project-MONAI/VISTA) |
| **3** | **SwinUNETR (AbdomenAtlas Pretrained)** | Swin Transformer Encoder | **0.904** | **0.873** | **4.28 mm** | [MedIA 2024](https://doi.org/10.1016/j.media.2024.103285) |
| **4** | **Standard nnU-Net Baseline** | Vanilla 3D fullres U-Net | **0.898** | **0.865** | **4.56 mm** | [Nature Methods 18](https://doi.org/10.1038/s41592-020-01008-z) |
| **5** | **MedSAM (Zero-Shot Box Prompt)** | ViT-B Segment Anything Backbone | **0.871** | **0.842** | **5.91 mm** | [Nature Comms 15](https://doi.org/10.1038/s41467-024-44824-z) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - The complete 20k volume dataset exceeds **1.5 TB**.
  - Researchers should utilize the official 5k manual subset (~300 GB) for workstation-level training; full-scale pretraining requires multi-node A100/H100 infrastructure.
- **Minimal Local Verification / Load Command:**
  ```python
  import nibabel as nib
  ct = nib.load("AbdomenAtlas_1.0/images/BDMAP_0000001.nii.gz")
  mask = nib.load("AbdomenAtlas_1.0/masks/BDMAP_0000001.nii.gz")
  print(f"Hospital Center: BDMAP, Voxel dimensions: {ct.shape}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Counting Unit Fallacy:* "673K masks" refers to the sum of individual organ binary masks across all volumes, not 673,000 CT scans.
  2. *Vascular Inconsistency:* Segmenting small mesenteric vessels (celiac trunk, superior mesenteric artery) remains challenging without arterial phase timing.
