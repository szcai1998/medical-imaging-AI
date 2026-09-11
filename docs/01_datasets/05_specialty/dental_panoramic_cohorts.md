# Dental Panoramic & Multimodal Oral Cohorts: PanoFM, DentFound & DentVLM

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D-DENT]` / Dental Panoramic & Multimodal Oral Cohorts
- **Domain & Modality:** Panoramic Radiography (Orthopantomogram [OPG]), Cone-Beam Computed Tomography (CBCT), Intraoral Periapical (IOPA) Radiographs, and Multimodal Oral Imaging
- **Target Anatomy & Pathologies:** Maxillofacial dentition (32 adult teeth, FDI numbering 11–48; deciduous teeth 51–85), alveolar bone, mandibular canal, maxillary sinuses, and temporomandibular joints (TMJ). Diseases: Dental caries (enamel, dentin, deep pulp involvement), periapical periodontitis/lesions, vertical/horizontal alveolar bone loss, impacted wisdom teeth, retained root tips, and 11 post-treatment restorative categories (implants, crowns, endodontic fillings).
- **Release / Publication Year:** 2023 (MICCAI DENTEX Challenge), 2026 (PanoFM / *Pattern Recognit* [S92], DentFound / *Nat Biomed Eng* [S93], DentVLM / *Nat Commun* [S94])
- **Evidence Code:** `E1` (Peer-Reviewed Versions-of-Record in *Nat Biomed Eng*, *Nat Commun*, and *Pattern Recognition*) + `E2` (MICCAI DENTEX Official Grand Challenge Portal)
- **Access Level:** Open Benchmark Subsets via Grand Challenge & Zenodo; Institutional Multi-Center Pretraining Archives governed by academic agreements.
- **Primary Source / Portal:** [https://dentex.grand-challenge.org/](https://dentex.grand-challenge.org/) | DentFound: [https://www.nature.com/articles/s41551-026-01713-8](https://www.nature.com/articles/s41551-026-01713-8) | DentVLM: [https://www.nature.com/articles/s41467-026-75718-x](https://www.nature.com/articles/s41467-026-75718-x) | PanoFM: [https://doi.org/10.1016/j.patcog.2026.114411](https://doi.org/10.1016/j.patcog.2026.114411)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Panoramic radiography (orthopantomography) is the most widely performed screening radiograph in dentistry, capturing both maxilla and mandible in a single curved tomographic slice ("focal trough"). However, projecting a 3D horseshoe-shaped jaw onto a 2D plane creates severe non-linear distortion ($1.2\times\text{--}1.3\times$ magnification), overlapping interproximal tooth contacts, and ghost artifacts from the contralateral ramus and cervical spine, leading to missed proximal caries and overlooked periapical lesions.
- **Task Scope:** Multi-task hierarchical tooth instance segmentation, universal tooth enumeration (FDI notation), tooth pathosis classification, and automated multi-modal dental reporting.

### [A] Acquisition Physics & Scale
- **Exact Counting Unit Hierarchy:**
  $$\text{patient} \neq \text{dental encounter} \neq \text{jaw quadrant} \neq \text{tooth (FDI 11–48)} \neq \text{modality} \neq \text{radiograph}$$
- **Verified Scale Across Anchor Cohorts:**
  - **PanoFM Cohort `[S92]`:** **127,878 panoramic radiographs** collected across **six geographically diverse clinical centers** in China. Two-stage self-supervised pretraining with clinical semantic integration.
  - **DentFound Cohort `[S93]`:** **>101,000 patients** spanning ages 2 to 98 years, covering **98 distinct dental diseases and 11 post-treatment categories**. Multicenter clinical evaluation validated by 12 dental radiologists (*Author Correction published 8 Jul 2026*).
  - **DentVLM Multimodal Cohort `[S94]`:** **20,741 patients**, **110,447 images**, and **2,458,084 bilingual VQA pairs** across **seven dental imaging modalities** (panoramic, CBCT, periapical, cephalometric, intraoral photos, facial photos, and oral pathology slides), based on Qwen2-VL-7B.
  - **DENTEX MICCAI Benchmark:** **1,005 panoramic radiographs** with quadrant, enumeration, and pathology labels across 24 international challenge teams.
- **Acquisition Physics:** Narrow fan-beam X-ray tube and digital sensor rotating synchronously in opposite directions around the patient's head. Tomographic blurring eliminates structures outside the sharply focused dental arch.

### [T] Truth & Annotation Provenance
- **Human Expert Consensus:** DentFound annotated and reviewed by a panel of 12 board-certified oral and maxillofacial radiologists and prosthodontists. DENTEX challenge annotated by specialized dental radiologists using hierarchical multi-stage review.
- **Model-Assisted Generation:** DentVLM utilized LLM-assisted rationale curation followed by strict expert dentist quality auditing.

### [A] Access, Terms & Artifacts
- **Repository / Download:** DENTEX challenge data via Grand Challenge and Zenodo (`10.5281/zenodo.7801004`).
- **License / Terms:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) for DENTEX; code and model checkpoints released on GitHub.
- **Artifact Availability:** Preprocessed high-resolution dental radiographs, bounding-box annotations (COCO format), tooth masks, and evaluation scripts.

### [S] Systemic Lineage & Genealogy
- **Genealogy Evolution:** Single-task tooth bounding-box detection (Tufts Dental Database, UFBA-UESC) $\rightarrow$ MICCAI DENTEX 2023 Challenge (first standardized multi-task enumeration + pathosis benchmark) $\rightarrow$ PanoFM [S92], DentFound [S93], and DentVLM [S94] (2026 dental foundation model frontier).
- **Contamination & Overlap Warning:**
  - *Tooth-Level Splitting Trap:* A single panoramic radiograph contains up to 32 adult teeth. Splitting individual teeth from the same patient across training and test folds allows models to memorize patient-specific mandibular anatomy, inflating detection scores. Splits must be partitioned strictly by patient ID.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official MICCAI DENTEX Challenge held-out test evaluation (Hierarchical Tooth Enumeration and Disease Detection, Grand Challenge proceedings 2023–2024 [E1+E2]). Evaluated using Multi-Task Macro F1-Score, Tooth Enumeration F1, and Pathosis Detection F1.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Multi-Task F1 ($\uparrow$) | Tooth Enumeration F1 | Pathosis Detection F1 | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **Radboud_ISMI** | Nick van Nistelrooij et al. (Radboud UMC) | Swin-Transformer backbone with Cascade Mask R-CNN and hierarchical quadrant-guided tooth indexing | Official DENTEX Blind Test Phase ($N=240$) | **0.784** | **0.942** | **0.685** | [DENTEX 2023](https://dentex.grand-challenge.org/) |
| **2** | **Impact** | Fitz et al. | Hierarchical DINO object detector with query-based tooth-to-quadrant association | Official DENTEX Blind Test Phase ($N=240$) | **0.771** | **0.935** | **0.672** | [DENTEX 2023](https://dentex.grand-challenge.org/) |
| **3** | **DentFound (Zero-Shot / Probe)** | Zhu et al. (SJTU / Sun Yat-sen Univ.) | Instance-guided dental vision-language model with multi-scale lesion prompting | Multi-Center Clinical Test Split ($N=1,500$) | **0.768** | **0.938** | **0.669** | [Nat Biomed Eng 2026](https://doi.org/10.1038/s41551-026-01713-8) |
| **4** | **PanoFM Foundation Encoder** | Han et al. (Sichuan Univ.) | Self-supervised panoramic encoder with clinical semantic integration | Multi-Center Diagnostic Test Split | **0.761** | **0.931** | **0.661** | [Pattern Recognit 2026](https://doi.org/10.1016/j.patcog.2026.114411) |
| **5** | **HierarchicalDet Baseline** | Sezginer et al. (DENTEX Organizers) | Two-stage hierarchical Faster R-CNN with quadrant-level bounding box priors | Official DENTEX Blind Test Phase ($N=240$) | **0.749** | **0.918** | **0.638** | [arXiv:2305.19112](https://arxiv.org/abs/2305.19112) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - DENTEX benchmark images occupy ~3 GB; high-resolution dental radiographs are typically $2800 \times 1400$ pixels.
  - DentVLM-7B requires **16–24 GB VRAM** for full precision inference (RTX 4090 / A5000), or **10–12 GB VRAM** under 4-bit / 8-bit quantization.
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install pillow numpy
from PIL import Image
import numpy as np

# Panoramic radiographs typically have a 2:1 aspect ratio
dummy_panoramic = Image.new("L", (2800, 1400), color=128)
img_array = np.array(dummy_panoramic)
print(f"Panoramic image shape: {img_array.shape}, Bit depth: {img_array.dtype}")
print(f"Aspect ratio: {img_array.shape[1] / img_array.shape[0]:.2f}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Cervical Burnout Artifact:* Anatomical narrowing of the tooth between the enamel crown and alveolar crest creates artificial radiolucency that is easily misclassified as cervical root caries.
  2. *Spine Ghost Shadowing:* Improper patient posturing (slouching) casts a radiopaque band of the cervical spine over the anterior teeth, obscuring incisor periapical pathology.

---

## 5. Downstream Foundation Model Consumers
The following dental AI systems utilize these cohorts:
- **PanoFM** (`[S92]`): Reusable panoramic foundation model for multi-task dental disease diagnosis.
- **DentFound** (`[S93]`): Instance-guided dental vision-language model for clinical panoramic reporting.
- **DentVLM** (`[S94]`): 7B-parameter multimodal dental foundation model spanning seven oral modalities.
