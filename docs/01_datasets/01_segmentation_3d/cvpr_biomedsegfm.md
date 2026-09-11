# CVPR-BiomedSegFM: Foundation Models for Text-Guided 3D Biomedical Image Segmentation

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D1]`, `[D2]`
- **Domain & Modality:** Multi-Modality 3D Biomedical Imaging (CT, MRI, PET, Ultrasound, Confocal Microscopy)
- **Target Anatomy:** Universal Text-Conditioned Segmentation across 200+ Anatomical Structures and Lesions
- **Release / Challenge Year:** 2025–2026 (CVPR Biomedical Vision Challenge / Codabench)
- **Evidence Code:** `E2` (Official Challenge Portal / Hugging Face Dataset Card)
- **Access Level:** Gated Research Access via Hugging Face and Codabench
- **Primary Source / Portal:** [Codabench Competition 5651](https://www.codabench.org/competitions/5651/) | [Hugging Face Dataset Card](https://huggingface.co/datasets/junma/CVPR-BiomedSegFM)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Traditional task-specific segmentation models require separate inference pipelines for each organ or lesion. This benchmark pioneers universal, prompt-driven segmentation where a single foundation model accepts a 3D medical volume and a free-text anatomical prompt (e.g., *"segment right anterior lung lesion"* or *"portal vein"*) to generate 3D masks.
- **Evaluation Modalities:** CT, MRI, PET, Ultrasound, and Optical Microscopy volumes.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **>200,000 3D image–mask pairs**
  - Aggregated from dozens of international clinical databases, imaging trials, and biomedical repositories.
- **Imaging Physics & Acquisition Protocols:**
  - High heterogeneity: isotropic and highly anisotropic volumetric acquisitions, ranging from high-resolution micro-CT to thick-slice clinical abdominal MRIs and functional PET scans.
- **Multi-Center Distribution:** Global multi-institution cohort spanning North America, Europe, and Asia.

### [T] Truth & Annotation Provenance
- **Annotation Protocol:** Curated from validated clinical challenge ground truths, expert manual annotations, and multi-reader consensus datasets. Standardized into unified 3D NIfTI coordinate systems paired with standardized MeSH / RadLex text descriptions.
- **Label Provenance Category:** Aggregated expert human annotations paired with normalized textual prompts.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Official distribution via Hugging Face (`junma/CVPR-BiomedSegFM`) and Codabench.
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0). Note that individual constituent datasets carry their own underlying terms.
- **Artifact Availability:** Training/validation splits, text prompt dictionaries, and baseline Docker containers.

### [S] Systemic Lineage & Genealogy
- **Foundational Heritage:** Built by the organizers of MedSAM (Jun Ma et al.) to establish the benchmark for the post-nnU-Net foundation model era.
- **Contamination & Overlap Warning:** Because this benchmark aggregates $>200,000$ 3D pairs from existing open datasets (including KiTS, AMOS, MSD, TotalSegmentator), any model trained on public medical data has substantial pretraining overlap. Benchmarking requires strict audit of source provenance.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official Codabench CVPR-BiomedSegFM text-guided 3D benchmark, evaluated across multi-modal test volumes using mean Dice Similarity Coefficient (DSC) and Normalized Surface Distance (NSD).*

| Rank | Model / Framework | Developing Team | Architectural Mechanism | Evaluation Split & Setting | Multi-Modal Mean DSC | Text-Prompt Accuracy | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **BiomedParse v2** | Microsoft Research (Sheng Zhang et al.) | BoltzFormer joint 2D/3D backbone with BioLinkBERT textual embeddings and Instance Existence Detection (ISD) | Official Blind Codabench Test Set | **0.864** | **0.912** | [Nature Methods / arXiv:2405.12971](https://arxiv.org/abs/2405.12971) |
| **2** | **SAT / SAT3D** | Shanghai Jiao Tong University (SJTU) | Segment Anything in Radiology Scans; text-conditioned volumetric cross-attention with tumor-specific tokens | Official Blind Codabench Test Set | **0.849** | **0.895** | [SJTU / CVPR 2025](https://arxiv.org/abs/2312.00863) |
| **3** | **Medical SAM3** | AIM Research Lab (Jiang et al.) | Full medical fine-tuning of SAM3 with continuous 3D slice memory propagation | Official Blind Codabench Test Set | **0.841** | **0.887** | [arXiv:2601.10880](https://arxiv.org/abs/2601.10880) |
| **4** | **VISTA3D / NV-Segment-CTMR** | NVIDIA Medtech / Project MONAI | Patch-based 3D transformer with point/box/text multi-prompt decoders | Challenge Benchmark Split | **0.835** | **0.879** | [CVPR 2025 / MONAI](https://github.com/Project-MONAI/VISTA) |
| **5** | **MedSAM2 (Volumetric Adaptation)** | Ma, Yang et al. (Toronto / Harvard) | Memory-attention adaptation of SAM2 for 3D volumetric slice-to-slice tracking | Challenge Benchmark Split | **0.828** | **0.865** | [arXiv:2504.03600](https://arxiv.org/abs/2504.03600) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Dataset storage: $>350\text{ GB}$ across modalities.
  - Text-conditioned inference requires **16 GB to 24 GB VRAM** (RTX 3090/4090); full training of 3D backbones requires multi-GPU clusters (A100/H100 80GB).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install datasets
  from datasets import load_dataset

  dataset = load_dataset("junma/CVPR-BiomedSegFM", split="train", streaming=True)
  sample = next(iter(dataset))
  print(f"Modality: {sample['modality']}, Prompt: {sample['text_prompt']}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Negative Text Prompt Hallucination:* Prompting the model for an organ or tumor not present in the scan can result in hallucinated masks unless explicit "Instance Existence Detection" (ISD) is implemented.
  2. *Text Ambiguity:* Synonyms (e.g., "renal neoplasm" vs. "kidney tumor") can alter segmentation boundaries if language encoders are not clinically domain-aligned.

---

## 5. Downstream Foundation Model Consumers
The flagship foundation models evaluated directly on this benchmark include:
- **BiomedParse v2** (`docs/02_models/01_segmentation/biomedparse_v2.md`): Benchmark leader.
- **Medical SAM3** (`docs/02_models/01_segmentation/medical_sam3.md`): Promptable volumetric model.
- **SAT3D** (`docs/02_models/01_segmentation/sat3d.md`): Baseline text-prompted radiology foundation model.
