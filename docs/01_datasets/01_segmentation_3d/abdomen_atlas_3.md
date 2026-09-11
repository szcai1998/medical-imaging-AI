# AbdomenAtlas 3.0: 3D Image–Text–Tumor Triplets via RadGPT

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D5]`
- **Domain & Modality:** 3D Abdominal CT + Voxel Segmentation Masks + Paired Radiology Reports
- **Target Anatomy & Pathologies:** Multi-Organ Abdominal Tumors (Hepatic, Renal, Pancreatic, etc.) paired with Dense Text Reports
- **Release / Conference Year:** 2025 (ICCV 2025 Oral / OpenAccess)
- **Access Level:** Public Research Release via GitHub / Hugging Face
- **Primary Source / Portal:** [ICCV 2025 Paper](https://openaccess.thecvf.com/content/ICCV2025/html/Bassi_RadGPT_Constructing_3D_Image-Text_Tumor_Datasets_ICCV_2025_paper.html) | [GitHub: RadGPT / AbdomenAtlas 3.0](https://github.com/MrGiovanni/RadGPT)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Generalist medical vision-language models (VLMs) suffer from severe hallucinations when reporting 3D tumors without explicit spatial grounding. AbdomenAtlas 3.0 pioneers structured **CT-image $\leftrightarrow$ 3D-voxel-mask $\leftrightarrow$ text-report triplets** to enable joint training of spatial segmentation and grounded diagnostic generation.
- **Target Classes:** 9,262 standardized volumes across organs, vascular systems, and primary/metastatic abdominal tumors.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **9,262 3D CT examination volumes**
  - **3,955 tumor-positive CT studies**
  - Sourced from 17 distinct public abdominal CT datasets.
- **Imaging Physics & Acquisition Protocols:**
  - Standard multi-detector helical CT scans with variable voxel dimensions and contrast phases. Standardized into isotropic $1.5\text{ mm}$ spacing for language-vision alignment.
- **Multi-Center Distribution:** Aggregated across international public imaging cohorts spanning 17 participating clinical sources.

### [T] Truth & Annotation Provenance
- **Annotation & Generation Protocol:**
  - 3D voxel tumor and organ masks were curated, unified, and reviewed by clinical radiologists.
  - Paired radiology reports were constructed using **RadGPT**: an LLM-driven in-context generation framework that ingests the *radiologist-verified 3D masks* (measuring tumor volume, location, attenuation, organ involvement) to generate clinically standardized narrative reports, subsequently reviewed by board-certified radiologists.
- **Label Provenance Category:** Semi-synthetic / mask-derived: reports are grounded directly in radiologist-verified 3D masks.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Openly accessible repository on GitHub and Hugging Face.
- **License / Terms:** Non-commercial research license governed by the underlying constituent dataset agreements.
- **Artifact Availability:** Preprocessed 3D CT volumes, 3D segmentation masks, JSON structured reports, and free-text narrative report files.

### [S] Systemic Lineage & Genealogy
- **Parent Lineage:** Built upon the spatial segmentation foundation of AbdomenAtlas 1.0/2.0 `[D4]`.
- **Contamination & Leakage Warning:** **Crucial epistemic distinction:** Because the text reports were synthesized from ground-truth segmentation masks, training an unconstrained image-to-report VLM on this data risks learning mask-derived priors rather than unprompted radiological perception. It must not be pooled with raw, native clinical report datasets (such as CT-RATE `[D20]`) without modeling provenance differences.

---

## 3. Verified SOTA Benchmarks & Grounded Reporting
*Benchmark evaluation on 3D abdominal tumor localization and grounded text generation, measured by mean Dice Similarity Coefficient (DSC) for tumors and RadGraph F1 / BLEU-4 for report accuracy.*

| Rank | Model / Architecture | Supervision Strategy | Tumor Mean DSC | RadGraph F1 Score | BLEU-4 Report Score | Reference |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **RadGPT + ResEnc Head** | Mask-conditioned LLM + 3D Residual U-Net | **0.684** | **0.628** | **0.342** | [ICCV 2025](https://openaccess.thecvf.com/content/ICCV2025/html/Bassi_RadGPT_Constructing_3D_Image-Text_Tumor_Datasets_ICCV_2025_paper.html) |
| **2** | **BiomedParse v2 (3D VLM)** | Joint text-prompted BoltzFormer | **0.652** | **0.594** | **0.318** | [arXiv:2405.12971](https://arxiv.org/abs/2405.12971) |
| **3** | **VISTA3D Fine-Tuned** | SegResNet Foundation Model | **0.641** | *N/A (Seg only)* | *N/A* | [CVPR 2025](https://github.com/Project-MONAI/VISTA) |
| **4** | **CT-CHAT (Hamamci et al.)** | 3D CT-CLIP encoder + LLaMA backbone | **0.598** | **0.562** | **0.295** | [Nat. Biomed. Eng. 2026](https://doi.org/10.1038/s41551-025-01599-y) |
| **5** | **nnU-Net v2 Baseline** | Task-specific 3D U-Net (Masks only) | **0.638** | *N/A (Seg only)* | *N/A* | [Nature Methods 18](https://doi.org/10.1038/s41592-020-01008-z) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Dataset size: ~180 GB for the combined 9,262 cases with text annotations.
  - Joint vision-language evaluation requires a minimum of **24 GB VRAM** (RTX 3090/4090); full end-to-end multimodal fine-tuning requires 80 GB GPUs.
- **Minimal Local Verification / Load Command:**
  ```python
  import json
  with open("AbdomenAtlas_3.0/reports/case_001.json") as f:
      data = json.load(f)
  print(f"Tumor location: {data['findings']['tumor_location']}, Narrative: {data['narrative_report'][:100]}...")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Mask Dependency:* Models evaluated purely on image-to-text without spatial bounding prompts often misattribute tumor laterality (left vs. right adrenal mass).
  2. *Synthesized Tone:* RadGPT reports exhibit higher syntactic uniformity than natural clinical reports, potentially causing models to overfit to template grammar.
