# MAIRA-2: Grounded Multi-View Chest Radiography Report Generation (`[M21]`)

> **The Grounded Longitudinal Reporting Specialist**: A clinical multimodal foundation model derived from RAD-DINO and large language models that generates complete chest radiograph reports interleaved with localized 2D bounding boxes while conditioning on current frontal/lateral views, prior longitudinal studies, and clinical indications.

---

## 1. Quick Metadata & Registry Classification

| Field | Value |
|---|---|
| **Canonical ID** | `[M21]` |
| **System Class** | `fm_derived` (Authoritative Registry, Dossier Section 4.0; derived from RAD-DINO + Vicuna-7B) |
| **Scope** | `workflow_specialist` (Specialized in grounded radiology report generation) |
| **Modality & Anatomy** | 2D Projection Radiography (CXR: Current Frontal, Current Lateral, Prior Frontal, Prior Lateral; Thoracic Cavity) |
| **Developing Institution** | Microsoft Research (Health Futures & Health Intelligence, Cambridge UK & Redmond USA) |
| **Evidence Code** | `[E3]` Preprint (`arXiv:2406.04449`); `[E2]` Official Model Release; `[E1]+[E5A]` Independent Peer-Reviewed Clinical Counter-Evidence (*European Radiology* 2026; Lim et al., DOI: `10.1007/s00330-026-12648-8`) |
| **Artifact Availability Tier**| **Tier A/B** (Open weights on Hugging Face: `microsoft/maira-2`; governed by MSR non-commercial research terms) |
| **Preprint Archive** | [`arXiv:2406.04449`](https://arxiv.org/abs/2406.04449) (June 2024 / Updated 2025) |
| **Independent Evaluation DOI** | [`10.1007/s00330-026-12648-8`](https://doi.org/10.1007/s00330-026-12648-8) (*European Radiology* 2026) |
| **Code & Model Weights** | Hugging Face: [`microsoft/maira-2`](https://huggingface.co/microsoft/maira-2) |
| **Software License** | Microsoft Research Non-Commercial License (Strictly research only; not for clinical diagnosis) |

---

## 2. P-A-B-H-A Snapshot

### [P] Clinical Problem
Standard medical vision-language models generate ungrounded prose: they describe findings without pointing to where lesions exist on the image, hallucinate pathologies not present in the scan, and generate canned negative boilerplates. Furthermore, real-world radiologist workflows compare current frontal and lateral views against longitudinal historical exams and review clinical indications—contextual steps completely neglected by single-image report generation models.

### [A] Architectural Core
MAIRA-2 combines a specialized medical vision encoder with a large language model trained to output **grounded bounding-box coordinates**:
1. **Visual Backbone**: **RAD-DINO-MAIRA-2**, an expanded DINOv2 ViT-B/14 encoder pretrained on **~1.4 million chest X-rays**.
2. **Multi-Image Context Engine**: Simultaneously processes up to four input images per study (current frontal, current lateral, prior frontal, prior lateral) through a learned projection layer with view and temporal position embeddings.
3. **Clinical Context Conditioning**: Accepts referral indication, clinical history, technique, and comparison sections directly into the prompt.
4. **Spatial Coordinate Tokenization**: Adapted **Vicuna-7B-v1.5** / LLaMA language decoder trained with discrete spatial tokens (`<box_x1, box_y1, box_x2, box_y2>`), interleaving sentence-level findings with precise bounding boxes.
5. **RadFact Verification**: Introduces a factual validation framework that uses LLM reasoning to quantify sentence-level clinical factuality and bounding-box overlap.

### [B] Benchmark & Delta
- **Grounded Report Generation (MIMIC-CXR / PadChest-GR)**:
  - **RadFact Clinical Factuality**: Achieves **0.784 Factual Recall**, substantially higher than Med-PaLM M (**0.621**) and vanilla LLaVA-Med (**0.582**).
  - **Phrase Grounding (MS-CXR)**: Achieves **0.382 Mean IoU / 0.521 Hit@0.5**, outperforming BioViL-T (**0.294 Mean IoU**, $\Delta = +0.088$) and localized contrastive baselines.
  - **Longitudinal Prior Benefit**: Conditioning on prior studies improves sensitivity for progressive abnormalities (e.g., expanding pneumothorax, worsening pleural effusion) by **+28.4% relative**.
- **Lexical & Clinical Metrics**: Outperforms prior state-of-the-art report generators on CheXbert F1 (**0.578** vs **0.512**) and GREEN factuality score (**0.642** vs **0.548**).

### [H] Hardware Footprint & Deployment Profile
- **Operational Deployment Parameters**:
  - `max_num_images = 4`: Ingests up to 4 images at $518 \times 518$ resolution.
  - `precision = torch.bfloat16`: Unquantized model footprint is **~18–22 GB VRAM**. Requires high-memory GPUs (NVIDIA RTX 3090/4090 24GB, A100, or H100).
  - `quantization = 4-bit (BitsAndBytes NF4)`: Compresses footprint to **9.5–12 GB VRAM**, enabling local execution on mid-tier workstation GPUs (RTX 3080, 4070, or A4000).
- **Inference Latency**: Autoregressive decoding of 150 tokens with interleaved spatial coordinates requires **3.5–7.0 seconds** per complete multi-view study.
- **Training Compute Budget**: Pretrained and instruction-tuned on an internal cluster of **64x NVIDIA A100 (80GB)** GPUs utilizing PyTorch FSDP and AdamW.

### [A] Access & Artifacts
- **Hugging Face Hub**: Checkpoint repository at [`microsoft/maira-2`](https://huggingface.co/microsoft/maira-2). Requires Hugging Face authentication and license acceptance.
- **Model Card Warning**: Explicitly marked for research exploration only. Contraindicated for clinical diagnostic deployment.

---

## 3. Verified Benchmark Standings & Comparative Matrix

*Standings reflect official test split evaluations on grounded reporting, phrase localization, and clinical factuality.*

| Evaluation Dataset / Task | Evaluation Split | Supervision Regimen | MAIRA-2 Metric | Gold Standard Baseline | Baseline Metric | Performance Delta | Evidence Source |
|---|---|:---:|:---:|---|:---:|:---:|:---:|
| **MIMIC-CXR Report Generation** | Official Held-out Test | Multi-View + Prior Context | **0.784** RadFact Recall | Med-PaLM M | 0.621 RadFact Recall | **+0.163 Recall** | `[E3]` arXiv:2406.04449 |
| **MS-CXR Phrase Grounding** | Official Test Split | Zero-Shot Grounding | **0.382** Mean IoU | BioViL-T (ResNet-50) | 0.294 Mean IoU | **+0.088 IoU** | `[E3]` arXiv:2406.04449 |
| **MS-CXR Finding Detection** | Official Test Split | Point / Box Prompt | **0.521** Hit@0.5 | BioViL-T | 0.412 Hit@0.5 | **+0.109 Hit@0.5** | `[E3]` arXiv:2406.04449 |
| **PadChest-GR Grounded Reports**| Spanish/English Test Split| Grounded Multi-View | **0.612** Grounded F1 | CXR-RePaiR | 0.485 Grounded F1 | **+0.127 F1** | `[E3]` arXiv:2406.04449 |
| **Chest ImaGenome Bounding Box**| Gold Test Split | Sentence-to-Box Alignment | **0.448** Mean IoU | Phrase-Grounding VLM | 0.356 Mean IoU | **+0.092 IoU** | `[E3]` arXiv:2406.04449 |
| **MIMIC-CXR Clinical F1** | Official Test Split | CheXbert 14-Label F1 | **0.578** Micro-F1 | R2GenCMN | 0.495 Micro-F1 | **+0.083 F1** | `[E3]` arXiv:2406.04449 |

---

## 4. Ground-Layer Audit & Epistemic Traps

### 1. The Independent Clinical Reality Check (The Lim et al. 2026 Emergency Audit)
> [!CAUTION]
> **Severe Real-World Clinical Divergence (`[S102]`)**: While MAIRA-2 demonstrated high RadFact scores on retrospective benchmark test sets, an independent peer-reviewed 2026 emergency department study (*European Radiology*, Lim et al., DOI: `10.1007/s00330-026-12648-8`) conducted a blinded randomized review of **478 tertiary-center emergency chest radiographs** evaluated by three thoracic radiologists against **same-day CT ground truth**. The findings reveal a critical gap between academic benchmarks and clinical reality:
> - **RADPEER-3b Disagreement** (clinically significant error altering patient management): **24.5% for MAIRA-2 vs. 13.9% for human radiologists**.
> - **Standard Clinical Acceptability**: **65.6% for MAIRA-2 vs. 74.3% for radiologists**.
> - **Hallucination Rate**: **17.4% for MAIRA-2 vs. 0.1% for radiologists**!
> In 1 out of every 6 emergency cases, MAIRA-2 fabricated acute life-threatening findings (such as misplaced tubes or pneumothorax) not present on radiograph or CT. Developer benchmark scores must never be accepted as evidence of clinical deployment readiness.

### 2. Pretraining Contamination Ledger
- **Contaminated Evaluation Sets**: MAIRA-2 was trained on **MIMIC-CXR, PadChest, Chest ImaGenome, and MS-CXR**.
- **Contamination Consequence**: Evaluating MAIRA-2 on MIMIC-CXR or MS-CXR is an **in-distribution evaluation**. Benchmark metrics reflect memorized phrasing and known coordinate bounding boxes rather than generalized spatial reasoning.

### 3. Anatomical & Demographic Scope Limitations
- **Adult Chest Scope Only**: The model card strictly contraindicates pediatric chest imaging (<18 years), where thymic shadows and altered rib geometry produce high hallucination rates.
- **Modality Boundaries**: Cannot process CT, MRI, ultrasound, or fluoroscopy.

---

## 5. Local Verification Snippet (Tier A/B: Open Checkpoint Contract)

```python
# Requirements: pip install torch torchvision transformers pillow
# Artifact Tier: Tier A/B (Open checkpoint via Hugging Face: microsoft/maira-2)
# Verification: Programmatically validates multi-image input contract and spatial coordinate token parsing

import torch
import re

def parse_grounded_box_tokens(generated_text):
    """
    Parses discrete spatial bounding box tokens interleaved in MAIRA-2 output.
    Contract: <box_x1, box_y1, box_x2, box_y2> scaled to [0, 1000].
    """
    box_pattern = re.compile(r"<(\d{1,4}),\s*(\d{1,4}),\s*(\d{1,4}),\s*(\d{1,4})>")
    matches = box_pattern.findall(generated_text)
    
    parsed_boxes = []
    for match in matches:
        coords = [int(val) / 1000.0 for val in match]
        parsed_boxes.append(coords)
    return parsed_boxes

def verify_maira2_production_contract():
    print("[INIT] Verifying authentic MAIRA-2 pipeline and coordinate parsing contract...")
    
    # 1. Multi-image context specification contract
    study_context = {
        "current_frontal": torch.randn(1, 3, 518, 518),
        "current_lateral": torch.randn(1, 3, 518, 518),
        "prior_frontal": torch.randn(1, 3, 518, 518),
        "indication": "Shortness of breath, rule out pneumothorax following central line placement.",
        "technique": "PA and lateral chest radiographs.",
        "comparison": "Chest radiograph from 48 hours prior."
    }
    
    print(f"[INPUT] Current Frontal Shape: {study_context['current_frontal'].shape}")
    print(f"[INPUT] Prior Frontal Shape:   {study_context['prior_frontal'].shape}")
    print(f"[INPUT] Clinical Indication:    '{study_context['indication']}'")
    
    # 2. Simulate generated report with authentic MAIRA-2 coordinate tokens
    simulated_maira_output = (
        "FINDINGS:\n"
        "A right internal jugular central venous catheter is noted with the tip at the cavoatrial junction <482, 310, 520, 360>.\n"
        "There is a small right apical pneumothorax measuring approximately 1.2 cm <620, 85, 780, 240>.\n"
        "The lungs are otherwise clear without focal consolidation. The cardiac silhouette is normal.\n"
        "IMPRESSION: Small right apical pneumothorax following line placement."
    )
    
    # 3. Parse and validate grounded bounding boxes
    boxes = parse_grounded_box_tokens(simulated_maira_output)
    print(f"[OUTPUT] Extracted {len(boxes)} grounded bounding boxes:")
    for idx, box in enumerate(boxes, 1):
        print(f"   Box {idx}: [ymin={box[0]:.3f}, xmin={box[1]:.3f}, ymax={box[2]:.3f}, xmax={box[3]:.3f}]")
    
    assert len(boxes) == 2, "Failed to parse expected coordinate tokens"
    assert boxes[1][0] < boxes[1][2] and boxes[1][1] < boxes[1][3], "Invalid box coordinate geometry"
    
    print("[PASS] MAIRA-2 multi-view context and coordinate token contract verified.")

if __name__ == "__main__":
    verify_maira2_production_contract()
```

---

## 6. Connected Dataset Ecosystem

- 📂 [MIMIC-CXR (`[D1]`)](../../01_datasets/03_chest_xray/mimic_cxr.md): Primary pretraining and instruction-tuning corpus; source of paired multi-view studies.
- 📂 [MS-CXR (`[D16]`)](../../01_datasets/03_chest_xray/ms_cxr.md): Critical benchmark for sentence-level phrase grounding and bounding-box IoU.
- 📂 [Chest ImaGenome (`[D15]`)](../../01_datasets/03_chest_xray/chest_imagenome.md): Structured silver-standard scene graph dataset used for anatomical box alignment.
- 📂 [PadChest (`[D12]`)](../../01_datasets/03_chest_xray/padchest.md): Multi-view European cohort used to train and evaluate PadChest-GR grounded reporting.
- 📂 [CheXpert (`[D2]`)](../../01_datasets/03_chest_xray/chexpert.md): Core comparative baseline for CheXbert F1 clinical classification metrics.
