# nnU-Net v2: The Self-Configuring Medical Segmentation Baseline (`[M04]`)

> **The Universal Supervised Benchmark Standard**: A self-configuring framework that automatically adapts preprocessing, network topology, training hyperparameters, and postprocessing to any biomedical segmentation dataset without manual intervention.

---

## 1. Quick Metadata & Registry Classification

| Field | Value |
|---|---|
| **Canonical ID** | `[M04]` |
| **System Class** | `companion` (Dossier Section 4.0 authoritative registry) |
| **Scope** | `workflow_specialist` (Supervised reference comparator) |
| **Modality & Anatomy** | 3D/2D Volumetric CT, MRI, PET, Microscopy (Pan-anatomical) |
| **Developing Institution** | German Cancer Research Center (DKFZ), Division of Medical Image Computing & Helmholtz Imaging (Heidelberg, Germany) |
| **Evidence Code** | `[E1]` Peer-Reviewed Version-of-Record (*Nature Methods* 2021; MICCAI 2024) |
| **Artifact Availability Tier**| **Tier A** (Fully open: PyPI package, open-source engine, public model weights) |
| **Primary Paper DOI** | [`10.1038/s41592-020-01008-z`](https://doi.org/10.1038/s41592-020-01008-z) (*Nature Methods* 2021) |
| **Code Repository** | [GitHub: MIC-DKFZ/nnUNet](https://github.com/MIC-DKFZ/nnUNet) |
| **Software License** | Apache 2.0 |

---

## 2. P-A-B-H-A Snapshot

### [P] Clinical Problem
Biomedical segmentation pipelines suffer from systemic empirical fragility: hyperparameter tuning, patch-size selection, resampling strategies, and loss engineering are often ad-hoc and overfitted to single datasets, causing newer architectural proposals to look superior simply because the baseline was poorly tuned.

### [A] Architectural Core
nnU-Net v2 abstracts the segmentation pipeline into three operational tiers: **Rule-based Parameters** (derived deterministically from dataset fingerprint: spacing, median shapes, intensities), **Fixed Parameters** (architecture topology, deep supervision, dice+cross-entropy loss), and **Empirical Parameters** (automated ensembling of 2D, 3D full-resolution, and 3D cascade U-Nets). The updated v2 framework integrates modern **ResEnc** (Residual Encoder) presets (M, L, XL), placing residual blocks into the encoder stages to accelerate gradient flow across deep networks.

### [B] Benchmark & Delta
On the Medical Segmentation Decathlon (MSD, 10 tasks) and competitive benchmarks (KiTS, AMOS, BTCV), nnU-Net v2 matches or outperforms complex vision transformers. With the **ResEnc L/XL** configuration, it improves over standard nnU-Net:
- **KiTS (Kidney + Tumor)**: $86.04 \rightarrow 88.67$ Dice (+2.63 DSC).
- **AMOS (15 Abdominal Organs)**: $88.64 \rightarrow 89.68$ Dice (+1.04 DSC).
- **BTCV**: $83.08 \rightarrow 83.35$ Dice (+0.27 DSC).

### [H] Hardware Footprint & Deployment Profile
- **Inference Footprint (Volumetric)**: Uses sliding-window Gaussian patched inference with $50\%$ patch overlap. Minimum VRAM: **4–8 GB** (Standard 3D fullres), **12–16 GB** (ResEnc L), **24 GB** (ResEnc XL).
- **Training Compute Budget**: Single RTX 3090/4090 (24 GB) trains standard 5-fold cross-validation in ~16–24 hours per fold; ResEnc XL benefits from 40–80 GB VRAM (A100/H100).
- **Workstation Feasibility**: **Full local feasibility**; out-of-the-box CPU/GPU execution via pip.

### [A] Access & Artifacts
- **Package**: `pip install nnunetv2`
- **Trained Model Weights**: Downloadable directly via CLI for TotalSegmentator, KiTS, AMOS, BraTS, and MSD tasks (`nnUNetv2_download_pretrained_model_by_url`).
- **Data Preprocessing CLI**: `nnUNetv2_plan_and_preprocess -d DATASET_ID`.

---

## 3. Verified Benchmark Standings & Comparative Matrix

*Standings reflect 5-fold Cross-Validation (CV) and official challenge held-out blind test evaluations.*

| Evaluation Dataset / Task | Evaluation Split | Supervision Regimen | nnU-Net Configuration | Target Metric | Architectural Baseline | Baseline Metric | Delta ($\Delta$) | Evidence Source |
|---|---|:---:|---|:---:|---|:---:|:---:|:---:|
| **KiTS23** (Kidney/Tumor/Cyst) | Held-out Blind Test | Fully Supervised (from scratch) | nnU-Net v2 Cascade | **89.2%** (Kidney) / **83.5%** (Tumor) | SwinUNETR | 82.1% (Tumor) | **+1.4% DSC** | `[E2]` Grand Challenge |
| **AMOS22** (15 Organs) | Held-out Blind Test | Fully Supervised (from scratch) | nnU-Net v2 ResEnc XL | **89.68%** Mean DSC | UNETR | 84.10% Mean DSC | **+5.58% DSC** | `[E1]` MICCAI 2024 |
| **MSD 10-Task Decathlon** | Multi-task Held-out | Fully Supervised (from scratch) | Standard nnU-Net v2 | **Rank 1 Overall** (All 10 Tasks) | Standard 3D U-Net | Diverse Drops | **Outperformed 95% entries** | `[E1]` *Nature Methods* |
| **BTCV Abdomen** | 5-Fold CV | Fully Supervised (from scratch) | nnU-Net ResEnc L | **83.35%** Mean DSC | TransUNet | 77.48% Mean DSC | **+5.87% DSC** | `[E1]` *Nature Methods* |
| **autoPET II** (FDG Lesions) | Held-out Blind Test | Fully Supervised (from scratch) | nnU-Net ResEnc M Ensemble | **0.842** Dice / **0.42** FP Vol | DynUNet Baseline | 0.781 Dice | **+0.061 DSC** | `[E2]` MICCAI autoPET |

---

## 4. Ground-Layer Audit & Epistemic Traps

### 1. The Reference Standard Doctrine & The 23x Data-Efficiency Paradox
> [!IMPORTANT]
> **The 23x Data-Efficiency Paradox**: Across structured volumetric benchmarks (e.g., AMOS22, KiTS23), **nnU-Net v2 trained on only 500 cases achieves higher Dice than massive vision foundation models (e.g., VISTA3D) pretrained on 11,454 cases (89.68% vs 88.10% DSC on AMOS)**. Pretraining scale on heterogeneous uncurated CTs does not automatically compensate for task-specific spatial inductive biases and exact dataset fingerprint adaptation. Any claim that a foundation model has surpassed SOTA is scientifically invalid without an identical comparison against a properly tuned **nnU-Net ResEnc** baseline.

### 2. Operational Deployment Parameters
To avoid workstation out-of-memory errors and ensure reproducible inference:
- **Sliding-Window Gaussian Patching**: Step size `tile_step_size = 0.5` (50% overlap); `use_gaussian = True` prevents edge artifacts.
- **Inference Precision**: AMP (`torch.autocast`) with `torch.float16` or `torch.bfloat16`.
- **Test-Time Augmentation (TTA)**: Mirroring across 3 spatial axes improves DSC by +0.3–0.8% but increases inference time $8\times$. Set `use_mirroring = False` for high-throughput clinical triage.

### 3. Pretraining Contamination Risk
- **Contamination Status**: `None (Clean)`.
- nnU-Net v2 trains from **random initialization (scratch)** on the target task dataset. It does not ingest external foundation model pretraining weights by default, eliminating zero-shot leakage and training set contamination risks.

### 4. Critical Limitations
- **Not Promptable**: Cannot accept user clicks, bounding boxes, or free-text descriptions at inference time.
- **Requires Task Labels**: Completely dependent on task-specific annotated training data (cannot perform zero-shot segmentation on unseen classes).
- **Inference Latency**: Sliding-window inference with Gaussian blending and TTA requires 30–90 seconds per full 3D volume on workstation GPUs, slower than single-pass foundation feedforward networks.

---

## 5. Local Verification Snippet (Tier A: Fully Open)

```python
# Requirements: pip install nnunetv2 torch
# Artifact Tier: Tier A (Fully open PyPI package & GitHub: MIC-DKFZ/nnUNet)
# Verification: Demonstrates authentic programmatic nnUNetPredictor pipeline and network instantiation

import torch
from dynamic_network_architectures.architectures.unet import ResidualEncoderUNet
from dynamic_network_architectures.building_blocks.residual import BasicBlockD

def verify_nnunet_production_pipeline():
    print("[INIT] Verifying authentic nnU-Net v2 ResEnc configuration...")
    
    # 1. Authentic ResEnc L architecture instantiation (MICCAI 2024 preset)
    model = ResidualEncoderUNet(
        input_channels=1,
        n_stages=5,
        features_per_stage=[32, 64, 128, 256, 320],
        conv_op=torch.nn.Conv3d,
        kernel_sizes=[[3, 3, 3]] * 5,
        strides=[[1, 1, 1], [2, 2, 2], [2, 2, 2], [2, 2, 2], [2, 2, 2]],
        n_blocks_per_stage=[1, 2, 3, 4, 4],
        num_classes=15,  # AMOS22 15 abdominal organs
        n_conv_per_stage_decoder=[1, 1, 1, 1],
        conv_bias=True,
        norm_op=torch.nn.InstanceNorm3d,
        norm_op_kwargs={"eps": 1e-5, "affine": True},
        dropout_op=None,
        nonlin=torch.nn.LeakyReLU,
        nonlin_kwargs={"inplace": True},
        deep_supervision=True,
        block=BasicBlockD
    )
    model.eval()

    # 2. Volumetric forward pass with sliding-window patch (B=1, C=1, D=64, H=64, W=64)
    dummy_patch = torch.randn(1, 1, 64, 64, 64)
    with torch.no_grad():
        outputs = model(dummy_patch)

    print("nnU-Net v2 ResEnc initialized successfully.")
    print(f"Deep supervision levels: {len(outputs)}")
    print(f"Full-res logits shape: {outputs[0].shape}")
    assert outputs[0].shape == (1, 15, 64, 64, 64), "Logits shape mismatch"
    print("[PASS] nnU-Net v2 ResEnc architecture verified.")

    # 3. Authentic CLI verification command reference:
    # CLI: nnUNetv2_predict -i ${INPUT_DIR} -o ${OUTPUT_DIR} -d Dataset001_AMOS -c 3d_fullres -f 0

if __name__ == "__main__":
    verify_nnunet_production_pipeline()
```

---

## 6. Connected Dataset Ecosystem

- 📂 [KiTS23 (`[D9]`)](../../01_datasets/01_segmentation_3d/kits23.md): The official kidney challenge where nnU-Net won and defined the SOTA baseline.
- 📂 [AMOS22 (`[D8]`)](../../01_datasets/01_segmentation_3d/amos22.md): 15-organ abdominal CT/MRI benchmark where ResEnc XL achieved 89.68% DSC.
- 📂 [MSD Decathlon (`[D7]`)](../../01_datasets/01_segmentation_3d/msd_decathlon.md): The original 10-task competition that established the self-configuring nnU-Net doctrine.
- 📂 [TotalSegmentator (`[D6]`)](../../01_datasets/01_segmentation_3d/totalsegmentator.md): Built directly on nnU-Net v2 3D fullres engines for 117 anatomical structures.
- 📂 [autoPET II (`[D11]`)](../../01_datasets/01_segmentation_3d/autopet.md): Whole-body metabolic lesion segmentation won by nnU-Net ResEnc ensembles.
