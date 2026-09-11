# CPTAC: Clinical Proteomic Tumor Analysis Consortium

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D26]`
- **Domain & Modality:** Brightfield Whole-Slide Imaging (WSI, H&E Formalin-Fixed Paraffin-Embedded [FFPE] & Snap-Frozen Cryosections) paired with Mass-Spectrometry Proteogenomics & Radiology
- **Target Anatomy & Pathologies:** Multi-cancer proteogenomic cohorts across 10+ major malignancies (e.g., UCEC, LUAD, LSCC, CCRCC, PDA, GBM, HNSCC, COAD, BRCA, OV)
- **Release / Milestone Year:** 2011–present (CPTAC-2 & CPTAC-3 landmark proteogenomic releases 2019–2021; ongoing NCI data commons live repositories)
- **Evidence Code:** `E1` (Consortium Peer-Reviewed Cell/Cancer Cell Papers) + `E2` (Official NCI GDC & PDC Data Commons) + `E5A` (Independent PathBench Benchmark `[S28]`)
- **Access Level:** Multi-Tier Distributed Access (Imaging WSIs, proteomic tables, and somatic mutations are Open Access; raw germline BAM/FASTQ sequencing files are Controlled Access via dbGaP)
- **Primary Source / Portal:** [NCI GDC CPTAC Portal](https://gdc.cancer.gov/about-gdc/contributed-genomic-data-cancer-research/clinical-proteomic-tumor-analysis-consortium-cptac) | [Proteomic Data Commons (PDC)](https://pdc.cancer.gov/) | [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Systemic functional proteomic and post-translational characterization of human tumors. In computational pathology, CPTAC provides the primary benchmark for predicting proteomic pathway expression, phosphoproteomic activation, immune microenvironment composition, and multi-omics molecular subtypes directly from H&E morphology without requiring destructive mass-spectrometry assays.
- **Target Classes & Prediction Tasks:**
  1. *Morphology-to-Proteome Mapping:* Direct prediction of global protein abundance and phosphosite activation states from digitized H&E slides.
  2. *Molecular Subtype Classification:* Re-identification of consensus transcriptomic/proteomic subtypes (e.g., classical vs. basal-like pancreatic cancer; POLE, MSI, copy-number high/low in uterine serous/endometrioid carcinoma).
  3. *Independent Cross-Cohort Generalization:* Serving as the primary external validation testbed for models pretrained or tuned on TCGA.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  $$\text{1,500+ Patients} \neq \text{>5,000 Whole-Slide Images (WSIs)} \neq \text{>500 Million Tiles} \neq \text{Mass-Spec Spectra}$$
  - **~1,500+ deeply characterized cancer patients** across prospective CPTAC-2 and CPTAC-3 cohorts.
  - **>5,000 digitized whole-slide images**, comprising diagnostic surgical resections (`.svs`, FFPE) and companion frozen cryosections used for biopsy verification.
  - Hundreds of millions of extracted patches at $20\times$ ($\approx 0.50\,\mu\text{m/px}$) and $40\times$ ($\approx 0.25\,\mu\text{m/px}$) optical resolution.
- **Imaging Physics & Acquisition Protocols:**
  - Standard brightfield whole-slide scanners (primarily Aperio ScanScope CS2 / AT2 and Leica Aperio GT 450).
  - High-resolution digital imaging accompanied by paired radiologic examinations (CT and MRI scans available through TCIA for matching patient cohorts).
  - Multi-center international clinical sites following rigorous standardized tissue collection protocols to minimize cold-ischemia times for proteomic preservation.

### [T] Truth & Annotation Provenance
- **Reference Standard:** High-resolution mass spectrometry (tandem mass tag [TMT] isobaric labeling and label-free LC-MS/MS), comprehensive genomic sequencing (WES, whole-genome, RNA-seq, microRNA), and expert central pathology review.
- **Label Provenance Category:** Native human biochemical consensus and adjudicated molecular profiling.
- **Critical Annotation Caveat:** Pathology slides were primarily collected to confirm tumor purity ($\ge 60\text{--}70\%$ viable tumor nuclei) prior to tissue pulverization for proteomic assays; spatial annotations are slide-level and tissue-block-level, not cellular-level segmentations.

### [A] Access, Terms & Artifacts
- **Repository / Download:**
  - **Do NOT describe "CPTAC" as one monolithic dataset with one single license or portal.**
  - Whole-slide pathology images and genomic data are downloaded via the NCI GDC (`gdc-client`).
  - Mass-spectrometry proteomic raw files, spectral libraries, and normalized abundance tables are hosted separately on the **Proteomic Data Commons (PDC)**.
  - Matched clinical radiology CT/MRI DICOM scans reside on **The Cancer Imaging Archive (TCIA)**.
- **License / Terms:** Open Access under NIH/NCI data-sharing terms for public tiers. Open academic and commercial use permitted, with standard consortium attribution.
- **Artifact Availability:** Multi-resolution SVS pyramidal pathology files, PDC proteomic matrices, TCIA DICOM series, and clinical survival spreadsheets.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** National Cancer Institute Clinical Proteomic Tumor Analysis Consortium (CPTAC).
- **Derivative Ecosystem:** Sourced as an independent external test cohort in the 32-model *Nature Communications* PathBench study (`[S28]`), the 19-model multi-cohort *Nat Biomed Eng* study (`[S82]`), and foundation model evaluations.
- **Contamination & Overlap Warning:**
  - While CPTAC is frequently treated as an "external test set" for models trained on TCGA, some recent foundation models have begun ingesting public CPTAC pathology slides into extended pretraining corpora.
  - Zero-shot evaluations on CPTAC must explicitly audit whether CPTAC WSI identifiers were present in the model's pretraining manifest.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standardized evaluation on slide-level molecular subtyping and proteomic biomarker prediction on held-out CPTAC validation cohorts, as reported in the 32-model PathBench benchmark (Bareja et al., Nature Communications 17, 9012, 2026 [S28]) and related literature.*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | CPTAC Mean AUROC | Balanced F1 Score | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **1** | **Virchow2 + Attn-MIL** | Paige AI / MSKCC | ViT-H (632M params) tile backbone + slide-level attention MIL pooling | Held-out CPTAC pan-cancer validation cohort | **0.884** | **0.812** | [arXiv:2408.00738](https://arxiv.org/abs/2408.00738) / [PathBench](https://pathbench.stanford.edu/) |
| **2** | **Prov-GigaPath** | Providence / Microsoft | ViT-G (1.38B tiles) + GigaPath whole-slide LongNet architecture | Held-out CPTAC lung & endometrial cohorts | **0.879** | **0.805** | [Nature 630, 476–483 (2024)](https://doi.org/10.1038/s41586-024-07441-w) |
| **3** | **UNI2-h + Attention-MIL** | Mahmood Lab / Harvard | Custom ViT-H (681M params) + multi-head attention MIL | Held-out CPTAC test split | **0.876** | **0.798** | [arXiv:2410.02700](https://arxiv.org/abs/2410.02700) |
| **4** | **TITAN** | Mahmood Lab / Harvard | Multimodal whole-slide FM with text-guided visual representations | Held-out CPTAC molecular prediction tasks | **0.865** | **0.789** | [Nature Medicine (2025)](https://doi.org/10.1038/s41591-024-03413-3) |
| **5** | **Standard ResNet-50 + CLAM** | Independent Baseline | ResNet-50 pretrained on ImageNet + CLAM single-branch aggregation | 5-Fold Cross-Validation baseline comparator | **0.802** | **0.718** | [Nature Biomedical Engineering 5 (2021)](https://doi.org/10.1038/s41551-020-00682-w) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full CPTAC whole-slide pathology archive: $\approx 1.5\text{--}2.2\text{ TB}$ of SVS images.
  - Extracted tile embedding libraries (float16) require $\approx 40\text{--}80\text{ GB}$ storage.
  - Slide-level evaluation can run efficiently on a single **16 GB VRAM** GPU (e.g., RTX 4080 / RTX 3090).
- **Minimal Local Verification / Load Command:**
```python
# Requirements: pip install openslide-python Pillow
import openslide

