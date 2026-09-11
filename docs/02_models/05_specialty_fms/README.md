# 05: Specialty Modalities & Frontier Foundation Models

Welcome to the **Specialty Modalities & Frontier Foundation Model Library**. This module documents evidence-audited foundation architectures operating beyond conventional volumetric radiology (CT/MRI/CXR). It captures the cutting edge across six high-impact clinical and physical domains: **Tomographic Inverse-Problem Reconstruction**, **Multi-View Echocardiography Video**, **3D Retinal Optical Coherence Tomography**, **Longitudinal Oculomic Phenomics**, **Multimodal Dermatology**, **Digital Stomatology & Dental VLMs**, and **Nanoscale Fluorescence Microscopy Restoration**.

Every model card in this directory strictly adheres to the repository's canonical **Hardened P-A-B-H-A Schema**, detailing the clinical bottleneck, architectural core, verified benchmark standings with explicit evaluation splits, ground-layer epistemic audits, and runnable production verification contracts.

---

## 1. Master Comparative Foundation Model Matrix

| Canonical ID | System / Architecture | System Class | Scope | Modality & Clinical Domain | Developing Institution | Evidence Code | Artifact Tier | Benchmark SOTA Metric | Model Card |
|:---:|---|:---:|:---:|---|---|:---:|:---:|---|:---:|
| **`[M27A]`** | **EchoPrime & EchoCLIP** | `core_fm` | `modality_generalist` | Multi-View Echocardiography Video (Hemodynamics & Valvular Disease) | Cedars-Sinai & Stanford AIMI | `[E1]` *Nature* 2026 / *Nat Med* 2024 | **Tier A** | LVEF MAE **3.42%**; Macro AUROC **0.884** | [Card](./echoprime_echoclip.md) |
| **`[M26]`** | **OCTCube-M** | `core_fm` | `modality_generalist` | 3D Volumetric Retinal OCT, cSLO & En-Face (Macular Disease & GA) | Washington Univ & Tsinghua Univ | `[E1]` *Nat Biomed Eng* 2026 | **Tier B** | Retinal Macro AUROC **0.962**; GA $R^2$ **0.741** | [Card](./octcube_m.md) |
| **`[M26]`** | **RETFound & RETFound+** | `core_fm` | `modality_generalist` | Color Fundus & Longitudinal Retinal Imaging (Oculomics & Survival) | Moorfields Eye Hospital & UCL | `[E1]` *Nature* 2023 / *npj Dig Med* 2026 | **Tier A** | 5-Yr Incident Stroke C-index **0.782** | [Card](./retfound_plus.md) |
| **`[M28C]`** | **PanDerm** | `core_fm` | `specialty_generalist` | Multimodal Dermatology (TBP Tiles, Dermoscopy, Clinical, Dermpath) | Monash Univ & Univ of Queensland | `[E1]` *Nat Med* 2025/2026 | **Tier A** | 28-Benchmark Macro AUROC **0.938** | [Card](./panderm.md) |
| **`[M28E]`** | **PanoFM & DentVLM** | `core_fm` / `fm_derived` | `modality_gen` / `specialty_gen` | Dental Panoramic Radiographs & 7 Oral Modalities (Caries, Perio, VQA) | Wuhan Univ, West China, SJTU | `[E1]` *Pattern Recog* / *Nat Comm* 2026 | **Tier B** | Panoramic F1 **0.832**; 36-Task Acc **84.6%** | [Card](./panofm_dentvlm.md) |
| **`[M23]`** | **DeepSparse** | `core_fm` | `workflow_specialist` | Sparse-View CBCT Projection Reconstruction (Dose Reduction) | HKUST (xmed-lab) & Sichuan Univ | `[E1]` *IEEE TMI* 2026 | **Tier A** | 50-View PSNR **37.84 dB** / SSIM **0.958** | [Card](./deepsparse.md) |
| **`[M28D]`** | **UniFMIR & FluoResFM** | `core_fm` | `modality_generalist` | Multi-Modality Fluorescence Microscopy (Denoising, Deconv, 3D SR) | Fudan Univ & HUST | `[E1]` *Nat Methods* 2024 / *Nat Comm* 2026 | **Tier A** | Denoising PSNR **36.42 dB**; $+2.8\text{ dB}$ OOD | [Card](./unifmir.md) |
| **`[S105]`** | **HorusEye** | `core_fm` | `modality_generalist` | Universal X-ray Tomography Restoration (Synchrotron, Micro-CT, LDCT) | KAUST, HIT & CAS | `[E1]` *Nat Comput Sci* 2026 | **Tier A** | LDCT PSNR **38.15 dB**; De-Ringing CNR $+84\%$ | [Card](./horuseye.md) |

---

## 2. Specialty Imaging Physics & Inverse Problem Formulations

