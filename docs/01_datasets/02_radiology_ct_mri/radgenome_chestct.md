# RadGenome-ChestCT: Grounded Vision-Language 3D Chest CT Dataset

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D21]`
- **Domain & Modality:** 3D Chest CT + 3D Multi-Organ Segmentation Masks + Region-Grounded Radiology Reports + Grounded Visual Question Answering (GVQA)
- **Target Anatomy & Pathologies:** 197 anatomical categories and abnormalities (airways, lung lobes, vascular tree, mediastinum, pleural space, ribs, lymph nodes)
- **Release / Publication Year:** 2025 (*Scientific Data*, DOI: 10.1038/s41597-025-05922-9)
- **Evidence Code:** `E1` (Peer-Reviewed Data Descriptor in *Scientific Data*) + `E2` (Official Hugging Face Dataset Card)
- **Access Level:** Public Research Access under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)
- **Primary Source / Portal:** [Scientific Data (Zhang et al., 2025)](https://doi.org/10.1038/s41597-025-05922-9) | [Hugging Face: RadGenome/RadGenome-ChestCT](https://huggingface.co/datasets/RadGenome/RadGenome-ChestCT) | [GitHub: xiaoman-zhang/RadGenome-ChestCT](https://github.com/xiaoman-zhang/RadGenome-ChestCT)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Standard medical vision-language models frequently suffer from spatial hallucination: they may correctly state that "a pulmonary nodule is present" but fail to localize it to the right lower lobe, or describe cardiomegaly while attending to liver tissue. RadGenome-ChestCT establishes fine-grained spatial correspondences by binding individual descriptive sentences and diagnostic questions directly to 3D voxel segmentation masks.
- **Target Tasks:** Grounded Report Generation (GRG) and Grounded Visual Question Answering (GVQA) across 197 anatomical structures and thoracic abnormalities.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **25,692 non-contrast 3D chest CT volumes** (inherited from the parent CT-RATE cohort)
  - **197 anatomical and pathological segmentation classes**
  - **665,000 region-grounded reports**
  - **1,200,000 grounded VQA pairs** in the *Scientific Data* version of record (October 2025)
  - **1,300,000 grounded VQA pairs** in the updated live Hugging Face release
  - *Version-Drift Note:* The live card reflects continuous automated pipeline expansion. Scientific evaluations must explicitly specify whether they benchmark against the 1.2M version-of-record split or the 1.3M live release.
- **Imaging Physics & Acquisition Protocols:**
  - Helical thoracic CT scans resampled to standardized spatial grids ($64 \times 128 \times 128$ or $240 \times 424 \times 424$) with aligned 3D binary and multi-label segmentation masks.

### [T] Truth & Annotation Provenance
- **Annotation & Derivation Protocol:**
  - **Anatomical Masks:** Automatically generated using deep neural networks and models including TotalSegmentator, followed by anatomical consistency filtering.
  - **Grounded Reports & VQA Pairs:** Extracted from CT-RATE clinical reports using named entity recognition (NER), anatomical dependency parsing, and large language model (LLM) curation pipelines (LLaMA/GPT-4), linking text phrases to spatial mask IDs.
  - **Radiologist Validation:** Expert thoracic radiologists validated a dedicated quality-assurance test subset for spatial accuracy and linguistic coherence.
- **Label Provenance Category:** Semi-synthetic / model-assisted spatial grounding. It is **not** a dataset of 197 classes manually contoured from scratch by human radiologists.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Openly available via Hugging Face (`RadGenome/RadGenome-ChestCT`) and GitHub.
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0).
- **Artifact Availability:** 3D segmentation masks in NIfTI format, grounded report JSONs (linking sentence spans to 3D bounding boxes and mask indexes), and grounded VQA query-answer-mask triplets.

### [S] Systemic Lineage & Genealogy
- **Parent Cohort:** Direct child and spatial extension of **CT-RATE `[D20]`**.
- **Contamination & Overlap Warning:** Because RadGenome-ChestCT shares identical imaging scans with CT-RATE, any foundation model pretrained on CT-RATE has already observed these scans. Testing such models on RadGenome evaluates *supervision transfer* and *spatial grounding adaptation*, **not** out-of-distribution institutional generalization.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Benchmark evaluation on the RadGenome-ChestCT official held-out test split ($N=2,500$ volumes). Evaluated on Grounded Visual Question Answering (GVQA Accuracy) and Grounded Report Generation (RadGraph F1 and BLEU-4).*

| Rank | Model / Architecture | Developing Team | Core Architecture & Strategy | Evaluation Split & Setting | GVQA Accuracy | RadGraph F1 | BLEU-4 | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **RadGenome-LLaMA3-8B** | Zhang et al. (SJTU / Shanghai AI Lab) | 3D CT patch encoder + LLaMA 3 8B with mask-conditioned adapter layers | Official Held-out Test Split ($N=2,500$) | **78.4%** | **0.612** | **0.324** | [Sci. Data 2025](https://doi.org/10.1038/s41597-025-05922-9) |
| **2** | **CT-CHAT (Adapted)** | Hamamci et al. / Zhang et al. | 3D CT-CLIP visual encoder + Vicuna-7B fine-tuned with mask prompt tokens | Official Held-out Test Split ($N=2,500$) | **73.6%** | **0.578** | **0.289** | [Sci. Data 2025](https://doi.org/10.1038/s41597-025-05922-9) |
| **3** | **3D-LLaVA-Med** | SJTU Baseline | 3D ResNet encoder + LLaVA projection layer + LLaMA-2-7B | Official Held-out Test Split ($N=2,500$) | **70.2%** | **0.541** | **0.264** | [Sci. Data 2025](https://doi.org/10.1038/s41597-025-05922-9) |
| **4** | **MedGemma 1.5 4B (Zero-Shot)** | Google Health AI | Native 3D patch tokenization autoregressive VLM | Zero-Shot Evaluation Protocol | **66.8%** | **0.519** | **0.238** | [MedGemma Model Card](https://developers.google.com/health-ai-developer-foundations/medgemma/model-card) |
| **5** | **Rad-LLM (Text-Only Prompt)** | Zhang et al. | Pure LLM receiving spatial coordinates as text tokens without 3D visual pooling | Official Held-out Test Split ($N=2,500$) | **61.5%** | **0.472** | **0.211** | [Sci. Data 2025](https://doi.org/10.1038/s41597-025-05922-9) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Mask and annotation archive: ~85 GB compressed.
  - Memory requirement: Evaluating frozen 3D vision encoders paired with 8B LLMs requires **24 GB VRAM** (RTX 3090 / RTX 4090) under 4-bit/8-bit quantization; full fine-tuning requires 48 GB to 80 GB GPUs.
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install nibabel
  import json
  import nibabel as nib

  # Inspect a grounded VQA sample and load its corresponding segmentation mask
  with open("radgenome_chestct/gvqa_test_sample.json") as f:
      vqa_sample = json.load(f)[0]

  print(f"Question: {vqa_sample['question']}")
  print(f"Ground Truth Answer: {vqa_sample['answer']}")
  print(f"Target Structure: {vqa_sample['category_name']} (ID: {vqa_sample['category_id']})")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Model-Derived Mask Artifacts:* Because the 197 segmentation masks were generated by neural networks (e.g., TotalSegmentator), boundary errors in challenging regions (e.g., retrocardiac atelectasis vs. pleural effusion) can leak into downstream evaluation targets.
  2. *Synthesized Question Formats:* The LLM-generated VQA pairs exhibit repetitive syntactic templates; models can achieve high accuracy by memorizing linguistic prompt cues rather than resolving 3D visual geometry.

---

## 5. Downstream Foundation Model Consumers
The following foundation models utilize RadGenome-ChestCT for grounded 3D reasoning and evaluation:
- **CT-CHAT** (`docs/02_models/02_volumetric_ct_mri/ct_clip_chat.md`): Used for grounded conversational question answering and instruction fine-tuning.
- **BiomedParse v2** (`docs/02_models/01_segmentation/biomedparse_v2.md`): Incorporates text-guided 3D anatomical segmentation targets.
- **MedGemma 1.5 4B** (`docs/02_models/02_volumetric_ct_mri/medgemma_1_5.md`): 3D visual question answering evaluations.
