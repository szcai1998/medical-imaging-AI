# fastMRI: Raw Measurement-Domain k-Space & Accelerated MRI Reconstruction Benchmark

## 1. Quick Metadata & Classification
- **Canonical Dossier ID:** `[D24]`
- **Domain & Modality:** Raw Measurement k-Space (Cartesian 2D/3D Complex Fourier Domain) & Multi-Coil / Single-Coil Clinical MRI (Knee and Brain)
- **Target Anatomy & Pathologies:** Knee (meniscal tears, ligamentous tears, cartilage defects) & Brain (intracranial tumors, stroke, multiple sclerosis lesions, acoustic neuroma)
- **Release / Challenge Year:** 2018 (Initial Knee Dataset), 2019 (fastMRI Knee Challenge / Knoll et al.), 2020 (Brain Dataset & fastMRI Brain Challenge / Muckley et al.), 2021 (IEEE TMI Challenge Report)
- **Evidence Code:** `E2` (Official Challenge & Dataset Portal) + `E1` (Peer-Reviewed Challenge Proceedings in *Radiology: AI* & *IEEE TMI*)
- **Access Level:** Research-Gated Open Access via application at [fastmri.med.nyu.edu](https://fastmri.med.nyu.edu/)
- **Primary Source / Portal:** [https://fastmri.med.nyu.edu/](https://fastmri.med.nyu.edu/) | Code: [https://github.com/facebookresearch/fastMRI](https://github.com/facebookresearch/fastMRI)

---

## 2. D-A-T-A-S Snapshot

### [D] Domain & Clinical Target
- **Clinical Motivation:** Clinical MRI is inherently time-constrained. Scanning a patient typically requires 30–60 minutes, leading to severe motion artifacts, patient discomfort, limited scanner throughput, and high healthcare costs. Retrospectively or prospectively undersampling frequency data ($k$-space) accelerates scans by $4\times$ to $8\times$, but violates the Nyquist-Shannon sampling theorem, causing severe aliasing artifacts in image space.
- **Inverse Problem Formulation:** The reconstruction task recovers a 2D/3D spatial MR image $x \in \mathbb{C}^N$ from undersampled multi-channel frequency measurements $y = \mathcal{M} \mathcal{F} \mathcal{S} x + \epsilon$, where $\mathcal{S}$ represents coil sensitivity maps, $\mathcal{F}$ is the discrete Fourier transform, $\mathcal{M}$ is the undersampling trajectory mask, and $\epsilon$ is receiver noise.

### [A] Acquisition Physics & Scale
- **Exact Counting Units:**
  - **Knee Raw k-Space Cohort:** **1,598 raw k-space examinations** (1,172 multi-coil train, 199 multi-coil val, 108 multi-coil test, 119 multi-coil challenge test; plus corresponding single-coil pairs).
  - **Knee Clinical DICOM Cohort:** **10,000 clinical knee MRI examinations** (>1,500,000 DICOM image slices).
  - **Brain Raw k-Space Cohort:** **6,970 raw k-space examinations** (4,469 multi-coil train, 1,378 multi-coil val, 558 multi-coil test, 565 challenge test; covering T1, T1+contrast, T2, and FLAIR contrast sequences).
- **Imaging Physics & Scanner Distribution:**
  - Acquired on clinical 1.5T and 3.0T scanners (Siemens Magnetom Prisma, Skyra, Aera, Biograph mMR) at NYU Langone Health.
  - Multi-channel receiver coil arrays: 15-channel knee coils and 16-to-32-channel head coils.
  - Formats: High-precision complex 32-bit floating point arrays stored in HDF5 (`.h5`) format containing raw multi-coil $k$-space $(N_{\text{slices}}, N_{\text{coils}}, N_y, N_x)$, sensitivity maps, and acquisition metadata.

### [T] Truth & Annotation Provenance
- **Physical Ground Truth:** Fully sampled Cartesian acquisitions serve as the gold-standard physical ground truth ($S = \mathcal{F}^{-1}(k)$). Acceleration is simulated by applying standardized Cartesian undersampling masks ($4\times$ and $8\times$ acceleration with central auto-calibration signal [ACS] lines preserved).
- **Radiologist Evaluation Panel:** Top challenge submissions underwent blinded evaluation by expert musculoskeletal and neuroradiologists scoring diagnostic quality, edge sharpness, contrast-to-noise ratio, and artifact severity on 4-point and 5-point Likert scales.

### [A] Access, Terms & Artifacts
- **Repository / Download:** NYU Langone fastMRI data portal ([https://fastmri.med.nyu.edu/](https://fastmri.med.nyu.edu/)).
- **License / Terms:** NYU FastMRI Data Sharing Agreement. Free for academic, non-commercial research; redistribution and re-identification strictly prohibited.
- **Artifact Availability:** Official PyTorch `fastmri` package (`pip install fastmri`), reference unrolled pipelines (E2E-VarNet, MoDL, U-Net), evaluation scripts for SSIM, PSNR, and NMSE.

### [S] Systemic Lineage & Genealogy
- **Parent / Predecessor:** Pioneered large-scale open raw $k$-space benchmarking, establishing the first standardized open inverse-problem ecosystem in medical imaging.
- **Derivative Ecosystem:** Inspired Stanford SKM-TEA (Knee MRI with tissue labels), Calgary-Campinas (CC-359), and fastMRI prostate extensions.
- **Contamination & Overlap Warning:** Foundation models and general inverse-problem restoration encoders (e.g., UniFMIR, SwinIR-MRI, Med-JEPA) almost universally use fastMRI knee or brain data for pretraining. Any paper claiming "zero-shot MRI reconstruction" on knee or brain MRI must explicitly verify whether the weights were exposed to fastMRI.

---

## 3. Verified SOTA Leaderboard (Top 5 Rank)
*Standings on the official fastMRI Multi-Coil Brain Challenge held-out test evaluation ($8\times$ acceleration track, Muckley et al., IEEE TMI 2021 [E1+E2]). Evaluated using Structural Similarity Index (SSIM), Peak Signal-to-Noise Ratio (PSNR in dB), and Normalized Mean Squared Error (NMSE).*

| Rank | Model / Submission | Team / Affiliation | Core Architecture & Strategy | Evaluation Split & Setting | SSIM ($\uparrow$) | PSNR (dB $\uparrow$) | NMSE ($\downarrow$) | Reference / Links |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **1** | **AIRS Medical Unrolled Net** | AIRS Medical / Seoul National Univ. | Multi-stage unrolled data-consistency network with complex U-Net image refiners and coil sensitivity calibration | Official Blind Test Set ($8\times$ multi-coil brain, $N=565$) | **0.942** | **38.65** | **0.0152** | [IEEE TMI 2021](https://doi.org/10.1109/TMI.2021.3075837) |
| **2** | **XPDNet** | Zaccharie Ramzi et al. (NeuroSpin / CEA / Inria) | Unrolled Primal-Dual Hybrid Gradient (PDHG) algorithm with multi-scale wavelet (MWCNN) spatial prior | Official Blind Test Set ($8\times$ multi-coil brain, $N=565$) | **0.938** | **38.12** | **0.0165** | [IEEE TMI 2021](https://doi.org/10.1109/TMI.2021.3075837) |
| **3** | **E2E-VarNet** | Anuroop Sriram et al. (FAIR / NYU Langone) | End-to-End Variational Network with learned sensitivity maps and unrolled gradient descent steps | Official Blind Test Set ($8\times$ multi-coil brain, $N=565$) | **0.934** | **37.89** | **0.0178** | [MICCAI 2020](https://arxiv.org/abs/2004.06688) |
| **4** | **i-RIM** | Patrick Putzky et al. (Amsterdam UMC / Philips) | Iterative Recurrent Inference Machine with learned recurrent spatial-frequency updates | Official Blind Test Set ($8\times$ multi-coil brain, $N=565$) | **0.929** | **37.31** | **0.0194** | [IEEE TMI 2021](https://doi.org/10.1109/TMI.2021.3075837) |
| **5** | **Standard Multi-Coil U-Net** | fastMRI Organizers (NYU / FAIR) | Deep 2D convolutional U-Net applied to root-sum-of-squares (RSS) initial zero-filled inverse FFT | Official Blind Test Set ($8\times$ multi-coil brain, $N=565$) | **0.884** | **33.45** | **0.0382** | [Radiology: AI 2020](https://doi.org/10.1148/ryai.2020200006) |

---

## 4. Ground-Layer Practical Guidance
- **Workstation Compute Feasibility:**
  - Storage: Full knee k-space is ~1.5 TB; brain k-space exceeds 4.5 TB. Downloading a single validation subset requires 150–300 GB NVMe storage.
  - VRAM Footprint: Complex unrolled gradient models (e.g., 12-cascade VarNet) process multi-coil tensors $(B, C, H, W)$ with $C=16\text{ coils}$, requiring a minimum of **24 GB VRAM** (RTX 3090/4090/A5000) for batch size 1, or **48–80 GB** (A6000/A100) for full volumetric training.
- **Minimal Local Verification / Load Command:**
  ```python
# Requirements: pip install h5py numpy
import h5py
import numpy as np

with h5py.File("knee_sample.h5", "r") as hf:
    kspace = hf["kspace"][()]  # Shape: (slices, coils, height, width)
    attrs = dict(hf.attrs)
    print(f"k-space shape: {kspace.shape}, dtype: {kspace.dtype}")
    print(f"Acquisition: {attrs.get('acquisition')}, Max value: {attrs.get('max')}")

    # Physical reconstruction of central slice via Inverse 2D FFT
    slice_k = kspace[kspace.shape[0] // 2]
    slice_img = np.fft.ifftshift(np.fft.ifft2(np.fft.fftshift(slice_k, axes=(-2, -1)), axes=(-2, -1)), axes=(-2, -1))
    rss_recon = np.sqrt(np.sum(np.abs(slice_img) ** 2, axis=0))
    print(f"Reconstructed slice RSS shape: {rss_recon.shape}")
```
- **Dominant Failure Modes & Gotchas:**
  1. *Hallucination of Fine Anatomical Structures:* Purely image-based networks without rigorous measurement-domain data consistency (DC) layers hallucinate realistic trabecular bone patterns or vessel bifurcations not supported by $k$-space.
  2. *RSS vs. Phase Cancellation:* Root-sum-of-squares (RSS) combination ignores coil phase relationships. Models must operate in complex multi-coil space or employ explicit sensitivity map estimation (ESPIRiT).

---

## 5. Downstream Foundation Model Consumers
The following models and clinical translation pipelines utilize fastMRI as a core training or inverse-imaging benchmark:
- **UniFMIR** (`docs/02_models/05_specialty/unifmir.md`): Universal medical image restoration foundation model for MRI super-resolution and de-aliasing.
- **Vision-Language-Guided fastMRI** (`[S42]`): Multimodal prior framework guiding inverse reconstruction.
- **AIRS Medical SwiftMR**: Commercial FDA-cleared and CE-marked accelerated MRI reconstruction software derived from the winning 2020 fastMRI architecture.
- **Med-JEPA / V-JEPA MRI Modules**: Joint-embedding predictive architectures evaluated on subsampled $k$-space representation.