Unlike computer vision on photographic RGB images, specialty medical foundation models interface directly with the **physical signal acquisition layer**. Violating physical constraints inevitably produces catastrophic diagnostic hallucinations.

``` text
┌─────────────────────────────────────────────────────────────────────────────┐
│                    SPECIALTY IMAGING FORWARD MODELS                         │
├─────────────────────────────────────────────────────────────────────────────┤
│ 1. Tomographic Transmission   │ p(θ, s) = ∫∫ f(x, y) δ(x cosθ + y sinθ - s) │
│    (Radon Transform / CBCT)   │ Line integrals of X-ray attenuation μ(x, y) │
├────────────────────────────────┼─────────────────────────────────────────────┤
│ 2. Magnetic Resonance (MRI)   │ S(k) = ∫∫ m(x, y) exp(-i 2π k · r) dr       │
│    (Fourier k-Space Encoding) │ Spatial frequency samples via gradient coils│
├────────────────────────────────┼─────────────────────────────────────────────┤
│ 3. Optical Coherence (OCT)    │ I(k) = 2 S_0(k) ∫ r(z) cos(2 k z) dz        │
│    (Low-Coherence Interfero)  │ Depth-resolved backscatter cross-sections   │
├────────────────────────────────┼─────────────────────────────────────────────┤
│ 4. Acoustic Doppler Echo      │ Δf = (2 f_0 v cosθ) / c                     │
│    (Pulse-Echo Ultrasound)    │ Hemodynamic blood velocity shift            │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1. Transmission Tomography & Radon Inversion (DeepSparse, HorusEye)
In transmission X-ray computed tomography, the forward projection models line integrals of the spatial attenuation distribution $f(x, y)$ via the 2D/3D Radon transform $\mathcal{R}$:
$$\mathcal{R}[f](\theta, s) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x, y) \delta(x \cos\theta + y \sin\theta - s) \, dx \, dy$$
The **Fourier Slice Theorem** proves that the 1D Fourier transform of a parallel projection at angle $\theta$ equals a radial slice through the 2D Fourier transform of the object:
$$\mathcal{F}_1\{\mathcal{R}[f](\theta, \cdot)\}(\omega) = \mathcal{F}_2\{f\}(\omega \cos\theta, \omega \sin\theta)$$
When the angular sampling rate $\Delta\theta$ violates Nyquist-Shannon limits (as in 20-to-50-view ultra-sparse CBCT), classical filtered backprojection (FDK) fails completely. **DeepSparse (`[M23]`)** solves this ill-posed inverse problem by interleaving deep spatial feature extraction with differentiable projection and backprojection unrolling operators $\mathbf{A}^T (\mathbf{A} x - y)$, mathematically guaranteeing **Data Consistency (DC)** against physical measurements. **HorusEye (`[S105]`)** acts complementarily in the reconstructed domain ($x$-space), using self-supervised interslice axial contrast to extinguish detector ring artifacts and quantum noise without requiring paired radiation scans.

### 2. Low-Coherence Interferometry in Retinal OCT (OCTCube-M, RETFound Plus)
Optical Coherence Tomography (OCT) measures the echo time delay and magnitude of backscattered light using a low-coherence Michelson or Mach-Zehnder interferometer. In Spectral-Domain OCT (SD-OCT), the interference spectrum $I(k)$ is detected by a high-speed spectrometer:
$$I(k) = S_0(k) \left[ R_r + \int_0^\infty r_s(z) \, dz + 2 \sqrt{R_r} \int_0^\infty r_s(z) \cos(2 k z) \, dz \right]$$
Taking the inverse Fourier transform of $I(k)$ with respect to wavenumber $k$ resolves depth-resolved reflectivity profiles ($A$-scans). Collecting linear arrays of $A$-scans forms $B$-scans, and stacking $B$-scans yields a 3D interference volume ($512 \times 496 \times 64$ to $1024 \times 512 \times 128$). **OCTCube-M (`[M26]`)** establishes that flattening this interferometric volume into a single 2D central slice discards the sub-retinal fluid topology and geographic atrophy expansion frontiers.

### 3. Acoustic Beamforming & Hemodynamics in Echocardiography (EchoPrime, EchoCLIP)
Ultrasound transducers emit high-frequency acoustic pulses ($2\text{--}5\text{ MHz}$) and reconstruct B-mode brightness cines via dynamic receive beamforming. Blood flow velocities ($v$) are acquired via the Doppler frequency shift $\Delta f$:
$$\Delta f = \frac{2 f_0 v \cos\theta}{c}$$
where $f_0$ is transducer frequency, $\theta$ is the Doppler insonation angle, and $c \approx 1540\text{ m/s}$ is acoustic velocity in soft tissue. **EchoPrime (`[M27A]`)** bridges visual wall motion with multi-view acoustic geometry, learning to integrate parasternal, apical, and subcostal windows into a unified multi-chamber assessment.

---

## 3. Video & Temporal Dynamics in Medical AI

Specialty modalities often unfold across **time**—either within fractions of a second during a cardiac cycle, or across decades of longitudinal patient monitoring.

``` text
┌─────────────────────────────────────────────────────────────────────────────┐
│                       TEMPORAL HIERARCHY & LEAKAGE                          │
├─────────────────────────────────────────────────────────────────────────────┤
│ 1. Sub-Second Video Dynamics   │ Cardiac cines (30-60 fps); consecutive      │
│    (Echocardiography Video)    │ frames share 98% speckle texture.           │
│                                │ RISK: Random frame split = 100% leakage.    │
├────────────────────────────────┼─────────────────────────────────────────────┤
│ 2. Multi-Year Disease Trajectory│ Fundus/OCT follow-up visits (Δt = 1-5 yrs);│
│    (Longitudinal Phenomics)    │ progressive microvascular attrition vs.    │
│                                │ stationary biological patient baseline.     │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1. Dense Frame Leakage in Video Ultrasound (EchoPrime, EchoCLIP)
An echocardiographic cine contains 30 to 100 consecutive frames sampled milliseconds apart. Dividing video frames randomly between training and testing creates catastrophic temporal data leakage: the model memorizes stationary rib shadows and ultrasound gain settings rather than myocardial contractility. **Splits must strictly be enforced at the patient encounter level.**

