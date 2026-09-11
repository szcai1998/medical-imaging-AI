# PathoROB External Multi-Center Patch Suite

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[S29]` (PathoROB Robustness Benchmark) & `[S28]` (PathBench 32-Model Comparative Benchmark)
- **Domain & Modality:** Multi-Center Brightfield Histopathology Patch Suites (H&E Formalin-Fixed Paraffin-Embedded [FFPE] 2D Regions of Interest [ROIs] and Image Patches)
- **Target Anatomy & Pathologies:** Multi-organ histological classification, lesion subtyping, tissue composition, and technical confound robustness across 8 public clinical datasets
- **Release / Milestone Year:** 2026 (Landmark Studies: Kömen et al., *Nature Communications* 17, 5218, June 2026 `[S29]`; Bareja et al., *Nature Communications* 17, 9012, July 2026 `[S28]`)
- **Evidence Code:** `E1` (Peer-Reviewed *Nature Communications* Benchmark Papers) + `E5A` (Standardized Independent Out-of-Distribution Evaluation Suite)
- **Access Level:** Fully Open Access across public heterogeneous source repositories (Zenodo, Grand Challenge, GitHub, Kaggle)
- **Primary Source / Portal:** [Nature Comms 17, 5218 (2026)](https://doi.org/10.1038/s41467-026-73923-2) | [PathBench Portal](https://pathbench.stanford.edu/) | [Nature Comms 17, 9012 (2026)](https://doi.org/10.1038/s41467-026-76004-6)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Standard computational pathology benchmarks evaluate foundation models solely on in-domain classification accuracy (AUROC or F1). This creates a dangerous blind spot: deep feature representations frequently learn **technical shortcuts** (exploiting hospital-specific hematoxylin staining recipes, scanner sensor profiles, or illumination artifacts) rather than true biological morphology. The PathoROB suite was designed to explicitly measure both biological diagnostic capability and invariance to non-biological technical confounds.
- **Constituent Datasets & Clinical Scopes (8-Benchmark Suite):**
  1. *BRACS (BReAst Carcinoma Subtyping):* 4,391 patches across 7 lesion categories (Normal, Benign, Pathological Benign, UDH, ADH, FEA, DCIS, Invasive Carcinoma) from 3 clinical centers.
  2. *BACH (BreAst Cancer Histology):* 400 high-resolution images across 4 diagnostic classes (Normal, Benign, In Situ, Invasive Carcinoma) from 4 medical centers.
  3. *UnitoPatho:* 9,536 colorectal polyp patches across 6 histological grades (HP, TA low-grade, TA high-grade, TVA low-grade, TVA high-grade, normal) from the University of Turin.
  4. *SICAPv2:* 10,861 prostate biopsy patches annotated with Gleason patterns (Non-cancerous, Gleason 3, Gleason 4, Gleason 5).
  5. *BreakHis:* 7,909 breast tumor microscopic images across 4 optical magnifications ($40\times, 100\times, 200\times, 400\times$) from 82 patients.
  6. *LC25000:* 25,000 histopathological images across 5 balanced classes ($5 \times 5{,}000$: colon adenocarcinoma, benign colon, lung adenocarcinoma, lung squamous cell carcinoma, benign lung).
  7. *MHIST:* 7,096 colorectal polyp patches (binary classification: hyperplastic polyp vs. sessile serrated adenoma) from Dartmouth-Hitchcock Medical Center.
  8. *NCT-CRC-HE-100K:* 100,000 non-overlapping H&E patches from colorectal cancer across 9 tissue classes (Adipose, Background, Debris, Lymphocytes, Mucus, Smooth Muscle, Normal Mucosa, Cancer-Associated Stroma, Colorectal Adenocarcinoma Epithelium) from NCT Heidelberg and UMM Mannheim.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  $$\text{>165,000 Curated Patches} \neq \text{34 Medical Centers} \neq \text{Hundreds of Patients} \neq \text{Multiple Optical Magnifications}$$
  - **Scale:** Over **165,000 non-overlapping image patches** across 8 distinct clinical benchmark corpora.
  - **Multi-Center Distribution:** Sourced across **34 international medical centers** spanning Europe, North America, and Asia.
  - **Scanner & Hardware Heterogeneity:** Scanned across Aperio ScanScope, Hamamatsu NanoZoomer, Leica SCN400, Philips Ultra Fast, and Zeiss Axio Imager systems. Patch sizes vary from $224 \times 224$ to $512 \times 512$ pixels, primarily at $20\times$ ($0.50\,\mu\text{m/px}$) and $40\times$ ($0.25\,\mu\text{m/px}$).

### [T] Truth & Annotation Provenance
- **Reference Standard:** Consensus histopathological grading and tissue phenotyping performed by board-certified anatomical pathologists and subspecialist oncologic pathologists from the respective originating medical institutions.
- **Label Provenance Category:** Native human expert consensus.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Distributed across original verified open-access archives:
  - NCT-CRC-HE-100K: Zenodo (`10.5281/zenodo.1214456`).
  - MHIST: Official repository (`github.com/cgc621/MHIST`).
  - BRACS: Grand Challenge (`bracs.grand-challenge.org`).
  - BACH: Grand Challenge (`iciar2018-challenge.grand-challenge.org`).
  - SICAPv2: Mendeley Data (`10.17632/9xxrpm42ts.1`).
  - BreakHis: Web portal (`web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/`).
  - LC25000: Kaggle / Academic repository.
- **License / Terms:** Open access under various Creative Commons terms (CC BY 4.0, CC BY-NC-SA 4.0) allowing non-commercial research use.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Independent clinical research collections unified into standardized evaluation suites by the PathoROB consortium (`[S29]`) and the PathBench platform (`[S28]`).
- **The Suite Doctrine (MANDATORY):**
  - **Use as a composite suite, not as isolated single-dataset proof of generalization.** High accuracy on one patch dataset (e.g., NCT-CRC-HE-100K) does *not* prove out-of-distribution robustness.
  - **The Robustness Reality (`[S29]`):** Digital pathology foundation models strongly encode non-biological technical features. In Kömen et al. (2026), medical center identity could be predicted from frozen foundation model embeddings with up to **$95\%$ accuracy**.
  - A leaderboard based solely on task AUROC is methodologically incomplete; **robustness to technical confounds must be reported alongside biological accuracy.**

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings under the standardized PathoROB multi-center robustness protocol (Kömen et al., Nature Communications 17, 5218, 2026 [S29]). Evaluated on: (1) Biological Task Mean AUROC across the patch suite, (2) Medical Center Confound AUROC (measures how strongly embeddings encode acquisition site; lower is better / more invariant), and (3) PathoROB Robustness Index ($R_{\text{idx}}$, harmonic balance of biological capability and technical invariance).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Pretraining Scale | Biological Task Mean AUROC | Center Confound AUROC ($\downarrow$) | PathoROB Robustness Index ($R_{\text{idx}} \uparrow$) | Reference / Links |
| :---: | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **UNI2-h** | Mahmood Lab / Harvard | Custom ViT-H (681M params) trained on >200M tiles from >350k slides | **0.914** | **0.612** | **0.842** | [arXiv:2410.02700](https://arxiv.org/abs/2410.02700) / [Nat Commun 17, 5218](https://doi.org/10.1038/s41467-026-73923-2) |
| **2** | **Virchow2** | Paige AI / MSKCC | ViT-H (632M params) trained on 3.1M WSIs with mixed magnification | **0.919** | **0.648** | **0.838** | [arXiv:2408.00738](https://arxiv.org/abs/2408.00738) / [Nat Commun 17, 5218](https://doi.org/10.1038/s41467-026-73923-2) |
| **3** | **Prov-GigaPath** | Providence / Microsoft | ViT-G (1.38B tiles from 171,189 slides across 31 tissue types) | **0.910** | **0.665** | **0.825** | [Nature 630, 476–483 (2024)](https://doi.org/10.1038/s41586-024-07441-w) |
| **4** | **CONCH / TITAN** | Mahmood Lab / Harvard | ViT-B (CoOp visual-language pretraining on 1.17M histology image-caption pairs) | **0.898** | **0.672** | **0.816** | [Nature Medicine 30 (2024)](https://doi.org/10.1038/s41591-024-02856-4) |
| **5** | **Phikon-v2** | Owkin | ViT-L (307M params) trained via masked image modeling (iBOT) on 400k WSIs | **0.882** | **0.710** | **0.801** | [arXiv:2409.09173](https://arxiv.org/abs/2409.09173) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full suite storage: $\approx 25\text{--}35\text{ GB}$ total for all 8 constituent patch datasets (compact PNG/JPEG format).
  - Because all images are pre-extracted patches ($224 \times 224$ to $512 \times 512$), feature extraction and linear-probe benchmarking can be executed entirely on consumer workstations with **8--12 GB VRAM** (e.g., RTX 3070 / 4060 Ti / Apple M-series), requiring no high-RAM whole-slide pyramid infrastructure.
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install Pillow torchvision numpy
from PIL import Image
import numpy as np
import glob
import os

# Load a sample patch from the NCT-CRC-HE-100K dataset
patch_files = glob.glob("NCT-CRC-HE-100K/*/*.tif") or ["sample_crc_patch.tif"]
img = Image.open(patch_files[0]).convert("RGB")
arr = np.array(img)

print(f"Patch Resolution: {img.size}")
print(f"Color Channels:   {arr.shape[-1]}")
# Inspect staining signature (mean R, G, B intensity) to check for center stain shift
mean_rgb = arr.mean(axis=(0, 1))
print(f"Mean RGB Stain Profile: R={mean_rgb[0]:.1f}, G={mean_rgb[1]:.1f}, B={mean_rgb[2]:.1f}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Center Shortcut Learning:* Models trained to classify cancer vs. normal frequently learn scanner background illumination or coverslip sealant tint. Always test whether your classifier can predict `medical_center_id`; if center classification accuracy is high, your model is exploiting confound shortcuts.
  2. *Patient-Level Leakage in Patch Splits:* Datasets such as NCT-CRC-HE-100K and LC25000 contain thousands of patches extracted from a smaller set of original patients. Random patch-level train/test splits result in severe data leakage. Splits **must be stratified at the patient level**.
  3. *Magnification Mismatch:* BreakHis contains patches at $40\times, 100\times, 200\times,$ and $400\times$, while NCT-CRC-HE is standardized at $20\times$. Feeding unrescaled $40\times$ patches into models expecting $20\times$ ($0.5\,\mu\text{m/pixel}$) induces severe out-of-scale feature degradation.

---

## 5. Downstream Foundation Model Consumers
The following foundation model systems utilize the PathoROB multi-center suite for robustness, out-of-domain generalization, and linear probing:
- **UNI2-h / UNI** (`docs/02_models/04_pathology/uni2_h.md`): Top-ranked robustness index benchmark.
- **Virchow2 / Virchow** (`docs/02_models/04_pathology/virchow2.md`): Top-ranked biological task AUROC benchmark.
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): Tile encoder generalizability evaluation.
- **H-Optimus-1** (`docs/02_models/04_pathology/h_optimus_1.md`): External multi-center evaluation.
- **TITAN** (`docs/02_models/04_pathology/titan.md`): Zero-shot patch-level classification.
- **CRISP** (`docs/02_models/04_pathology/crisp.md`): Tissue typing and intraoperative generalization.