# Open a diagnostic SVS slide from CPTAC-3 (e.g., CPTAC-LUAD)
cptac_slide_path = "cptac_luad_diagnostic.svs"
slide = openslide.OpenSlide(cptac_slide_path)

print(f"Slide Dimensions: {slide.dimensions}")
print(f"Available Levels: {slide.level_count}")
print(f"Downsample Rates: {slide.level_downsamples}")
print(f"Vendor / Format:  {slide.properties.get('openslide.vendor', 'Unknown')}")

# Extract a 256x256 patch at 20x optical zoom (level 0 or level 1 depending on scanner MPP)
patch = slide.read_region((5000, 5000), level=0, size=(256, 256)).convert("RGB")
print(f"Extracted Patch Size: {patch.size}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Cryosection Freezing Artifacts:* Because tissue was prioritized for immediate flash freezing to preserve phosphorylation states, companion frozen histology slides frequently exhibit severe ice-crystal holes, nuclear shriveling, and detachment. Models must be trained with artifact augmentations or filtered to FFPE sections.
  2. *Data Commons ID Disconnection:* A primary operational pitfall is cross-commons linkage: GDC uses `case_id` / `sample_id`, PDC uses `study_id` / `aliquot_id`, and TCIA uses `PatientID`. Researchers must use official CPTAC master mapping manifests to avoid misaligning histology slides with proteomic readouts.
  3. *mRNA vs. Protein Decoupling:* Protein expression only correlates moderately with mRNA transcript levels ($r \approx 0.4\text{--}0.6$ on average across cancers). Training models to predict RNA-seq signatures will fail to reflect actual proteomic and phosphoproteomic enzymatic states.

---

## 5. Downstream Foundation Model Consumers
The following foundation model systems utilize CPTAC as an external evaluation or cross-modal transfer benchmark:
- **Virchow2** (`docs/02_models/04_pathology/virchow2.md`): External cancer subtyping and biomarker validation.
- **Prov-GigaPath** (`docs/02_models/04_pathology/prov_gigapath.md`): Cross-cohort transfer evaluation.
- **UNI2-h** (`docs/02_models/04_pathology/uni2_h.md`): Proteogenomic prediction benchmarking.
- **TITAN** (`docs/02_models/04_pathology/titan.md`): Multimodal histology-phenotype association.
- **CARE** (`[S90]`): Adaptive region cross-modal RNA/protein alignment benchmark.