### 2. Time-Aware Survival Modeling (RETFound Plus)
Traditional medical classification outputs a discrete probability $\hat{y} \in [0, 1]$, which is meaningless for future disease prediction because it ignores *when* the event will occur. **RETFound Plus (`[M26]`)** integrates continuous follow-up intervals $\Delta t$ and Cox proportional hazards:
$$h(t \mid x) = h_0(t) \exp\left( \mathbf{w}^T \mathbf{z}_{\text{retina}} \right)$$
allowing clinicians to estimate personalized 3-year and 5-year survival curves for incident stroke and myocardial infarction from a single eye examination.

---

## 4. Non-Radiology Clinical Translation & Governance

Deploying foundation models in dental clinics, dermatology triage kiosks, and point-of-care ultrasound environments introduces unique regulatory and operational boundaries:

### 1. The Bilateral Organ Correlation Trap
In ophthalmology (eyes), dermatology (bilateral limbs), and dentistry (left/right jaw quadrants), anatomy exhibits strong genetic and systemic symmetry. Placing a patient's right eye (OD) in training and left eye (OS) in testing leaks blood pressure, glucose control, and genetic risk, generating fabricated out-of-distribution generalization claims.

### 2. Edge Hardware & Point-of-Care Deployment
Unlike high-throughput radiology PACS backed by hospital datacenters, specialty AI often operates on edge hardware:
- **Dental Clinics**: PanoFM and DentVLM must execute on chairside dental PCs (RTX 3060/4060 or quantized INT4/AWQ configurations).
- **Dermatology Triage**: PanDerm must support rapid smartphone/tablet inference ($<200\text{ ms}$) in primary care clinics.
- **Handheld Ultrasound**: Point-of-care cardiac probes require low-power FP16 inference for instantaneous ejection fraction feedback.

---

## 5. Connected Dataset Ecosystem

Every model card in this directory directly connects to our verified benchmark datasets in `docs/01_datasets/05_specialty/`:

- 📂 [fastMRI (`[D24]`)](../../01_datasets/05_specialty/fastmri.md): Raw $k$-space inverse imaging benchmark paired with DeepSparse and MRI reconstruction models.
- 📂 [EchoNet-Dynamic (`[D-ECHO]`)](../../01_datasets/05_specialty/echonet_dynamic.md): 10,030 video echocardiography benchmark connected to EchoPrime and EchoCLIP.
- 📂 [OCTCube & RETFound Cohorts (`[D-RETINA]`)](../../01_datasets/05_specialty/octcube_retfound_cohorts.md): 3D OCT volumes and longitudinal fundus cohorts connected to OCTCube-M and RETFound Plus.
- 📂 [PanDerm Corpus (`[D-DERM]`)](../../01_datasets/05_specialty/panderm_corpus.md): 2.15M multimodal dermatology images connected to PanDerm.
- 📂 [Dental Panoramic Cohorts (`[D-DENT]`)](../../01_datasets/05_specialty/dental_panoramic_cohorts.md): 127k panoramic radiographs and multimodal oral datasets connected to PanoFM and DentVLM.
- 📂 [GastroNet-5M (`[D-ENDO]`)](../../01_datasets/05_specialty/gastro_net_5m.md): Endoscopic video frame pretraining corpus.
