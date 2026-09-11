# Chest ImaGenome: Large-Scale Scene Graph & Localized Comparison Benchmark on MIMIC-CXR

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D18]`
- **Domain & Modality:** Digital Radiography (DX) & Computed Radiography (CR), frontal chest radiographs (PA and AP)
- **Target Anatomy & Pathologies:** Thoracic anatomy structured as a formal scene graph; 29 anatomical regions, localized bounding boxes, attributes (normal, abnormal, severity), and spatial/temporal comparison relations
- **Release / Challenge Year:** 2021 (Wu, Agu, Lourentzou et al. / PhysioNet v1.0.0; Nature Scientific Data)
- **Evidence Code:** `E1` (Peer-Reviewed Scientific Data Descriptor) + `E2` (Official PhysioNet v1.0.0 Release)
- **Access Level:** Credentialed Access / DUA Required (Requires approved PhysioNet credentialing for MIMIC-CXR)
- **Primary Source / Portal:** [https://physionet.org/content/chest-imagenome/1.0.0/](https://physionet.org/content/chest-imagenome/1.0.0/) | Paper: [https://doi.org/10.1038/s41597-021-01058-x](https://doi.org/10.1038/s41597-021-01058-x) | GitHub: [https://github.com/allenai/chest-imagenome](https://github.com/allenai/chest-imagenome)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Unstructured radiology reports contain implicit relational, anatomical, and temporal information that flat multi-label binary classifiers fail to exploit. Chest ImaGenome transforms clinical reporting into a machine-actionable **medical scene graph**, where nodes represent 29 anatomical structures, node attributes represent localized radiological observations (e.g., opacity, effusion, enlargement), and edges represent spatial connections or temporal comparisons across serial radiographs.
- **Target Classes & Schema:**
  - **29 Anatomical Regions:** Trachea, carina, left/right lung, cardiac silhouette, aortic arch, left/right hilar structures, left/right hemidiaphragms, costophrenic angles, spine, clavicles, ribs.
  - **Attributes:** Anatomic status (normal, abnormal), abnormality types, degree of severity, and temporal change relative to priors (e.g., improved, worsened, unchanged, new).
  - **Relations:** Spatial relationships (e.g., adjacent to, surrounding) and longitudinal comparison relations.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **242,072 frontal chest radiographs with automatically derived scene graphs (Silver Dataset)**
  - **29 localized anatomical bounding box regions per image**
  - **>670,000 localized temporal comparison relations**
  - **500 unique patients (1,000 radiographs) in the Gold Standard Dataset** (fully adjudicated by board-certified radiologists)
- **Imaging Physics & Acquisition Protocols:**
  - Derived from the frontal (PA and AP) radiograph cohort of the MIMIC-CXR v2.1.0 database `[D12]`.
  - Bedside mobile AP and upright PA projections from Beth Israel Deaconess Medical Center, Boston, MA.
  - Native pixel dimensions preserved from MIMIC-CXR.
- **Multi-Center Distribution:** Sourced entirely from a single tertiary academic medical center (BIDMC).

### [T] Truth & Annotation Provenance
- **Annotation Protocol:**
  - **Silver Dataset (242k Scans):** Constructed using an automated natural language processing (NLP) pipeline. Clinical entities, anatomical mentions, and temporal relations were parsed from MIMIC-CXR free-text reports using a rule-based parser and clinical BERT. Corresponding anatomical bounding boxes were generated using an Atlas-guided Faster R-CNN object detector trained on anatomical priors.
  - **Gold Dataset (500 Patients / 1,000 Scans):** Board-certified practicing radiologists manually reviewed, corrected, and verified both the anatomical bounding box coordinates and the extracted attribute/relation graphs.
- **Label Provenance Category:** Bimodal: automated pipeline combining NLP and model-generated bounding boxes (Silver Dataset, 242k scans) contrasted with verified human expert consensus (Gold Dataset, 500 patients).

### [A] Access, Terms & Artifacts
- **Repository / Download:** Hosted on PhysioNet (`chest-imagenome/1.0.0`).
- **License / Terms:** PhysioNet Credentialed Data Use Agreement; strictly non-commercial academic research. Access inherits prerequisites from MIMIC-CXR.
- **Artifact Availability:**
  - JSON scene graphs for all 242,072 images (`silver_dataset/`).
  - Radiologist-curated gold annotations (`gold_dataset/`).
  - Anatomical bounding box coordinates in tabular CSV format.
  - Image files are mapped directly from parent MIMIC-CXR-JPG via `dicom_id`.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Direct derivative of **MIMIC-CXR v2.1.0 `[D12]`**.
- **Derivative Ecosystem:** Sourced as a training and evaluation resource for anatomical grounding, scene-graph-aided report generation (SGRRG), and longitudinal progression modeling.
- **Critical Genealogy Rule & Contamination Warning (Mandatory Section 3A.3 Ground-Layer Audit):**
  - **MIMIC-CXR $\rightarrow$ Chest ImaGenome is a parent $\rightarrow$ derivative relationship.**
  - A model pretrained on MIMIC-CXR that is evaluated on Chest ImaGenome scene graphs has **NOT** undergone external multi-center evaluation. It evaluates structured representational competence within the same patient demographic and scanner distribution.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official Chest ImaGenome Gold Standard evaluation benchmark. Evaluated across anatomical abnormality classification, relation reasoning, and temporal progression prediction.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | Macro AUROC | Temporal Accuracy | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **CoCa-CXR** | Academic Research Consortium (MICCAI 2024) | Contrastive captioner with longitudinal cross-attention for multi-image temporal progression reasoning | Gold Test Split ($N=500$ Patients) | **0.884** | **65.0%** (5 conditions) | [MICCAI 2024](https://miccai.org/) |
| **2** | **SGRRG** | Anatomy-Guided Team (2024) | Scene graph-aided report generator with dual-encoder graph convolutional network and token alignment | Gold Test Split ($N=500$ Patients) | **0.871** | — (CheXbert F1: 0.682) | [arXiv:2404.03210](https://arxiv.org/abs/2404.03210) |
| **3** | **BioViL-T** | Bannur et al. (Microsoft Research) | Temporally aware vision-language foundation model trained with prior-current image pairs | Gold Test Split ($N=500$ Patients) | **0.862** | 61.2% | [CVPR 2023](https://openaccess.thecvf.com/) |
| **4** | **Relation Transformer Baseline** | Wu et al. (Stanford / Independent) | Faster R-CNN region extractor + relational transformer for scene graph generation | Official Benchmark Test Phase | **0.835** | 56.4% (SG Recall@50: 0.428) | [Scientific Data 2021](https://doi.org/10.1038/s41597-021-01058-x) |
| **5** | **Standard ResNet-50 Baseline** | Wu et al. (Benchmark Baseline) | Independent regional classification baseline with isolated anatomical crops | Gold Test Split ($N=500$ Patients) | **0.798** | 52.1% | [Scientific Data 2021](https://doi.org/10.1038/s41597-021-01058-x) |

---

## 4. Ground-Layer Practical Guidance

- **Workstation Compute Feasibility:**
  - Scene graph JSON files: **~500 MB**, easily stored in local memory or SSD.
  - Image access requires mounting parent MIMIC-CXR-JPG (~50 GB for $512 \times 512$ resolution).
  - Graph neural network or transformer training runs efficiently on a single **16 GB–24 GB VRAM** GPU (e.g., RTX 3090 / RTX 4080).
- **Minimal Local Verification / Load Command:**
  ```python
  # Requirements: pip install pandas pillow
  import json
  import pandas as pd

  # 1. Load sample scene graph JSON from gold dataset
  with open("gold_dataset/scene_graph_sample.json", "r") as f:
      sg = json.load(f)

  print(f"Image ID: {sg.get('image_id', 'sample')}")
  print(f"Number of anatomical nodes: {len(sg.get('objects', []))}")
  if 'objects' in sg and len(sg['objects']) > 0:
      sample_node = sg['objects'][0]
      print(f"Node: {sample_node.get('name')}, Attributes: {sample_node.get('attributes')}")
  ```
- **Dominant Failure Modes & Gotchas:**
  1. *Conflating Silver and Gold Truth:* In the Silver Dataset (242k images), anatomical bounding boxes were generated by an automated Faster R-CNN model, NOT drawn by humans. In cases of severe cardiomegaly or pleural effusion, the model's boxes distort or clip anatomy. Benchmark evaluations must prioritize the **500-patient Gold Standard**.
  2. *Severe Graph Sparsity:* The vast majority of anatomical nodes across normal radiographs possess zero abnormal attributes. Training loss functions must incorporate class weighting, focal loss, or negative sampling to prevent trivial majority-class collapse.

---

## 5. Downstream Foundation Model Consumers
The following foundation model lineages utilize Chest ImaGenome as a structured reasoning or progression benchmark:
- **BioViL-T** (`docs/02_models/02_cxr/biovil.md`): Longitudinal reasoning and temporal progression benchmark.
- **MAIRA-2** (`docs/02_models/02_cxr/maira_2.md`): Evaluates anatomical finding grounding and report structuring.
- **CARE-X** (`docs/02_models/02_cxr/care_x.md`): Multi-task grounding and structured question answering.
- **CLEAR** (`docs/02_models/02_cxr/clear.md`): Validates clinical concept graph consistency.
- **MedVersa** (`docs/02_models/02_cxr/medversa.md`): Multi-task coordination and anatomical classification.
