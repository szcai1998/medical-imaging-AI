# EchoNet-Dynamic: Video-Based Echocardiography Benchmark for Cardiac Function

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D-ECHO]` / EchoNet-Dynamic
- **Domain & Modality:** 2D B-mode Transthoracic Echocardiography (TTE) Video Cines (Apical 4-Chamber view, A4C)
- **Target Anatomy & Pathologies:** Left Ventricle (LV) myocardium and cavity; Left Ventricular Ejection Fraction (LVEF), End-Systolic Volume (ESV), End-Diastolic Volume (EDV), Heart Failure with Preserved/Reduced Ejection Fraction (HFpEF / HFrEF)
- **Release / Publication Year:** 2020 (*Nature* 580, 252–256)
- **Evidence Code:** `E1` (Peer-Reviewed *Nature* Version-of-Record) + `E2` (Stanford AIMI Official Dataset Portal)
- **Access Level:** Open research access via Stanford AIMI Data Sharing Agreement
- **Primary Source / Portal:** [https://echonet.github.io/dynamic/](https://echonet.github.io/dynamic/) | Stanford AIMI: [aimi.stanford.edu/datasets/echonet-dynamic](https://aimi.stanford.edu/datasets/echonet-dynamic)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Left Ventricular Ejection Fraction (LVEF) is the primary quantitative biomarker for assessing cardiac systolic performance, directing heart failure therapy, monitoring chemotherapy cardiotoxicity, and qualifying patients for implantable cardioverter-defibrillators (ICDs). Standard clinical biplane Simpson's rule tracing suffers from 8–12% inter-observer variability and high inter-beat variability due to manual selection of end-systolic and end-diastolic frames.
- **Task Formulation:** Beat-to-beat video regression of continuous LVEF (%), end-diastolic volume (EDV in mL), and end-systolic volume (ESV in mL), combined with frame-level semantic segmentation of the LV endocardial boundary.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **10,030 apical-4-chamber echocardiogram videos** from **10,030 unique patients** collected at Stanford Hospital between 2016 and 2018.
  - **Standard Split:** 7,465 training videos, 1,288 validation videos, and **1,277 held-out test videos**.
  - **Frame Volume:** Videos average 176 frames per acquisition at 50 frames/sec (~1.8 million total cardiac video frames).
- **Imaging Physics & Acquisition Constraints:**
  - Acquired using standard Philips and GE sector ultrasound probes (2.0–4.0 MHz phased-array transducers).
  - Native spatial resolution: $112 \times 112$ downsampled video crops (or $400 \times 400$ DICOM source frames) centered on the apical 4-chamber view.
  - Inherent physical degradation: Acoustic shadow artifacts from ribs, near-field clutter, ultrasound speckle noise, lateral beam spread, and signal dropout at the lateral myocardial wall.

### [T] Truth & Annotation Provenance
- **Sonographer Expert Tracings:** Human expert sonographers traced the endocardial border of the left ventricle at end-systole (ES) and end-diastole (ED) under the supervision of board-certified cardiologists.
- **Clinical Ground Truth:** LVEF, ESV, and EDV values extracted from the verified institutional echocardiography reporting system, representing board-certified cardiologist consensus measurements.

### [A] Access, Terms & Artifacts
- **Repository / Download:** Stanford AIMI Shared Datasets ([https://aimi.stanford.edu/datasets/echonet-dynamic](https://aimi.stanford.edu/datasets/echonet-dynamic)).
- **License / Terms:** Stanford AIMI Research Data Use Agreement (academic, non-commercial use).
- **Artifact Availability:** Preprocessed AVI video files, CSV metadata with LVEF/ESV/EDV, frame numbers for ED/ES, and polygonal coordinates for endocardial tracings.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Pioneered open video-level echocardiography benchmarking. Expanded into EchoNet-Pediatric (congenital heart disease) and EchoNet-LVH (left ventricular hypertrophy).
- **Lineage Contrast with Cedars-Sinai Lineage:** EchoNet-Dynamic is a single-view (A4C), single-institution benchmark. In contrast, multi-view foundation models like **EchoCLIP** (`[S84]`, 1.03M video-text pairs / 99k patients) and **EchoPrime** (`[S83]`, 12.12M videos / 108k patients) derive from Cedars-Sinai multi-view archives (A4C, A2C, PLAX, PSAX) and use EchoNet-Dynamic strictly as an external validation cohort.
- **Contamination & Overlap Warning:** Foundation models pretrained on public video ultrasound corpora often ingest EchoNet-Dynamic. Zero-shot or few-shot LVEF claims must verify whether Stanford AIMI data was included in pretraining.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official EchoNet-Dynamic held-out test set ($N=1,277$ unique patient videos). Evaluated using Mean Absolute Error (MAE in %) for LVEF, Coefficient of Determination ($R^2$), and Left Ventricular Endocardial Dice Similarity Coefficient (DSC).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | LVEF MAE ($\downarrow$) | $R^2$ ($\uparrow$) | LV Dice ($\uparrow$) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **EFNet** | Li et al. | Dual-branch spatiotemporal CNN integrating beat-to-beat cycle attention with adaptive frame selection | Official Blind Test Set ($N=1,277$) | **3.70%** | **0.820** | **0.931** | [ResearchGate 2023](https://doi.org/10.1016/j.media.2023.102830) |
| **2** | **FDS-Net** | Wang et al. (Tsinghua Univ.) | Frequency-domain spatiotemporal network decomposing cardiac cycle harmonics from speckle noise | Official Blind Test Set ($N=1,277$) | **3.75%** | **0.825** | **0.932** | [SciOpen 2024](https://doi.org/10.26599/TST.2024.9240012) |
| **3** | **EchoCoTr** | Arnaout et al. | Spatiotemporal co-attention transformer coupling multi-frame temporal attention with LV shape priors | Official Blind Test Set ($N=1,277$) | **3.95%** | **0.820** | **0.928** | [arXiv:2303.07684](https://arxiv.org/abs/2303.07684) |
| **4** | **EchoPrime (Zero-Shot)** | Vukadinovic et al. (Cedars-Sinai) | Multi-view video-language foundation model pretrained on 12.1M Cedars-Sinai videos; zero-shot linear probe | External Validation on EchoNet Test ($N=1,277$) | **4.01%** | **0.812** | — | [Nature 2026](https://doi.org/10.1038/s41586-025-09850-x) |
| **5** | **EchoNet R(2+1)D Baseline** | Ouyang et al. (Stanford AIMI) | 3D spatiotemporal ResNet with factorized space-time convolutions R(2+1)D and periodic temporal pooling | Official Blind Test Set ($N=1,277$) | **4.10%** | **0.801** | **0.920** | [Nature 2020](https://doi.org/10.1038/s41586-020-2145-8) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Full dataset size: ~35 GB compressed video archive.
  - Video batch processing: Single-video 3D ConvNet/Transformer inference requires **8–12 GB VRAM** (RTX 3060/4070). Training with 32–64 frame clips at batch size 8–16 requires **24 GB VRAM** (RTX 3090/4090/A5000).
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install opencv-python pandas numpy
import cv2
import pandas as pd
import numpy as np

df = pd.read_csv("EchoNet-Dynamic/FileList.csv")
print(f"Total entries: {len(df)}, Splits: {df['Split'].value_counts().to_dict()}")

sample_file = df.iloc[0]["FileName"] + ".avi"
cap = cv2.VideoCapture(f"EchoNet-Dynamic/Videos/{sample_file}")
frame_count = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))
fps = cap.get(cv2.CAP_PROP_FPS)
width, height = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)), int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
cap.release()

print(f"Video: {sample_file} | Frames: {frame_count} @ {fps:.1f} FPS | Dimensions: {width}x{height}")
print(f"Reported LVEF: {df.iloc[0]['EF']}%, ESV: {df.iloc[0]['ESV']} mL, EDV: {df.iloc[0]['EDV']} mL")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Apical Foreshortening:* When the ultrasound probe is placed too high on the chest wall, the true LV apex is cut off, artificially shortening the vertical axis and producing significant LVEF overestimation.
  2. *Lateral Endocardial Dropout:* Due to acoustic beam physics, tissue interfaces parallel to ultrasound beam lines (such as the lateral LV wall) suffer acoustic dropout, leading segmentation models to hallucinate borders into the pericardium.

---

## 5. Downstream Foundation Model Consumers
The following foundation models and cardiac frameworks utilize EchoNet-Dynamic:
- **EchoPrime** (`docs/02_models/05_specialty/echoprime.md` / `[S83]`): Nature 2026 multi-view echocardiography video-language FM evaluated on EchoNet-Dynamic as its primary external single-view benchmark.
- **EchoCLIP** (`[S84]`): Nature Medicine 2024 vision-language model for contrastive echocardiogram interpretation.
- **Pan-Cardiac Video Transformers**: Multi-beat cardiac spatio-temporal dynamics research.
