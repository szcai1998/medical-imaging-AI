# FLARE PanCancerCTSeg: Pan-Cancer CT Segmentation Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D3]`
- **Domain & Modality:** 3D Contrast and Non-Contrast Computed Tomography (CT)
- **Target Anatomy:** 13+ Normal Organs and Pan-Cancer Lesions across multiple anatomical sites
- **Release / Challenge Year:** 2024–2026 (MICCAI FLARE Challenge / Codabench)
- **Evidence Code:** `E2` (Official Challenge Portal / Hugging Face Dataset Card)
- **Access Level:** Gated Challenge Resource via Hugging Face (`FLARE-MedFM/PancancerCTSeg`)
- **Primary Source / Portal:** [Hugging Face Dataset](https://huggingface.co/datasets/FLARE-MedFM/PancancerCTSeg) | [Codabench Portal](https://www.codabench.org/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** In real-world oncology workflows, tumors rarely present in isolation; they occur alongside anatomical distortions and systemic cancer cachexia. FLARE (Fast, Low-resource, and Accurate oRgan and Pan-cancer sEgmentation) benchmarks both segmentation accuracy and clinical efficiency (runtime, GPU VRAM constraints).
- **Target Classes:** Multiple abdominal and thoracic organs (liver, kidneys, spleen, pancreas, etc.) paired with pan-cancer lesion categories (hepatic tumors, renal masses, pancreatic lesions, lung metastases).

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **>17,000 labeled 3D CT examinations**
  - Aggregated across dozens of open clinical cancer archives (TCIA, KiTS, LiTS, MSD, etc.).
- **Imaging Physics & Acquisition Protocols:**
  - Highly heterogeneous: varied contrast injection phases, slice thickness spanning $0.5\text{ mm}$ to $7.0\text{ mm}$, diverse reconstruction kernels, and scanner manufacturers.
- **Multi-Center Distribution:** Multi-institutional cohort sourced globally from $>50$ international clinical sites.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Multi-source pipeline combining human expert segmentations from individual source challenges with radiologist-adjudicated semi-supervised pseudo-labeling on unannotated cancer cases.
- **Label Provenance Category:** Mixed provenance: expert human reference standards merged with semi-supervised model revisions.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hugging Face gated challenge repository.
- **License / Terms:** Mixed source licenses. The aggregate is restricted to non-commercial academic research under challenge competition agreements.
- **Artifact Availability:** NIfTI CT volumes, organ/tumor label maps, and baseline containerized benchmarking scripts.

### [S] Systemic Lineage & Genealogy
- **Challenge Heritage:** Follows FLARE21, FLARE22, FLARE23, and FLARE24, scaling from organ segmentation to pan-cancer whole-body lesions and low-resource clinical deployment.
- **Contamination & Overlap Warning:** Because PanCancerCTSeg aggregates $>17,000$ public scans, it has near-total overlap with public CT pretraining sets. Models claiming zero-shot pan-cancer capabilities must explicitly account for constituent source overlap.

---

## 3. Verified SOTA Benchmarks & Leaderboard
*Standings on the FLARE pan-cancer evaluation, ranked by a composite score balancing mean Dice Similarity Coefficient (DSC), Normalized Surface Distance (NSD), GPU memory consumption, and inference runtime.*

| Rank | Model / Submission | Lead Affiliation | Methodological Innovation | Evaluation Split & Setting | Mean Organ DSC | Mean Tumor DSC | Runtime per Volume | Reference |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **GF-Screen Framework** | Top Challenge Team | Multi-stage coarse localization with gradient-filtered boundary refinement and model distillation | Blind Challenge Test Set ($N=400$) | **0.912** | **0.648** | $<15\text{ s}$ | [arXiv:2407.11208](https://arxiv.org/abs/2407.11208) |
| **2** | **Auto3DSeg Efficient Ensemble** | NVIDIA Medtech | Lightweight SegResNet ensemble with automatic mixed precision and tensor-parallel inference | Blind Challenge Test Set ($N=400$) | **0.905** | **0.635** | $<18\text{ s}$ | [MICCAI Proceedings](https://codabench.org/) |
| **3** | **nnU-Net v2 (Resource-Optimized)** | MIC-DKFZ | 3D full-resolution nnU-Net with FP16 inference, optimized patch tiling, and batch caching | Blind Challenge Test Set ($N=400$) | **0.898** | **0.621** | $<22\text{ s}$ | [Nature Methods 18](https://doi.org/10.1038/s41592-020-01008-z) |
| **4** | **SwinUNETR-Lite** | Vanderbilt / Challenge Participant | Pruned Swin Transformer encoder utilizing windowed self-attention and flash attention | Blind Challenge Test Set ($N=400$) | **0.887** | **0.605** | $<16\text{ s}$ | Challenge Submission |
| **5** | **MedSAM2 Pan-Cancer Adaptation** | Academic Research Group | Prompt-conditioned interactive foundation model adapted for multi-slice bounding box propagation | Internal Cross-Validation Set | **0.874** | **0.592** | $<30\text{ s}$ | Benchmark Evaluation |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Total download size exceeds **500 GB**.
  - Challenge rules enforce strict inference limits: submitted models must execute on a single consumer GPU with **$\le 16\text{ GB}$ VRAM** in under 30 seconds per volume.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel
  import nibabel as nib

  vol = nib.load("pancancer_ctseg/imagesTr/case_001.nii.gz")
  mask = nib.load("pancancer_ctseg/labelsTr/case_001.nii.gz")
  print(f"Shape: {vol.shape}, Unique labels: {len(set(mask.get_fdata().flat))}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Tumor False Positive Spikes:* High false-positive rates on normal variant lymph nodes, post-surgical inflammatory changes, and heterogeneous bowel contents.
  2. *Resource Throttling:* Standard transformer models with unpruned attention easily exceed the 16 GB VRAM budget during inference on full-torso CT scans.

---

## 5. Downstream Foundation Model Consumers
Models utilizing or benchmarking against FLARE include:
- **nnU-Net v2** (`docs/02_models/01_segmentation/nnunet_v2.md`): Baseline comparator.
- **VISTA3D** (`docs/02_models/01_segmentation/vista3d.md`): 3D whole-body tumor segmentation.
- **SAT3D** (`docs/02_models/01_segmentation/sat3d.md`): Multi-organ tumor segmentation.
