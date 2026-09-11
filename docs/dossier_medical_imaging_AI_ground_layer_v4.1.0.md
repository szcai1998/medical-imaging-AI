# Medical AI Technical Dossier 02: Medical Imaging AI — Ground-Layer Frontier Landscape

## Evidence-Audited Ground Layer for Medical Image Computing: Learning Paradigms, Foundation Models, Clinical Tasks, Modalities, Data, Robustness, Quantitative Imaging, Computational Pathology, Spatial Biology, and Clinical Translation

**Snapshot date:** 11 September 2026  
**Document ID:** `DOSSIER-MED-02-IMAGING`  
**Lifecycle:** **LIVING, EVIDENCE-AUDITED REFERENCE — NOT FROZEN**  
**Primary purpose:** provide a current, evidence-audited **ground layer for medical-imaging AI**: the imaging-problem stack, learning paradigms, clinical-task landscape, modality ecosystems, foundation-model and non-foundation-model frontiers, datasets, capabilities, maturity, independent evidence, failure modes, robustness/adaptation, quantitative imaging, clinical-translation state, and unresolved areas. **Foundation models are a major part of the 2026 landscape, not the organizing assumption. This dossier is not a project-selection document; project design comes only after the field map is understood.**

------------------------------------------------------------------------

## 0. Executive Position

Medical imaging AI in 2026 is not undergoing a simple replacement of task-specific networks by foundation models. The evidence supports a more complicated picture:

1.  **task-specific supervised systems** remain strong reference comparators/baselines when enough labelled data exist;
2.  **general visual and vision-language encoders** provide reusable representations, concept spaces, retrieval, and data-efficient adaptation;
3.  **promptable and uncertainty-aware segmentation models** trade task specialization for interactive flexibility and broader anatomical/tumour coverage;
4.  **native volumetric foundation models** increasingly learn directly from 3D CT/MRI rather than treating a study as unrelated 2D slices;
5.  **grounded and auditable radiology systems** are moving beyond fluent text toward localization, concepts, measurements, priors, and tool use;
6.  **reconstruction and registration are becoming foundation-model problems**, with 2026 peer-reviewed work in sparse-view CBCT, semantic-guided fast MRI, general-purpose MRI representation, and foundational registration;
7.  **health-system and spatiotemporal learning are distinct frontier paradigms**: NeuroVFM shows the value of learning directly from routine CT/MRI archives, while NeuroSTORM shows that 4D fMRI requires native spatiotemporal representation rather than slice-centric abstraction;
8.  **modality-specific foundation models remain important**: ophthalmic OCT, retinal longitudinal imaging, ultrasound, musculoskeletal radiography, mammography and endoscopy have acquisition physics and workflow structure that do not disappear inside a universal VLM;
9.  **pathology and spatial-biology foundation models** are moving from tile encoders toward slide-level aggregation, multimodal dialogue, morphology-to-marker inference, direct multiplex-proteomics modelling, uncertainty-aware prediction, and model-selection/efficiency analysis;
10. **clinical translation remains the bottleneck**: external validation, contamination control, robustness, calibration, privacy, workflow integration, safety, and prospective utility are not solved by scale alone;
11. **label-efficient and data-centric learning remain active frontiers rather than obsolete pre-FM topics**: weak/semi-supervised learning, active learning, heterogeneous-label training, pseudo-labelling and selective annotation continue to determine what can be built when expert labels are scarce or inconsistent [S108];
12. **domain robustness and model evolution are first-class research ecosystems**: domain generalization, adaptation, OOD detection, test-time adaptation and continual learning address scanner/site/protocol/temporal shift that pretraining alone does not remove [S109,S110];
13. **synthetic and generative imaging are broader than generative foundation models**: synthetic data are used for augmentation, rare-event coverage, privacy, bias studies and simulation, but fidelity, utility and privacy must be evaluated separately [S111];
14. **quantitative imaging remains a parallel frontier**: radiomics, deep imaging biomarkers, disease phenomics, opportunistic screening and prognosis ask what clinically useful information can be extracted from images beyond ordinary visual interpretation [S112,S116];
15. **acquisition quality, artifact detection, harmonization and preprocessing can be the bottleneck upstream of any model**, and should be treated as an active technical layer rather than invisible plumbing [S115].

This dossier therefore does **not** ask “Which model is SOTA?” as a global question. That question is ill-posed. It asks instead:

> **For each major medical-imaging AI branch, what exists, what capabilities are genuinely demonstrated, how mature is the evidence, where do independent results agree or disagree, and what remains unresolved?**

That is the standard used throughout this document. The dossier maps the field first; it deliberately postpones project selection, hypothesis design, and experimental planning to a separate downstream stage.

------------------------------------------------------------------------

# 1. Evidence Policy

## 1.1 Canonical evidence source/role codes — not an evidence hierarchy

The evidence code describes **what kind of source or evidentiary role supports a claim**. It is not a scalar evidence grade. The codes are deliberately **composable rather than mutually exclusive**: E1–E4 primarily describe publication/artifact source form, while E5A/E5B identify an independent empirical or synthesis/methodology role. For example, a peer-reviewed independent benchmark can be recorded as `E1+E5A`.

| Code | Source provenance | Appropriate use | Main limitation |
|---|---|---|---|
| **E1** | peer-reviewed primary paper / version of record | methodology, cohorts, author-reported results, limitations | peer review does not establish independence, transportability, clinical utility or reproducibility |
| **E2** | official model card, repository, dataset card, release documentation | implementation, artifacts, licence/access, supported inputs, current interfaces | usually developer-controlled; not independent effectiveness evidence |
| **E3** | preprint, non-archival submission, workshop/conference abstract without full peer-reviewed article | emerging methods and provisional results | claims remain provisional |
| **E4** | vendor/institutional announcement or technical blog | release context and engineering details | promotional/non-independent; requires corroboration for effectiveness claims |
| **E5A** | independent empirical benchmark, replication, external challenge analysis or independent comparative study **(role modifier; may coexist with E1/E3)** | comparative evidence, failure modes, external validity, replication | protocol scope, contamination and benchmark design constrain conclusions |
| **E5B** | review, consensus, perspective, reporting guideline or methodological framework **(role modifier; may coexist with publication-form metadata)** | synthesis, study design, terminology, field-level framing | not direct empirical proof that a particular model works |

**Important:** an `E1+E5A` independent peer-reviewed benchmark can be stronger evidence for generalization/model selection than an `E1` developer paper. E5B guidance can be methodologically authoritative without being empirical evidence for a specific model. Do not force one paper into a single code when both its publication form and evidentiary role matter.

### 1.1.1 Canonical evidence profile

Every consequential field-state statement or model/system evaluation should use the following fields. This table **replaces** the older v3.3/v3.4 shorthand; do not maintain parallel definitions.

| Dimension | Canonical values / questions |
|---|---|
| **Statement type** | empirical / comparative / methodological / translational / governance |
| **Publication maturity** | peer-reviewed / accepted-online-first / preprint / abstract / documentation-only |
| **Evidence code(s)** | E1 / E2 / E3 / E4, plus E5A and/or E5B role modifiers where applicable |
| **Evaluator independence** | developer / partially independent / independent |
| **Cohort independence** | overlapping / derivative / non-overlapping known / unknown |
| **Institution / health-system independence** | same / different / multicentre / unknown |
| **Geographic independence** | same region / different region / multinational / unknown |
| **Temporal independence** | concurrent / later-period / prospective / unknown |
| **Acquisition independence** | same scanner-protocol family / shifted vendor-protocol / heterogeneous / unknown |
| **Reference-standard independence** | incorporated / partially independent / independent / unknown |
| **Artifacts available** | none / code / weights / data / executable pipeline |
| **Replication status** | none / author rerun / independent partial / independent full |
| **Reference-standard type** | radiology report / pathology / surgery / outcome / expert annotation / quantitative measurement / other task-appropriate target |
| **Reference-standard quality** | single-reader / multi-reader / adjudicated / outcome-linked / uncertainty-characterized / mixed, as applicable |
| **Contamination status** | excluded_by_provenance / partially_auditable / plausible_overlap / known_overlap / unknowable |
| **Risk of bias / applicability** | low / some concerns / high / not assessed, with domain-specific tool where applicable |
| **Study temporality** | retrospective / prospective / mixed / not applicable |
| **Workflow exposure** | offline / shadow / controlled-human-AI / live-monitored / not applicable |
| **Statistical maturity** | descriptive / uncertainty reported / prespecified comparative inference / adequately powered clinical evaluation |
| **Audit completeness** | deep / substantial / partial / scaffold / gap — describes how thoroughly this dossier has mapped the branch, **not** how mature the field itself is |

A compact record should therefore look like:

```text
statement_type=empirical
publication=peer-reviewed
evidence_codes=E1
evaluator_independence=developer
cohort_independence=non-overlapping-known
institution_independence=multicentre
geographic_independence=multinational
temporal_independence=concurrent
acquisition_independence=heterogeneous
reference_standard_independence=independent
artifacts=weights+code
replication=none
contamination_status=partially_auditable
risk_of_bias=some_concerns
study_temporality=retrospective
workflow_exposure=offline
statistics=paired-CI-reported
```

This prevents several category errors at once: publication prestige ≠ evidence strength; repository availability ≠ replication; “external” ≠ fully independent; and absence of detected overlap ≠ overlap excluded.

### Rule

**No unqualified global “SOTA” claim is allowed.** A defensible claim must specify at minimum:

- task;
- evaluation dataset/cohort and unit of analysis;
- adaptation regime (zero-shot, linear probe, fine-tuned, native end-to-end, prompt-assisted, etc.);
- metric and statistical uncertainty;
- comparator set;
- source provenance;
- independence/externality;
- study temporality and workflow exposure where relevant;
- date/version.

If those are not known, write **“frontier / notable / emerging”**, not “SOTA.”

------------------------------------------------------------------------

## 1.2 Availability is multidimensional

“Open source” is frequently used too loosely in medical AI. Availability should be recorded on **three independent axes**, rather than compressed into one label.

### Artifact axis

| Label            | Meaning                                               |
|:-----------------|:------------------------------------------------------|
| **CODE**         | implementation/source code available                  |
| **WEIGHTS**      | model weights/checkpoints available                   |
| **DATA**         | dataset or a defined dataset subset released          |
| **API/PLATFORM** | usable primarily through a hosted or governed service |
| **NONE/NR**      | no reusable artifact verified in this audit           |

### Access axis

`direct-download / gated / credentialed-DUA / challenge-only / institutional-request / API-only / unreleased`

### Reuse-rights axis

`permissive / commercial-allowed-with-terms / research-only / non-commercial / no-derivatives / custom-EULA / mixed / unknown`

Legacy shorthand such as **OS** or **OW** may still appear in historical notes, but it should never replace these three fields. For example, “weights available” does not answer whether access is gated or whether commercial derivative use is permitted.

Licence and access status must be checked at use time. A paper being open access does **not** imply that its code, weights or training data are openly reusable.

------------------------------------------------------------------------

## 1.3 Verification rules

1.  Prefer **primary papers** for scientific results.
2.  Prefer **official repositories/model cards** for implementation, release, checkpoint, and licence claims.
3.  Treat preprint results as **author-reported emerging evidence**.
4.  Never copy benchmark numbers from a secondary table without checking the underlying evaluation protocol.
5.  Never compare numbers across papers as if they form a leaderboard unless the dataset, split, preprocessing, supervision, prompt protocol, and metric are genuinely comparable.
6.  Treat any dataset that may overlap with pretraining data as a **contamination risk**, especially TCGA, MIMIC-CXR, CheXpert, and widely mirrored public datasets.
7.  Treat vendor-reported VRAM, speed, and throughput as configuration-specific engineering measurements, not model constants.
8.  For 3D imaging, voxel geometry, orientation, spacing, sequence/phase selection, and study-level semantics are part of the model input definition, not implementation trivia.
9.  **Check event/publication timing against the snapshot date.** Online-first articles may have future issue dates; conference proceedings metadata may exist before a conference is held. As of 11 Sep 2026, MICCAI 2026 (27 Sep–1 Oct) is still future and must not be described as an already-presented conference result. [S57]
10. **Do not equate benchmark fluency with clinical competence.** Closed-form or multiple-choice performance must be separated from open-ended multimodal interpretation, diagnosis, planning, and rationale. A 3 Sep 2026 real-case benchmark found >90% performance on structured multiple-choice items for leading models but open-ended multimodal accuracy scarcely reached 60%, with substantial image-interpretation weakness and text-driven hallucination. [S67]
11. **Treat clinical evaluation as a lifecycle, not a single test set.** Technical validation, operational robustness, human–AI interaction, clinical evidence, and real-world integration require different study designs and may trigger return to earlier phases after drift, updates, or safety signals. [S68]
12. **Dataset-count units are not interchangeable.** Always state whether a number counts patients, examinations/studies, series, reconstructed volumes, 2D images/slices, WSIs, tiles/patches, masks, or report/VQA pairs. A larger number in a lower-level unit is not evidence of a larger independent clinical cohort.
13. **Derivative datasets are not independent external validation.** If dataset B is derived from dataset A, evaluation on B after training/pretraining on A is an annotation-task transfer test, not an independent site/cohort generalization test unless the relevant patient/study overlap is explicitly excluded.
14. **Dataset provenance and label provenance are separate fields.** Human expert labels, NLP-mined labels, model-generated masks, LLM-generated questions/reports, and radiologist-revised AI outputs must not be collapsed into a single “ground truth” category.
15. **Version drift must be recorded.** Prefer the final version of record for scientific claims and record the dataset/model-card version for live resources. If a live repository has changed counts since publication, preserve both values with dates rather than silently replacing the peer-reviewed number.
16. **Access is not binary.** Distinguish unrestricted public download, credentialed/DUA access, gated non-commercial access, challenge-only/hidden test labels, derivative annotations requiring a separately governed parent dataset, and unreleased/private training corpora.
17. **Reporting quality and evidentiary quality are different.** Compliance with CLAIM/STARD-AI/TRIPOD+AI/CONSORT-AI/DECIDE-AI improves transparency but does not upgrade a weak study design into strong evidence. [S70–S75]
18. **Reference standards are fallible measurements.** Use “reference standard,” not “ground truth,” unless the target is genuinely deterministic. Record who/what produced the target, adjudication, blinding, uncertainty and whether the standard is independent of the model input. [S70,S71]
19. **Prompt selection is part of the protocol.** For zero-shot/prompted systems, prompt templates, prompt ensembles and prompt-selection data must be declared; test-set prompt tuning is leakage.
20. **A consequential field-state conclusion must carry counter-evidence where available.** For every important conclusion, actively search for a negative benchmark, failed replication, contradictory modality result, or evidence that narrows the scope of the conclusion.

## 1.4 Landscape Evidence Graph

The dossier should be maintained as a **landscape evidence graph**, not merely a catalogue of model names and not a list of proposed research projects.

For every consequential area, model family, or field-state conclusion, record:

``` text
AREA / SYSTEM / FIELD-STATE STATEMENT
  -> strongest supporting evidence
  -> independent evidence, when available
  -> counter-evidence / negative result / scope-narrowing evidence
  -> evidence maturity
  -> unresolved confounders or disagreements
  -> current scope boundary
  -> evidence that would materially change the map
```

Examples of landscape questions that require this treatment:

- how mature native 3D representation learning is relative to slice-centric approaches;
- whether promptable segmentation has demonstrated human-efficiency benefits beyond benchmark flexibility;
- where medical-domain pretraining helps, where generic visual pretraining remains competitive, and where evidence conflicts;
- whether grounded/tool-augmented radiology has moved beyond developer benchmarks;
- how robust pathology FMs are across laboratories, scanners, stains, and institutions;
- whether longitudinal/spatiotemporal models demonstrate clinically meaningful gains beyond repeated-patient or site identity;
- how far virtual molecular imaging and direct spatial-proteomics foundation modelling have progressed;
- which branches have prospective or workflow-level evidence rather than retrospective benchmark evidence only.

A model paper is one observation in the landscape. It is not, by itself, the state of the field.

## 1.5 Field-State, Evidence-Maturity and Uncertainty Ledger

This ledger summarizes the **current audited state of major cross-cutting questions**. It is intentionally not a hypothesis ledger and does not prescribe experiments or projects.

### Maturity language

- **deep / mature evidence ecosystem:** multiple model families plus meaningful independent comparison, external validation, or repeated cross-study evidence;
- **established:** substantial peer-reviewed evidence with multiple systems/settings, but important transportability or clinical-evidence gaps remain;
- **developing:** credible peer-reviewed evidence exists, but independent comparison, breadth, or externality remains limited;
- **emerging:** technically important evidence exists, often from one/few groups or provisional releases, with substantial unanswered questions;
- **methodologically strong:** the field-level principle is well supported even though it is not itself a model-performance claim.

| Topic | Current field picture | Strongest support | Counter-evidence / scope boundary | Evidence maturity | What remains unresolved |
|---|---|---|---|---|---|
| **Native volumetric context** | Native 3D/volumetric modelling is clearly viable and advantageous in several audited settings, but the benefit attributable specifically to dimensionality is not consistently isolated. | direct volumetric OCT comparison [S46]; ecological independent CT comparison [S81]; contextual 3D-MRI transfer evidence [S40] | architecture, corpus, objective, compute and information exposure differ across most CT/MRI comparisons | **established, causality unresolved** | how much of the gain comes from 3D context itself versus data/objective/architecture |
| **Promptable 3D segmentation** | Promptable/generalist segmentation has become a real 3D medical-imaging branch, with some direct annotation-efficiency evidence and broad technical coverage. | MedSAM2 [S4], VISTA3D [S5], BiomedParse [S8], SAT3D [S39] | strongest human-efficiency evidence is narrower than the technical literature; task-specific supervised systems remain strong | **established technically; human-efficiency evidence developing** | generalizable accuracy–interaction-effort trade-off across annotators, sites and tasks |
| **Domain-aligned versus generic pretraining** | There is no universal winner. Severe modality shifts can favour domain alignment, while strong generic visual pretraining remains competitive in other settings. | PET/CT evidence [S77]; DINOv3 medical evaluation context [S86] | comparisons often differ in corpus scale, architecture, objective and adaptation | **developing comparative evidence** | when domain alignment matters after controlling scale/data/objective |
| **CT representation limits** | Independent frozen-feature evidence suggests focal abnormalities can be harder than diffuse abnormalities for several CT FM representations. | [S81] | bounded to a particular benchmark/adaptation regime; fine-tuning may change the pattern | **developing / bounded** | whether the focal–diffuse gap persists across sites and adaptation regimes |
| **Aggregation** | Learned aggregation is not automatically superior to simple pooling; value is task- and representation-dependent. | CT and pathology evidence [S30,S81] | architecture and feature distributions differ substantially | **developing comparative evidence** | which task/feature regimes actually justify complex aggregation |
| **Pathology FM model selection** | No universal pathology-FM winner is demonstrated across independent benchmarks; rankings change by task, data regime and adaptation. | [S28,S47,S82] | public-data/pretraining overlap and benchmark heterogeneity remain difficult to eliminate | **deep / mature comparative ecosystem** | robust model-selection rules under auditable overlap and clinically relevant tasks |
| **Technical/site/dataset signatures** | FM embeddings can encode strong non-biological technical or dataset identity signals, demonstrated clearly in pathology and mammography. | [S29,S99] | decodability does not prove downstream causal reliance | **strong evidence for encoding; causal use unresolved** | when technical signatures materially drive clinical predictions or calibration failures |
| **Tool augmentation in radiology** | Tool augmentation is a credible emerging direction for measurement-dependent tasks, but current evidence is still limited and subsystem attribution is difficult. | CARE-X tool study [S19] | preprint-level developer evidence; localization/tool/reasoning errors remain entangled | **emerging** | whether gains survive independent evaluation with validated tools and safe failure handling |
| **Temporal / spatiotemporal modelling** | Explicit longitudinal or spatiotemporal learning has demonstrated predictive value in retinal and fMRI settings. | RETFound Plus [S61], NeuroSTORM [S59] | identity, utilization, treatment, censoring and near-duplicate effects can mimic temporal intelligence | **established in selected settings** | how much gain reflects genuine temporal structure versus repeated-person/health-system signal |
| **Clinical readiness** | Benchmark breadth and model scale do not establish clinical readiness. Some specialty systems now extend into stronger workflow/prospective evidence, but most FM literature remains retrospective. | [S2,S67,S68,S102,S104] | readiness is intended-use specific; evidence maturity varies dramatically across branches | **methodologically strong; translational maturity heterogeneous** | which FM branches can show prospective utility, safe workflow integration and durable post-deployment performance |
| **Transportability** | “External validation” is heterogeneous; site, geography, time, acquisition, evaluator and reference-standard independence must be separated. | [S68,S70–S75,S79,S80] | a well-designed external cohort may be sufficient for a narrow intended use, but the label “external” alone is not | **methodologically strong** | practical reporting/benchmark standards that make transportability comparable across papers |
| **Study-level echocardiography** | Echocardiography has moved from single-frame/view representation toward video and study-level multi-view modelling. | EchoCLIP/EchoPrime/Echo-Vision-FM [S83,S84,S87] | shared Cedars-Sinai lineage limits independence of architectural evidence | **established specialty branch** | independent replication of the incremental value of learned study-level aggregation |
| **Open artifacts and reproducibility** | Code/weights/data materially improve auditability and reproduction support but are not equivalent to independent replication. | repeated cross-field experience; evidence doctrine | hidden preprocessing/data, artifact drift and environment dependence remain common | **methodologically strong** | reproducibility under pinned end-to-end pipelines and independent evaluators |

**Rule:** later synthesis sections should summarize these field states and modality ecosystems, not turn them back into a project or hypothesis ledger.


## 1.6 Landscape Construction, Recall and Audit-Completeness Method

This dossier is a **curated high-recall frontier atlas**, not a PRISMA-complete systematic review. That distinction is deliberate. Its purpose is to preserve the structure of the field well enough that later project design is not biased by whichever papers happened to be most visible.

### Construction method

The landscape is built from five evidence streams:

1. **primary peer-reviewed model/method papers** for scientific claims and model-family identity;
2. **official repositories/model cards/dataset cards** for artifacts, interfaces, versions, access and licences;
3. **independent comparative studies, replications and clinical evaluations** for model-selection reality, failure modes and transportability;
4. **reviews, consensus/guideline papers and methodological perspectives** for field structure and evaluation doctrine;
5. **systematic reviews as recall backstops**, not as substitutes for primary verification. The 2026 *Medical Image Analysis* FM systematic review maps 237 VFM/VLFM studies and is used to check whether this dossier is missing major paradigms or modality/task families rather than to copy its model catalogue [S114].

### Inclusion logic

Promote an item to the landscape when it is at least one of the following:

- a field-shaping or technically distinct model/method lineage;
- a strong task/modality anchor needed to understand a branch;
- an independent benchmark or negative result that changes interpretation;
- a dataset/resource whose genealogy materially affects evaluation;
- a methodological framework needed to interpret evidence quality;
- a clinically meaningful prospective/workflow evaluation;
- a non-FM frontier that could plausibly compete with, complement, or invalidate an FM-centric project framing.

Do **not** promote every incremental model variant. Exclude or demote duplicate systems that add no distinct paradigm/evidence, unsupported global-SOTA claims, and product announcements without auditable technical or clinical evidence.

### Audit completeness is separate from field maturity

Use one of the following labels:

- **deep** — multiple model/method families, datasets, independent evidence and major counter-evidence have been mapped;
- **substantial** — major lineages and evidence are covered, but some dataset genealogy or competing approaches remain incomplete;
- **partial** — credible anchors exist, but the branch has not been comprehensively mapped;
- **scaffold** — only enough structure is present to prevent the branch from disappearing from the field map;
- **gap** — this dossier does not currently support a field-state conclusion.

**Critical rule:** `audit_completeness` describes **this dossier**, not the underlying science. An `audit_completeness=gap` branch must be read as **unknown here**, never as “immature” or “unimportant.”

### Coverage-recall rule

At each major snapshot refresh:

- cross-check major modalities, tasks and learning paradigms against at least one recent systematic/scoping review;
- search specifically for independent negative evidence and not only new flagship models;
- record newly discovered missing **categories** before adding long lists of individual systems;
- preserve explicit gaps instead of filling them by inference;
- do not claim exhaustive literature coverage unless a formal systematic-review protocol has actually been executed.

The *Nature Methods* 2026 perspective on benchmarking biomedical foundation models is used as a methodological anchor for reproducibility, generalization, replicability and the deeper problem of what it means to falsify/evaluate a foundation model [S113].

------------------------------------------------------------------------

# 2. What This Dossier Covers — and What It Does Not

## 2.0 Operational taxonomy: what counts as a foundation model here?

The phrase **foundation model** is too inconsistently used in medical imaging to be accepted at face value. v4.1 uses one machine-auditable enum for FM system class:

```text
system_class = core_fm | fm_derived | fm_enabled | companion
```

| `system_class` | Operational definition | Typical examples |
|---|---|---|
| `core_fm` | the **medical release itself is reusable** across multiple downstream tasks, datasets, anatomical targets, prompts, or clinically meaningful settings; it may have been pretrained from scratch, adapted from a general FM, or distilled from another FM | Merlin, CT-CLIP, Triad, RAD-DINO, MedSAM2, Prov-GigaPath, UNI, OCTCube |
| `fm_derived` | a downstream medical model materially adapted from a reusable pretrained model, but whose released capability/evidence remains comparatively narrow or workflow-bounded | AbdomenNet clinical system, Sonomate, MAIRA-2, CARE-X |
| `fm_enabled` | larger clinical/engineering method or workflow in which an FM is one component; workflow success does not establish a new reusable FM | FOCUS, VLM-guided MRI reconstruction, radiotherapy ROI workflow |
| `companion` | benchmark, baseline, dataset/resource, aggregation method, deployment stack, review or governance framework important to the FM ecosystem but not itself a reusable FM | nnU-Net, nnMIL, MONAI Deploy, independent benchmarks |

**Eligibility rule:** author use of the phrase “foundation model” is not sufficient. `core_fm` describes the **reusability and demonstrated breadth of the medical release**, not whether it was initialized from another foundation model. Adaptation/distillation lineage must therefore be recorded separately. A system is promoted to `core_fm` only when the released medical model demonstrates transfer beyond one narrowly fixed endpoint.

**Ground-layer interpretation:** this four-value enum is only a **foundation-model ecosystem registry**. `companion` means “not itself a reusable FM” — it does **not** mean scientifically secondary or less important to medical-imaging AI. Non-FM methods such as specialist supervised learning, domain adaptation, active learning, continual learning, radiomics or QC are positioned using the three landscape axes in Section 3 rather than forced into the FM enum.

### Second axis: scope is not system class

Record **scope** separately using one exact enum:

```text
scope = generalist | modality_generalist | specialty_generalist | organ_specialist | workflow_specialist
```

Record **foundation lineage** separately for every system where relevant: the exact reusable pretrained backbone/model, adaptation source, or distillation teacher. This field is mandatory for `fm_derived` and `fm_enabled` systems and should also be populated for `core_fm` systems that are adapted/distilled rather than trained as a new medical foundation model. If the lineage is not verified, write `unverified` rather than infer it.

Examples:

```text
EchoPrime:  system_class=core_fm; scope=modality_generalist; foundation_lineage=n/a
Sonomate:   system_class=fm_derived; scope=workflow_specialist; foundation_lineage=BiomedCLIP-derived initialization
AbdomenNet: system_class=fm_derived; scope=workflow_specialist; foundation_lineage=self-supervised NCCT foundation encoder
FOCUS:      system_class=fm_enabled; scope=workflow_specialist; foundation_lineage=fine-tuned ophthalmic VFM
```

This prevents **foundation-model inflation** while still allowing clinically useful FM-enabled and FM-derived systems to remain in the landscape. **Section 4.0 is the authoritative taxonomy registry; downstream narrative tables may omit scope/foundation-lineage columns for readability but must not invent new `system_class` values.**

### 2.0.1 Evidence maturity is a separate axis

`system_class` and `scope` are **not maturity grades**. Use conservative branch-level maturity language:

- **deep / mature ecosystem** — multiple competing model lineages plus substantial peer-reviewed and independent comparative evidence;
- **established specialty branch** — multiple credible systems or one unusually broad system with substantive peer-reviewed transfer/external evidence, but less independent ecosystem depth;
- **emerging branch** — credible recent systems with limited independent replication, narrow task coverage, or provisional publication status;
- **watchlist / provisional** — strategically relevant release, abstract, preprint, or resource whose scientific position is not yet stable.

A `core_fm` can therefore remain **provisional** if its evidence is preprint/abstract-level, and an `fm_derived` system can have **strong clinical evidence**. Never infer evidentiary maturity from taxonomy alone.

## 2.1 Core scope

This dossier maps **medical-imaging AI in the foundation-model era**, not only foundation models. Its core scope therefore includes:

- strong task-specific supervised systems and specialist baselines where they remain competitive;
- label-efficient/data-centric learning: weak/semi-supervised learning, active learning, heterogeneous-label learning and pseudo-labelling;
- general medical image and image-text representation learning;
- 3D CT/MRI vision-language modelling, health-system learning, and 4D functional-neuroimaging representation learning;
- promptable, automatic and uncertainty-aware 3D segmentation;
- detection/localization, tracking and physical-space correspondence where they define active imaging problems;
- grounded, auditable, longitudinal, quantitative and tool-augmented radiology;
- acquisition quality control, artifact detection, harmonization and preprocessing as upstream AI layers;
- reconstruction/enhancement/restoration and foundational registration;
- domain generalization, adaptation, OOD/selective prediction, test-time adaptation, drift and continual learning;
- synthetic/generative imaging as a data strategy as well as a model family;
- quantitative imaging, radiomics, deep imaging biomarkers, phenomics, prognosis and opportunistic screening;
- modality-specific FM regimes in ultrasound, ophthalmic OCT/fundus, dermatology/dermoscopy/total-body photography, musculoskeletal radiography, mammography, endoscopy, and emerging PET/CT nuclear-medicine representation learning;
- biomedical microscopy/computational-imaging models for restoration and related inverse problems;
- radiotherapy imaging/data workflows involving CT, RTSTRUCT, RTDOSE, RTPLAN, spatial semantics and treatment-planning infrastructure;
- computational pathology from tiles to whole slides;
- virtual spatial biology and medical image synthesis;
- deployment infrastructure, privacy/security failure modes, federated/distributed learning, human–AI interaction, and clinical translation.

## 2.2 Explicit boundary

A person who masters only this dossier is **not yet a complete medical-image-computing expert**. The dossier now maps the major active AI frontiers above, but it does not replace deep technical training in:

- MRI acquisition physics, k-space, parallel imaging, compressed sensing, quantitative MRI and sequence design;
- CT projection physics, dose, reconstruction kernels, spectral/photon-counting CT, sparse-view and low-dose reconstruction;
- ultrasound beam formation, Doppler, elastography and operator/acquisition physics;
- nuclear-medicine tracer kinetics, attenuation/scatter correction, SUV quantification and SPECT/PET reconstruction;
- rigid, affine, deformable and diffeomorphic registration mathematics;
- classical image processing, optimization and signal-processing methods that remain useful baselines;
- causal inference, biostatistics, epidemiology and clinical-study design beyond AI-specific reporting frameworks;
- DICOM/DICOMweb, PACS/RIS, FHIR, workflow engines and hospital integration;
- device software engineering, cybersecurity, quality management and jurisdiction-specific regulation.

These are **depth boundaries**, not exclusions from the landscape. When one of them becomes a major active AI branch—such as reconstruction, registration, image-quality assessment, harmonization or radiomics—it is mapped here at the field level and then routed to a deeper companion curriculum.

This dossier now includes **anchor systems** for reconstruction and registration because those branches are already too mature to omit: DeepSparse for sparse-view CBCT reconstruction, vision-language-guided fast-MRI reconstruction, Triad for 3D MRI transfer across segmentation/classification/registration, uniGradICON and the 2026 LUMIR follow-up for foundational registration, universal CT enhancement, and HorusEye for self-supervised X-ray tomography restoration. These anchors do **not** replace the companion physics curriculum above; they connect that curriculum to the 2026 foundation-model frontier. [S34,S41–S44]

------------------------------------------------------------------------

## 2.3 Modality coverage ledger

This table separates **field maturity** from **audit completeness**. The former describes the scientific ecosystem; the latter describes how deeply this dossier has mapped it.

| Modality / data object | Representative anchors / topics in this dossier | Field-state summary | `audit_completeness` |
|---|---|---|---|
| **CT / CBCT** | Merlin; CT-RATE/CT-CLIP/CT-CHAT; AbdomenNet; OMAFound; DeepSparse; universal CT enhancement; HorusEye | deep 3D representation + application + reconstruction/restoration ecosystem | **deep** |
| **MRI / structural neuroimaging** | Decipher-MR; Prima; NeuroVFM; Triad; Brainfound; MedGemma 1.5; fast-MRI semantic reconstruction | established but heterogeneous; sequence/protocol/acquisition semantics central | **deep** |
| **fMRI / 4D neuroimaging** | NeuroSTORM | credible native spatiotemporal branch distinct from structural MRI | **substantial** |
| **CXR** | RAD-DINO; Ark/Ark+; CLEAR; MAIRA-2; CARE-X; disease-phenomics FM | deep representation, heterogeneous-supervision, grounding/reporting and systemic-phenotyping ecosystem | **deep** |
| **Ultrasound / echocardiography** | EchoPrime; EchoCLIP; Echo-Vision-FM; Sonomate; breast-US generative FM | established specialty branch spanning acquisition interaction and study-level video reasoning | **substantial** |
| **Ophthalmic OCT / fundus** | OCTCube-M; volumetric V-JEPA; RETFound Plus; FOCUS | established volumetric, longitudinal and workflow-level ecosystem | **deep** |
| **Dermatology / dermoscopy / TBP / dermatopathology** | PanDerm | established multimodal specialty branch | **substantial** |
| **Mammography** | multiview image-report pretraining; MammoScope watchlist; shortcut-robustness evidence | important multiview/screening branch; dataset/device/site structure highly consequential | **partial** |
| **Musculoskeletal radiography** | SKELEX | strong modality-specific representation branch | **partial** |
| **Dental / oral imaging** | PanoFM; DentFound; DentVLM | rapidly developing specialty branch across panoramic and multimodal dental imaging | **substantial** |
| **Endoscopy** | GastroNet-5M; emerging whole-case/video systems | active procedure/video branch; procedure-level leakage central | **partial** |
| **PET/CT** | open whole-body PET/CT FM; Dual-Stream DINOv3; autoPET | credible but emerging functional/anatomical multimodal branch | **substantial** |
| **SPECT** | no dedicated promoted anchor | **field maturity not concluded from this dossier** | **gap** |
| **Histopathology / WSI** | Prov-GigaPath; Virchow2; UNI2-h; H-Optimus; TITAN; PRISM2; nnMIL; CARE; PathSegmentor; CRISP | deepest competitive FM ecosystem in this dossier, with unusually rich independent and prospective evidence | **deep** |
| **Spatial proteomics / virtual spatial biology** | GigaTIME; VirTues | distinct cross-modal inference and measured-multiplex representation frontiers | **substantial** |
| **Biomedical microscopy / X-ray tomography** | UniFMIR; FluoResFM; HorusEye | established/developing restoration and inverse-imaging branch | **substantial** |
| **Radiotherapy imaging / planning objects** | CT + RTSTRUCT ROI interoperability; DICOM spatial semantics | emerging FM-enabled interoperability/spatial-safety workflow branch | **partial** |

**Interpretation rule:** a branch marked `partial`, `scaffold` or `gap` must not be ranked against a deeply audited branch on the basis of this dossier alone. In particular, **SPECT remains an audit gap**, not a conclusion that the SPECT AI field is immature.

------------------------------------------------------------------------

# 3. The 2026 Medical-Imaging AI Landscape: Three Orthogonal Axes

A reliable ground layer needs more than a model catalogue. The field should be read simultaneously along three axes:

1. **where in the imaging/clinical pipeline the problem sits**;
2. **what learning/data paradigm is being used**;
3. **what clinical role or endpoint is being served**.

A future project can live anywhere in this coordinate system. Foundation models occupy many cells, but they do not define the coordinate system.

## 3.1 Axis A — imaging problem stack

```text
RAW MEASUREMENT / CLINICAL STUDY
        |
        +--> A. Acquisition, protocol adequacy, quality control, harmonization
        |
        +--> B. Reconstruction, enhancement, restoration, synthesis
        |
        +--> C. Registration, motion and anatomical correspondence
        |
        +--> D. Detection, segmentation and spatial parsing
        |
        +--> E. Representation / retrieval / multimodal alignment
        |
        +--> F. Diagnosis, reporting, grounding and quantitative measurement
        |
        +--> G. Longitudinal matching, progression and response assessment
        |
        +--> H. Quantitative imaging / radiomics / phenomics / prognosis
        |
        +--> I. Planning, intervention and spatial-object workflows
        |
        +--> J. Deployment, monitoring, privacy, security and lifecycle update
```

This prevents a common bias: beginning from a fashionable model family and searching for somewhere to apply it. The clinically important bottleneck may instead be acquisition QC, domain shift, annotation, registration, calibration or workflow integration.

## 3.2 Axis B — learning and data paradigms

| Paradigm | What it is solving | Current landscape role | Key evidence anchors |
|---|---|---|---|
| **Task-specific supervised learning** | highest performance on a defined endpoint when labels are sufficient | remains the essential specialist reference; not displaced by FMs | nnU-Net lineage [S9] |
| **Label-efficient / data-centric learning** | scarce, expensive, heterogeneous or weak labels | active cross-cutting ecosystem: semi/weak supervision, active learning, pseudo-labels, heterogeneous-label aggregation | 350+ study 2026 survey [S108]; Ark/Ark+ [S106,S107] |
| **Self-supervised / representation pretraining** | reusable visual features without dense task labels | foundational layer across radiology, pathology, ophthalmology, echo and microscopy | RAD-DINO, Triad, pathology FMs, etc. |
| **Image–text / multimodal pretraining** | align visual findings with reports, concepts or instructions | major route to retrieval, grounding, reporting and multimodal reasoning | CT-CLIP, Merlin, ConceptCLIP, MAIRA lineage |
| **Promptable/generalist models** | flexible spatial/task specification without full retraining | established for segmentation; expanding into pathology and multimodal interfaces | VISTA3D, MedSAM2, PathSegmentor |
| **Domain generalization / adaptation / OOD / test-time adaptation** | site, scanner, protocol and population shift | active robustness ecosystem; pretraining alone does not solve transportability | DG review [S109] |
| **Continual / incremental learning** | update models under new sites, classes, tasks or temporal drift without catastrophic forgetting | active model-evolution frontier with healthcare-specific constraints | MedIA review [S110] |
| **Synthetic / generative data** | augmentation, rare cases, privacy, counterfactuals, simulation, restoration/synthesis | broad data strategy beyond generative FMs; evaluation still fragmented | systematic review [S111] |
| **Federated / distributed learning** | learn across governed sites without centralizing raw data | important systems paradigm; not a privacy theorem | [S69,S101] |
| **Human-in-the-loop / interactive AI** | combine human expertise with model assistance | central for annotation, reporting, intraoperative pathology, ultrasound acquisition and clinical decision support | MedSAM2, CRISP, Sonomate |
| **Tool-augmented / agentic workflows** | delegate deterministic computation, retrieval or orchestration to external tools | emerging systems frontier | CARE-X tool study; radiotherapy workflow |

### Ground-layer rule

When later evaluating a project direction, compare it against **all plausible paradigms** in the same problem cell. A foundation model is not the default comparator merely because this is 2026.

## 3.3 Axis C — clinical-task and use-role landscape

The same modality can support fundamentally different scientific/clinical endpoints. A compact task map is therefore necessary.

| Modality / object | Acquisition / QC | Reconstruction / registration | Detection / segmentation | Diagnosis / report / grounding | Quantitative biomarker / prognosis / screening | Longitudinal / workflow / intervention |
|---|---|---|---|---|---|---|
| **CT / CBCT** | active | **deep** | **deep** | **deep** | **active** | active |
| **MRI / fMRI** | active | **deep** | **deep** | active | **active** | active / spatiotemporal |
| **CXR** | active | limited | **deep** | **deep** | **deepening phenomics/opportunistic-risk branch** [S112] | active longitudinal/reporting |
| **Ultrasound / echo** | **central** | acquisition-dependent | active | **deepening study-level reasoning** | active | **operator/workflow interaction** |
| **Ophthalmology** | device/QC active | limited | active | active | **risk/progression strong** | **longitudinal strong** |
| **Mammography** | device/QC central | limited | active | active | screening/risk central | priors central |
| **PET/CT / SPECT** | tracer/reconstruction central | central | active in PET/CT | active | quantitative uptake/prognosis central | response assessment central |
| **Pathology / WSI** | stain/scanner/QC central | spatial alignment relevant | **deep** | **deep** | molecular/prognostic deep | intraoperative/workflow emerging-to-strong |
| **Microscopy / tomography** | acquisition physics central | **restoration deepening** | active | task-specific | quantitative morphology central | experimental workflow |
| **Radiotherapy objects** | protocol/geometry central | registration central | contouring central | semantic object correctness | dose/response linked | **planning/intervention central** |

`deep` here means the dossier contains a substantial evidence ecosystem for that cell; `active` means a meaningful research/engineering branch exists. This matrix is **not** a ranking of project value.

## 3.4 Cross-cutting adjacent frontiers that must remain visible

### 3.4.1 Label-efficient and data-centric medical imaging

Expert labels remain expensive, inconsistent and institution-specific. The 2026 *Medical Image Analysis* survey synthesizes **350+ studies** across label-efficient regimes and explicitly treats health foundation models as one contributor inside a larger supervision landscape [S108]. Relevant families include weak supervision, semi-supervised learning, active learning, pseudo-labeling, partial labels and heterogeneous-label aggregation.

**Landscape significance:** a future system may gain more from better supervision/data curation than from a larger backbone. Ark/Ark+ is a particularly important CXR example because it builds a reusable model by accruing heterogeneous expert labels without manual label consolidation, demonstrating a supervised route to foundation-model behaviour distinct from SSL or image–text pretraining [S106,S107].

### 3.4.2 Robust adaptation, domain shift and model evolution

Medical imaging has unusually strong domain shift from scanner vendors, acquisition protocols, reconstruction kernels, populations, sites and temporal practice changes. Domain generalization has a substantial dedicated literature [S109], while continual learning addresses the additional requirement that deployed systems must evolve under new domains/tasks/classes without catastrophic forgetting [S110].

The relevant landscape includes:

```text
domain generalization
source/target domain adaptation
source-free adaptation
test-time adaptation
OOD detection / selective prediction
harmonization / invariant representation
continual / domain-incremental learning
drift monitoring and recalibration
```

These are not merely evaluation techniques. They are competing technical strategies for making medical-imaging AI usable outside a static development cohort.

### 3.4.3 Synthetic and generative data as a data strategy

Synthetic imaging should be separated from generative-FM architecture. A 2026 systematic review of publicly released synthetic medical-image resources spans radiology, pathology, ophthalmology and dermatology and highlights persistent lack of standardized cross-study evaluation [S111].

The data-strategy landscape includes:

- augmentation and class balancing;
- rare-disease / rare-anatomy generation;
- privacy-preserving data sharing claims;
- counterfactual and bias analysis;
- paired image-to-image translation;
- simulation for acquisition/reconstruction;
- synthetic pretraining and pseudo-cohort construction.

**Ground-layer warning:** realism/fidelity, downstream utility, privacy leakage and biological/clinical validity are different endpoints. A visually plausible synthetic image is not automatically useful or private.

### 3.4.4 Acquisition quality, artifact detection and harmonization

Image-quality assessment is an active medical-imaging AI problem in its own right. A 2025 systematic review summarizes automated/ML-based medical image quality assessment and emphasizes variability in quality labels, datasets and standardization [S115]. This layer includes protocol adequacy, motion/artifact detection, series selection, scanner harmonization, repeat-scan decisions and automated rejection/abstention.

**Landscape significance:** downstream model quality cannot compensate for an acquisition or geometry failure that the pipeline never detects.

### 3.4.5 Quantitative imaging, radiomics, phenomics and opportunistic prediction

Medical-imaging AI is not limited to reproducing a radiologist's visible finding labels. Quantitative imaging/radiomics seeks reproducible image-derived biomarkers for diagnosis, molecular characterization, prognosis, treatment response and precision medicine; 2026 reviews continue to identify standardization, reproducibility, domain shift and prospective validation as central barriers [S116].

A distinct CXR example shows how FM embeddings can extend this idea into **disease phenomics**: a 2026 study evaluated 1,074 EHR-derived phecodes across three large independent cohorts and found reproducible prevalent/incident systemic-disease signals in CXR embeddings, with radiologist-curated feature co-embedding used for interpretation [S112]. This should be read as an imaging-biomarker/opportunistic-prediction frontier, not as proof of causal disease mechanisms or deployment utility.

------------------------------------------------------------------------

# 3A. Evidence-Audited Dataset and Benchmark Landscape

This section was introduced in the 9 September 2026 hostile audit and re-audited on 11 September 2026. It is intentionally **not** a list of “largest” datasets. It separates **pretraining resources, task benchmarks, spatial/grounded derivatives, external/OOD cohorts, and restricted corpora**, because those roles are scientifically different.

## 3A.1 Dataset numerical hygiene

Before comparing scale, write the counting unit. In this dossier:

> **patient ≠ study/exam ≠ series ≠ reconstruction ≠ 2D image/slice ≠ WSI ≠ tile/patch ≠ annotation pair**

Examples:

- **CT-RATE** contains 25,692 non-contrast chest CT **studies/scans** from 21,304 patients, expanded to 50,188 **reconstructed volumes**; the reconstruction-expanded number must not be read as 50,188 independent examinations. [D20]
- **MR-RATE** currently reports 83,425 patients, 98,334 MRI studies, and 705,254 MRI series/volumes. The dataset card is an official live release, while its paper/model are still marked “coming soon”; this is therefore an important **E2 emerging data resource**, not peer-reviewed proof of an MR-RATE model. [D23]
- **MIMIC-CXR** contains 377,110 images from 227,835 radiographic studies; images and studies are not interchangeable. Access requires a data-use agreement. [D12]

Any future table that gives a number without its unit should be treated as defective.

## 3A.2 Core segmentation and spatial-annotation resources

| Dataset / benchmark                                                                                      | Verified role and scale                                                                                                                                                                                                                                | Access / provenance                                                                                                                             | Appropriate use                                                                  | Red-team caveat                                                                                                                                                 |
|:---------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **CVPR 2025–2026 Foundation Models for Text-guided 3D Biomedical Image Segmentation / CVPR-BiomedSegFM** | Official challenge materials describe **>200,000 3D image–mask pairs** spanning CT, MRI, PET, ultrasound and microscopy; the Hugging Face dataset card identifies a 3D segmentation resource and CC-BY-NC-SA-4.0 metadata. [D1,D2]                  | Official challenge + HF release; challenge documentation also states that redistribution constraints apply to many constituent public datasets. | Universal/text-guided 3D segmentation, efficiency, modality-spanning evaluation. | Do not treat the aggregate as provenance-free. Source-dataset overlap with model pretraining must be audited.                                                   |
| **FLARE PanCancerCTSeg 2026**                                                                            | Active MICCAI 2026 pan-cancer CT challenge resource with **>17,000 labelled cancer CT scans** aggregated from many source datasets. [D3]                                                                                                            | Gated HF challenge resource; constituent datasets have **mixed source licences**.                                                               | Large-scale cancer segmentation and low-resource/generalization studies.         | MICCAI 2026 is still future at this snapshot. The challenge is active/current, not a completed conference result. Aggregation can create hidden source overlap. |
| **AbdomenAtlas**                                                                                         | Peer-reviewed base resource: **20,460 3D CT volumes from 112 hospitals** and **673K anatomical masks**; 22 structures were manually annotated in 5,246 CTs, with semi-automatic radiologist revision for the remainder. [D4]                         | Multicentre research dataset; exact downloadable subset/version terms must be checked at use time.                                              | Abdominal transfer learning, segmentation, large-scale external testing.         | “673K masks” is not 673K independent scans. Human/manual and AI-assisted-revised annotation stages differ.                                                      |
| **AbdomenAtlas 3.0**                                                                                     | ICCV 2025: **9,262 CT–mask–report triplets**, 3,955 with tumours, from 17 public datasets. Reports were created with **RadGPT from radiologist-revised tumour masks and radiologist verification**, not simply copied routine clinical reports. [D5] | Public research release linked by the paper/repository.                                                                                         | Segmentation-assisted report generation, tumour localization + language.         | Report generation is mask-informed; using these reports to claim unconstrained image-only reporting can leak segmentation-derived information.                  |
| **TotalSegmentator (original dataset/paper)**                                                            | **1,204 CT examinations, 104 structures** in the original peer-reviewed release. [D6]                                                                                                                                                                | Public annotated dataset/toolkit; current toolkit versions may contain more labels/data than the original paper.                                | Mature whole-body anatomy reference and external test source.                    | Always pin dataset/toolkit version; do not silently apply the original 1,204/104 figures to later releases.                                                     |
| **Medical Segmentation Decathlon**                                                                       | Ten heterogeneous segmentation tasks; public under **CC-BY-SA 4.0**. [D7]                                                                                                                                                                            | Open benchmark.                                                                                                                                 | Mature baseline/generalization suite.                                            | Old and heavily reused: contamination/pretraining overlap is plausible.                                                                                         |
| **LUNA16**                                                                                               | **888 CT scans** from LIDC-IDRI; reference nodules are ≥3 mm and accepted by at least 3 of 4 radiologists. [D8]                                                                                                                                      | CC BY 4.0 challenge release.                                                                                                                    | Lung nodule detection/localization benchmark.                                    | Derived from LIDC-IDRI; LUNA16 and LIDC-IDRI are not independent cohorts.                                                                                       |
| **KiTS23**                                                                                               | **599 cases**: 489 training and 110 held-out test cases; kidney, tumour and cyst segmentation. [D9]                                                                                                                                                  | Public challenge training data; held-out test data/labels are governed by challenge procedures.                                                 | Kidney/tumour/cyst CT segmentation.                                              | Do not describe all 599 as openly labelled training cases.                                                                                                      |
| **AMOS22**                                                                                               | **500 CT + 100 MRI** scans with 15 abdominal organ annotations. [D10]                                                                                                                                                                                | Challenge/public research resource.                                                                                                             | Multimodal abdominal segmentation.                                               | Mature benchmark; source/pretraining reuse should be checked.                                                                                                   |
| **autoPET**                                                                                              | Training resource: **1,014 PET/CT studies from 900 patients** with manually segmented tumour lesions; challenge also uses held-out tests. [D11A]                                                                                                     | Public challenge training data via TCIA.                                                                                                        | Whole-body FDG-PET/CT tumour segmentation.                                       | Training data are single-site; the test design, not training count alone, carries generalization evidence.                                                      |

### Interpretation

For **R1/S3-style segmentation research**, the strongest design is not “train and test on the biggest aggregate.” It is:

1.  establish a task-specific supervised reference (nnU-Net v2/ResEnc or equivalent);
2.  use an aggregated resource such as CVPR-BiomedSegFM or FLARE for breadth;
3.  explicitly trace constituent source datasets;
4.  reserve genuinely independent centres/cohorts for external testing;
5.  report prompt effort, annotation effort, and compute alongside Dice/NSD.

## 3A.3 Chest radiography: mature but highly derivative ecosystem

| Dataset              | Verified facts                                                                                                                                                                                     | Label / access provenance                                                 | Correct scientific role                                                    |
|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------|:---------------------------------------------------------------------------|
| **MIMIC-CXR v2.1.0** | 377,110 DICOM images, 227,835 studies, free-text radiology reports. [D12]                                                                                                                        | Credentialed/DUA access.                                                  | Central CXR image–report and longitudinal pretraining/evaluation resource. |
| **CheXpert**         | 224,316 chest radiographs from 65,240 patients with associated reports; canonical 14-observation benchmark. [D13]                                                                                | Stanford dataset; report-derived labels.                                  | Large CXR classification / representation benchmark.                       |
| **PadChest**         | >160,000 images from 67,000 patients; 174 findings, 19 differential diagnoses, 104 anatomical locations; 27% reports manually annotated, remainder labelled by a supervised RNN pipeline. [D14] | Public research dataset; mixed manual/automated label provenance.         | Cross-language/site CXR evaluation and representation learning.            |
| **NIH ChestX-ray14** | 112,120 frontal CXRs from 30,805 patients; 14 labels mined from reports with NLP/rule-based methods. [D15]                                                                                       | Public; weak report-mined labels.                                         | Historical classification benchmark.                                       |
| **VinDr-CXR**        | 18,000 released PA CXRs; 17 radiologists; 22 local bounding-box findings + 6 global diagnoses; 15,000 train / 3,000 test. [D16]                                                                  | Radiologist annotations; test labels produced by multi-reader consensus.  | Stronger localization/grounding reference than weak-label CXR sets.        |
| **MS-CXR**           | 1,162 image–sentence/bounding-box pairs across 8 findings, curated on a **subset of MIMIC-CXR**. [D17]                                                                                           | Radiologist phrase grounding; credentialed parent data constraints apply. | Phrase-grounding benchmark.                                                |
| **Chest ImaGenome**  | Automatically derived scene graphs for 242,072 frontal MIMIC CXRs, >670K localized comparison relations, plus a manually annotated 500-patient gold subset. [D18]                               | **Derivative of MIMIC-CXR**; credentialed access.                         | Anatomy-centred scene-graph and longitudinal relation research.            |
| **BRAX**             | 24,959 studies, 40,967 images, 19,351 patients; 14 labels derived from Portuguese reports with NLP. [D19A]                                                                                       | Credentialed/DUA PhysioNet resource; NLP-derived labels.                  | Geographic/site-shift CXR evaluation.                                      |

### Genealogy rule

**MIMIC-CXR → MS-CXR / Chest ImaGenome** is a parent→derivative relationship. A model pretrained on MIMIC-CXR and evaluated on MS-CXR has not demonstrated independent institutional generalization merely because the task labels are new.

## 3A.4 Open 3D image–language and grounded radiology resources

This is one of the most strategically important data fronts in 2026, but it requires unusually strict provenance control.

| Dataset                         | Verified state                                                                                                                                                                                                                                | Evidence / access                                                                                                       | What it enables                                                                                         | What it does **not** prove                                                                                                                                                        |
|:--------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **CT-RATE**                     | Peer-reviewed: **25,692 non-contrast 3D chest CT scans/studies from 21,304 patients**, each with a radiology report; the live dataset card additionally describes expansion to **50,188 reconstructed volumes**. [S13,D20]                  | Peer-reviewed paper + official gated/non-commercial HF dataset.                                                         | 3D CT image–text pretraining, retrieval, abnormality classification, report/QA work.                    | The 50,188 reconstructions are not 50,188 independent studies.                                                                                                                    |
| **Merlin abdominal CT dataset** | The 2026 *Nature* data-availability statement reports a released cohort of **25,494 abdominal/pelvic CT scans from 18,317 unique patients**, each paired with a radiology report. [S12,D30]                                                 | Stanford AIMI; DUA approval required.                                                                                   | Independent abdominal CT image–report research and an anatomy/site counterweight to chest-only CT-RATE. | Distinguish the **released dataset cohort** from the model’s specific pretraining/evaluation subsets reported elsewhere in the paper; the numbers are not interchangeable.        |
| **RAD-ChestCT**                 | Duke chest-CT cohort used as an **external validation set** in the CT-RATE/CT-CLIP paper. The Zenodo record describes a full cohort of 35,747 CT scans / 19,661 adults but an initial public release of **3,630 scans**. [S13,D31]          | Zenodo release with access/licence conditions; independent institution from CT-RATE.                                    | Genuine cross-institution CT external validation, subject to task/label compatibility.                  | Do not report the 35,747 full-cohort size as if all images are in the open Zenodo release.                                                                                        |
| **RadGenome-ChestCT**           | Peer-reviewed Scientific Data descriptor: CT-RATE-derived, 197 segmentation categories, **665K grounded reports and 1.2M grounded VQA pairs**. The live HF card may show a later **1.3M** VQA count; both should be version-labelled. [D21] | Derived dataset using segmentation + LLM/NER pipelines.                                                                 | Region-grounded 3D language/VQA supervision.                                                            | Not an independent cohort; not all grounding/VQA labels are native human annotations.                                                                                             |
| **PatchChestCT**                | Published 12 Aug 2026: **2,201 physician-reviewed CT studies** from CT-RATE with patch-level annotations for nine abnormalities. [D22]                                                                                                      | Annotation files on Zenodo under CC BY 4.0; underlying CT-RATE volumes must be obtained separately under CT-RATE terms. | Human-reviewed spatial supervision for 3D localization.                                                 | Authors explicitly recommend it primarily as a training resource rather than a strict performance benchmark; prevalence is intentionally enriched and it remains CT-RATE-derived. |
| **AbdomenAtlas 3.0**            | 9,262 abdominal CT–mask–report triplets; 3,955 tumour-positive. [D5]                                                                                                                                                                        | Reports are RadGPT/mask-assisted and radiologist-reviewed.                                                              | Abdominal tumour grounding/reporting with voxel masks.                                                  | It is not a routine-report cohort and should not be pooled with CT-RATE reports without modelling provenance differences.                                                         |
| **MR-RATE**                     | Live official release (18 Mar 2026): **705,254 brain/spine MRI series/volumes, 98,334 studies, 83,425 patients**, with reports/metadata and registration/segmentation derivatives. [D23]                                                    | Gated CC-BY-NC-SA-4.0; official dataset card. **Paper/model are still marked “coming soon.”**                           | Large-scale MRI+language data engineering, representation pretraining, sequence-aware research.         | No peer-reviewed MR-RATE model result should be claimed yet. Derived NV-Segment-CTMR masks are model predictions, not human segmentation ground truth.                            |

### Critical genealogy map

``` text
CT-RATE (parent clinical cohort)
   ├── RadGenome-ChestCT   [synthetic/model-assisted grounding + VQA]
   └── PatchChestCT        [human-reviewed patch localization subset]

MIMIC-CXR (parent CXR cohort)
   ├── MS-CXR              [radiologist phrase grounding subset]
   └── Chest ImaGenome     [automatic scene graphs + small gold subset]
```

These derivatives are scientifically valuable, but they are **not independent patient cohorts**. They are best viewed as new supervision layers over an existing cohort.

## 3A.5 Reconstruction / inverse-imaging data

**fastMRI** remains a core open inverse-problem resource: the official project reports raw data from **>1,500 fully sampled knee MRIs** and DICOM images from **10,000 clinical knee MRIs**. [D24] Its importance is methodological: it exposes raw measurement-domain data, which allows reconstruction research to be evaluated as an inverse problem rather than only as post-hoc image enhancement.

For CT reconstruction/enhancement, dataset identity must be read from the individual paper. For example, TAMP’s 2026 peer-reviewed work builds a physics-driven simulated pretraining corpus from ten public CT datasets; that is a different evidence regime from a raw-projection clinical benchmark. [S34]

## 3A.6 Computational pathology: benchmark abundance does not remove contamination risk

| Resource                  | Correct role                                                                                                                      | Access / provenance                                                                  | Red-team requirement                                                                                                             |
|:--------------------------|:----------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------|
| **TCGA**                  | Pan-cancer WSI plus molecular/clinical ecosystem; NCI states diagnostic and tissue WSIs are open through GDC. [D25]             | Imaging slides open; other molecular/genomic data have mixed open/controlled levels. | Treat TCGA primarily as **in-domain/common-public** for FM benchmarking because many models may have seen it during pretraining. |
| **CPTAC**                 | Proteogenomic cancer program with pathology/imaging and molecular data distributed across NCI data commons. [D26]               | Access level varies by data type; some genomic files require controlled access.      | Do not describe “CPTAC” as one monolithic dataset with one licence. Confirm cancer cohort and data commons.                      |
| **CAMELYON17**            | 1,000 lymph-node WSIs from **5 Dutch medical centres** (500 train, 500 test) with expert-prepared ground truth structure. [D27] | Open access, CC0.                                                                    | Useful multicentre shift benchmark, but still check whether a model’s pretraining included CAMELYON.                             |
| **PANDA**                 | Prostate WSI grading from Radboud UMC and Karolinska; providers used different scanners/pathology workflows. [D28]              | Kaggle competition terms; non-commercial academic/competition use.                   | Strong site/scanner contrast, but competition labels/masks have their own annotation caveats.                                    |
| **External patch suites** | BRACS, BACH, UnitoPatho, SICAPv2, BreakHis, LC2500, MHIST, NCT-CRC-HE appear in the 2026 32-model benchmark. [S28]              | Public heterogeneous datasets.                                                       | Use as a suite, not as proof that any single one is a comprehensive OOD test.                                                    |

The July 2026 benchmark explicitly notes that TCGA likely served as pretraining data for many evaluated models and that public-dataset overlap cannot be fully excluded. It therefore separates TCGA from non-TCGA evaluation and still avoids claiming guaranteed pretraining exclusion for every OOD set. [S28]

## 3A.7 Spatial biology: model openness and data openness diverge

The **GigaTIME** repository exposes a useful warning for the whole field: the model/code are available, but the public repository provides only **50 paired H&E/mIF test patches** for evaluation rather than the full large-scale paired training corpus. [D29] Therefore:

- reproducible inference ≠ reproducible pretraining;
- an open checkpoint ≠ an open paired biological dataset;
- R7/S6-style extensions may be data-limited even when the model is accessible.

## 3A.8 Dataset-role matrix for landscape interpretation

The purpose of this table is to show **what each dataset family contributes to understanding the field**, not to prescribe a research program.

| Landscape branch | Important starting resources | What these resources establish | What they cannot establish by themselves |
|---|---|---|---|
| **3D segmentation / promptability** | CVPR-BiomedSegFM, FLARE PanCancerCTSeg, AbdomenAtlas, TotalSegmentator, MSD, AMOS, autoPET | breadth of anatomy/modality/task coverage; comparison of supervised and generalist segmentation paradigms | true external generalization if constituent datasets overlap with pretraining; human-efficiency benefits unless directly measured |
| **Native 3D CT/MRI representation** | CT-RATE; Merlin released abdominal CT–report dataset; MR-RATE; AbdomenAtlas 3.0 | scale and structure of volumetric image–language pretraining resources; distinct CT/MRI data regimes | causal benefit of 3D dimensionality; independent generalization when derivative resources share the parent cohort |
| **3D grounding / spatial supervision** | CT-RATE + RadGenome-ChestCT + PatchChestCT; AbdomenAtlas 3.0 | dense language/spatial supervision and human-reviewed localization layers | external-cohort validation for CT-RATE-trained models because RadGenome/PatchChestCT are derivatives |
| **CXR representation / grounding / reporting** | MIMIC-CXR, CheXpert, PadChest, VinDr-CXR, MS-CXR, Chest ImaGenome, BRAX | unusually mature image-report, weak-label, grounding and geographic-shift ecosystem | independence when derivative annotations inherit MIMIC-CXR or other parent cohorts |
| **Pathology robustness and model selection** | TCGA, CPTAC, CAMELYON17, PANDA, PathoROB and external benchmark suites | one of the deepest independent-comparison and domain-shift ecosystems in medical imaging | guaranteed OOD status when public datasets may have appeared in FM pretraining |
| **Spatial biology** | GigaTIME resources/checkpoint/sample data; measured multiplex datasets used by VirTues | contrast between morphology-to-marker inference and direct measured spatial-proteomics modelling | reproducible pretraining when full paired data are unavailable; clinical truth from virtual markers without physical validation |
| **Inverse imaging / restoration** | fastMRI; paper-specific CT/CBCT and tomography resources | measurement-domain versus post-reconstruction paradigms; physics-aware evaluation requirements | interchangeability across scanner geometry, acquisition physics or simulated versus real degradation |

### Landscape interpretation rule

The most informative dataset is not necessarily the newest or largest. Its value depends on **genealogy, counting unit, label/reference-standard provenance, acquisition context, access constraints, and whether it provides genuinely new externality** relative to the models being evaluated.


## 3A.9 Datasets that are important but should not be over-promoted

- **MIMIC-CXR, TCGA, CheXpert and TotalSegmentator** are foundational community anchors, but their ubiquity increases contamination risk.
- **FLARE PanCancerCTSeg 2026** is current and large, but “hot” does not equal scientifically superior; it is an active challenge aggregate with heterogeneous source licences and likely source overlap with other public corpora.
- **MR-RATE** is strategically important because of scale and paired reports, but its current scientific status is an official live dataset release, not a peer-reviewed model paper.
- **RadGenome-ChestCT** is valuable for grounded supervision but much of the supervision is pipeline-generated; it must not be described as 1.2/1.3M independently human-authored VQA examples.
- **PatchChestCT** improves spatial-label quality but its authors caution against treating it as a strict prevalence-matched benchmark.
- **AbdomenAtlas 3.0** bridges segmentation and reporting, but its reports are mask-assisted/revised, which is a feature for some experiments and a leakage risk for others.

This distinction is central to becoming expert in the field: **the scientifically most informative dataset is the one whose provenance, supervision, cohort independence and access constraints fit the field-state question—not necessarily the newest or largest dataset.**

## 3A.10 Dataset-ledger completeness boundary

Section 3A is **deepest for segmentation, CXR, volumetric CT/MRI and pathology**. It is not yet an equally deep dataset genealogy for every modality named in Section 2.3.

Before claiming complete modality expertise, extend the same provenance template to:

- ultrasound: patient/exam/video/frame hierarchy, operator identity, machine/vendor and acquisition protocol; for echocardiography, explicitly model **study-level multi-view structure** and shared institutional lineage (e.g. EchoCLIP/EchoPrime both draw from Cedars-Sinai archives rather than independent pretraining ecosystems);
- ophthalmology: patient/eye/visit/volume/B-scan hierarchy and longitudinal eye-level leakage;
- dermatology: patient/visit/lesion/image hierarchy; longitudinal lesion identity; total-body-photo tile genealogy; skin tone, geography, camera/device and pathology-confirmation provenance;
- biomedical microscopy: specimen/cell/field/patch hierarchy; microscope/objective/acquisition settings; low/high-quality pairing provenance; synthetic degradation versus physically paired restoration targets;
- PET/CT (and any future SPECT audit): tracer, uptake time, injected activity, reconstruction, attenuation correction, modality pairing and lesion/reference-standard provenance;
- mammography: patient/breast/view/prior-exam structure and screening-versus-diagnostic cohort shift;
- endoscopy: patient/procedure/video/frame hierarchy and procedure-level leakage;
- musculoskeletal radiography: examination/view/body-region hierarchy;
- radiotherapy: CT, RTSTRUCT, RTDOSE, RTPLAN linkage, ROI naming conventions, contour provenance and treatment-planning-system/site effects.

**Rule:** a modality is not “dataset-audited” merely because one flagship dataset is named. It needs a genealogy, counting-unit discipline, label/reference-standard provenance, access terms and an explicit external-validation strategy.


## 3A.11 Priority modality genealogy extensions

The following mini-ledger closes the largest **structural** gaps without pretending that every underlying private training cohort is independently downloadable. It distinguishes the required counting hierarchy, known anchor cohorts/resources, and the next provenance audit that would be needed for full dataset-level mastery.

| Modality | Required hierarchy | Audited anchor cohort/resource | Main provenance / leakage hazard | Next dataset-level audit |
|---|---|---|---|---|
| **Echocardiography** | patient -> study -> video -> view -> frame | EchoCLIP/EchoPrime Cedars-Sinai lineages; EchoPrime external health systems [S83,S84] | shared institutional pretraining lineage; repeated studies; view imbalance; report-label extraction differs by site | create site-by-site cohort genealogy and verify patient overlap/external-label provenance |
| **Ophthalmology** | patient -> eye -> visit -> volume/photo -> B-scan | OCTCube-M; RETFound Plus; volumetric OCT benchmarks [S45,S46,S61] | bilateral-eye correlation; longitudinal near-duplicates; device/site shift; systemic-outcome confounding | pin every public retinal/OCT dataset used by each anchor and audit eye-level split rules |
| **Dermatology** | patient -> visit -> lesion -> modality/image | PanDerm 2.15M-image multimodal specialty corpus [S88] | same lesion across clinical/dermoscopic/TBP views; skin-tone/geography/device imbalance; pathology-confirmation heterogeneity | reconstruct the 11-source genealogy and benchmark-specific pretraining overlap |
| **Microscopy** | specimen -> acquisition -> field -> cell/structure -> patch | UniFMIR 14-dataset lineage; FluoResFM paired low/high-quality restoration corpora [S89,S100] | synthetic vs physically paired degradation; repeated structures/fields; microscope/objective dependence | pin all public training/test datasets and distinguish physical pairing from synthetic degradation |
| **Mammography** | patient -> breast -> examination -> view -> image | ten-dataset shortcut study; multiview FM + MammoScope watchlist [S50,S85,S99] | screening-vs-diagnostic shift; bilateral/view dependence; site/device identity; prior-exam leakage | build a dataset-by-country/device/task matrix and audit patient overlap across public collections |
| **Endoscopy** | patient -> procedure -> video -> frame/clip | GastroNet-5M [S51] | dense within-procedure frame correlation; procedure/site leakage; still-image extraction can inflate N | add procedure-level external datasets and whole-case/video benchmark genealogy |
| **Dental/oral imaging** | patient -> encounter -> tooth/region -> modality/view -> image | PanoFM 127,878 panoramic cases across six centres; DentFound >101k-patient panoramic corpus; DentVLM 20,741 patients/110,447 images across seven modalities [S92–S94] | tooth-level labels nested within patients; repeated panoramic exams; intraoral-view dependence; institution/geography confounding | audit public Dentex/TDD and any released PanoFM/DentFound/DentVLM subsets with patient-level overlap rules |
| **PET/CT** | patient -> study -> tracer -> paired PET/CT reconstruction -> lesion | open whole-body PET/CT FM + autoPET [S76,D11A] | tracer/uptake/reconstruction/SUV variation; paired-modality leakage; lesion-level repeated measures | create tracer/site/reconstruction genealogy and identify true external cohorts |

**Boundary:** these entries are **genealogy scaffolds**, not claims that all private source cohorts are accessible or independently re-verified at row level. Where a paper's private training data are not released, the dossier should say so rather than convert publication-scale numbers into a public-dataset claim.


## 3A.12 Dataset requirements for cross-cutting paradigms

The dataset question changes with the learning paradigm. A ground-layer map should therefore record not only named datasets, but **what kind of dataset structure is required to evaluate each frontier honestly**.

| Cross-cutting paradigm | Dataset structure that matters | Common false inference |
|---|---|---|
| **Label-efficient / active learning** | nested patient-level label budgets; partial/weak labels; annotation timestamps/cost; consistent held-out external test cohort | treating fewer labels as equivalent when annotation quality or case selection differs |
| **Domain generalization / adaptation** | explicit site/vendor/protocol/population domains; enough cases per domain; untouched external domains | calling a random split “domain generalization” |
| **OOD / selective prediction** | prespecified in-distribution and clinically meaningful OOD groups; prevalence and severity labels | using arbitrary corruptions as the only OOD test |
| **Continual learning / drift** | temporally ordered or domain-incremental cohorts; versioned labels/protocols; retained historical evaluation sets | shuffling all timepoints and then claiming continual robustness |
| **Synthetic data** | real training/test cohorts separated from generator training; paired or task-relevant validation; privacy attack sets where privacy is claimed | evaluating synthetic data only by visual realism or FID-like metrics |
| **Acquisition QC / harmonization** | raw/acquisition metadata; artifact/protocol labels; repeated scans or multi-device data where possible | measuring downstream accuracy without verifying whether “correction” preserved quantitative signal |
| **Radiomics / quantitative biomarkers** | standardized acquisition/preprocessing metadata; outcomes/reference standards; external sites; ideally test–retest or repeatability cohorts | treating high-dimensional feature count as independent biological information |
| **Longitudinal / response modelling** | patient-linked serial studies, treatment/intervention timing, censoring/follow-up information | using repeated images without modelling treatment/utilization/identity effects |

This table is not a project recipe. It prevents later novelty claims from being built on a dataset that cannot actually test the phenomenon of interest.

------------------------------------------------------------------------

# 4. Anchor Systems: Evidence-Audited Matrix


## 4.0 Canonical foundation-model system registry

This registry is the **authoritative taxonomy layer for FM-related systems only**. Narrative tables below may use human-readable role descriptions, but FM-related system classes must resolve to one of the four exact enum values here. It is not the master ontology for all medical-imaging AI methods; the broader field is organized by the three axes in Section 3.

| System / resource | `system_class` | `scope` | `foundation_lineage` / taxonomy note |
|---|---|---|---|
| MedSigLIP | `core_fm` | `generalist` | n/a |
| MedImageInsight | `core_fm` | `generalist` | n/a |
| ConceptCLIP | `core_fm` | `generalist` | n/a |
| RAD-DINO | `core_fm` | `modality_generalist` | n/a; CXR-specialized |
| Ark / Ark+ | `core_fm` | `modality_generalist` | supervised heterogeneous-label knowledge accrual across multiple CXR datasets; open lineage [S106,S107] |
| nnU-Net v2 | `companion` | `workflow_specialist` | supervised reference comparator |
| VISTA3D / NV-Segment-CT | `core_fm` | `modality_generalist` | n/a |
| NV-Segment-CTMR | `fm_derived` | `modality_generalist` | VISTA/NV-Segment lineage |
| SAT3D | `core_fm` | `workflow_specialist` | tumour-segmentation specialist |
| MedSAM2 | `core_fm` | `modality_generalist` | SAM2-derived but broadly medically pretrained/adapted |
| BiomedParse v2 | `core_fm` | `generalist` | BoltzFormer/BiomedParse lineage |
| Medical SAM3 | `core_fm` | `generalist` | SAM3 full medical fine-tuning; broad reusable medical segmentation release; preprint |
| CT-CLIP / CT-CHAT | `core_fm` | `modality_generalist` | CT-RATE ecosystem |
| Merlin | `core_fm` | `organ_specialist` | abdominal CT |
| Triad | `core_fm` | `modality_generalist` | 3D MRI |
| Decipher-MR | `core_fm` | `modality_generalist` | 3D MRI |
| Prima | `core_fm` | `organ_specialist` | neuro-MRI |
| NeuroVFM | `core_fm` | `organ_specialist` | neuro CT/MRI |
| Brainfound | `core_fm` | `organ_specialist` | brain CT/MRI |
| MedGemma 1.5 | `core_fm` | `generalist` | general multimodal model with 3D support |
| CLEAR | `core_fm` | `modality_generalist` | CXR |
| MAIRA-2 | `fm_derived` | `workflow_specialist` | grounded CXR reporting model |
| CARE-X | `fm_derived` | `workflow_specialist` | CXR generation/grounding; tool study separate |
| EchoPrime | `core_fm` | `modality_generalist` | echocardiography |
| EchoCLIP | `core_fm` | `modality_generalist` | echocardiography |
| Echo-Vision-FM | `core_fm` | `modality_generalist` | echocardiography |
| Sonomate | `fm_derived` | `workflow_specialist` | BiomedCLIP-derived initialization |
| Breast-ultrasound generative FM | `core_fm` | `organ_specialist` | breast ultrasound |
| DeepSparse | `core_fm` | `workflow_specialist` | sparse-view CBCT reconstruction |
| Vision-language-guided fast MRI reconstruction | `fm_enabled` | `workflow_specialist` | semantic FM prior + physics/data consistency |
| universal non-ideal CT enhancement FM | `core_fm` | `workflow_specialist` | CT enhancement |
| HorusEye | `core_fm` | `modality_generalist` | self-supervised X-ray tomography restoration across multiple restoration tasks [S105] |
| uniGradICON | `core_fm` | `modality_generalist` | registration |
| LUMIR follow-up | `companion` | `workflow_specialist` | registration challenge/benchmark study |
| OCTCube-M | `core_fm` | `modality_generalist` | OCT |
| volumetric V-JEPA OCT benchmark | `companion` | `workflow_specialist` | volumetric-comparison study |
| RETFound Plus | `core_fm` | `modality_generalist` | fundus/longitudinal retina |
| RETFound / VisionFM / broader ophthalmic-FM lineage | `companion` | `modality_generalist` | review/benchmark lineage; named model-specific claims require primary sources |
| FOCUS | `fm_enabled` | `workflow_specialist` | fine-tuned ophthalmic VFM + workflow |
| AbdomenNet clinical system | `fm_derived` | `workflow_specialist` | self-supervised NCCT foundation encoder |
| OMAFound | `core_fm` | `organ_specialist` | breast/lung screening from CT |
| Explainable multiview mammography image-report pretraining | `core_fm` | `modality_generalist` | mammography |
| MammoScope | `core_fm` | `modality_generalist` | abstract-level evidence only |
| GastroNet-5M | `companion` | `modality_generalist` | dataset/pretraining resource |
| LCTfound | `core_fm` | `organ_specialist` | lung CT |
| SKELEX | `core_fm` | `modality_generalist` | MSK radiographs |
| NeuroSTORM | `core_fm` | `organ_specialist` | 4D fMRI |
| Open multicentre whole-body FDG PET/CT FM | `core_fm` | `modality_generalist` | preprint |
| Dual-Stream DINOv3 PET/CT | `fm_derived` | `workflow_specialist` | DINOv3-derived PET/CT segmentation |
| autoPET | `companion` | `workflow_specialist` | dataset/challenge |
| RT ROI interoperability workflow | `fm_enabled` | `workflow_specialist` | LLM/CLIP-assisted radiotherapy workflow |
| PanDerm | `core_fm` | `specialty_generalist` | dermatology across four modalities |
| UniFMIR | `core_fm` | `modality_generalist` | fluorescence-microscopy restoration |
| FluoResFM | `core_fm` | `modality_generalist` | fluorescence-microscopy restoration |
| Prov-GigaPath / tile encoder | `core_fm` | `specialty_generalist` | pathology |
| GigaPath-Flash tile encoder | `core_fm` | `specialty_generalist` | distilled from Prov-GigaPath teacher; reusable tile encoder; preprint evidence |
| Virchow2 / Virchow2G | `core_fm` | `specialty_generalist` | pathology |
| UNI / UNI2-h | `core_fm` | `specialty_generalist` | pathology |
| H-Optimus-1 | `core_fm` | `specialty_generalist` | pathology |
| TITAN | `core_fm` | `specialty_generalist` | pathology |
| PRISM2 | `core_fm` | `specialty_generalist` | pathology |
| nnMIL | `companion` | `workflow_specialist` | WSI aggregation method |
| CARE | `core_fm` | `specialty_generalist` | pathology adaptive-region + molecular guidance |
| PathSegmentor | `core_fm` | `specialty_generalist` | natural-language pathology segmentation across hierarchical tissue/cell targets [S103] |
| CRISP | `core_fm` | `workflow_specialist` | intraoperative frozen-section pathology with retrospective + prospective clinical evaluation [S104] |
| GigaTIME | `fm_derived` | `workflow_specialist` | H&E -> virtual mIF system |
| VirTues | `core_fm` | `specialty_generalist` | measured spatial proteomics |
| XGeM | `fm_derived` | `workflow_specialist` | broad generative architecture, but published validation remains strongly CXR/report-centric; core-FM promotion withheld |
| PanoFM | `core_fm` | `modality_generalist` | panoramic-radiograph reusable encoder [S92] |
| DentFound | `fm_derived` | `workflow_specialist` | specialized panoramic VLM/reporting system [S93] |
| DentVLM | `fm_derived` | `specialty_generalist` | Qwen2-VL-7B-derived dental VLM across seven modalities [S94] |
| MONAI / MONAI Deploy | `companion` | `generalist` | infrastructure/deployment stack |

**Registry rule:** if a future table needs a more descriptive label such as “clinical system,” “benchmark,” or “method,” place that text in a `role/subtype` field—not inside `system_class`.

## 4.1 General medical representation models

| System | System class | Role | Evidence / source | Verified core facts | Availability / caveat |
| :-------------------- | --- | :--------------------------------------------------------- | --------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| **MedSigLIP** | `core_fm` | General image-text embedding model | E2 [S10] | two-tower model with ~400M vision + ~400M text parameters; trained on CXR, dermatology, ophthalmology, histopathology, and CT/MRI slices; intended for zero-shot/data-efficient classification and retrieval | weights/model access governed by Health AI Developer Foundations terms; verify artifact/access/reuse axes at use time; not generative |
| **MedImageInsight** | `core_fm` | General medical image embedding model | E2 [S11] | covers X-ray, CT, MRI, dermoscopy, OCT, fundus, ultrasound, histopathology, mammography; intended for embeddings, retrieval, classification and adaptation | Microsoft describes it as open-source/open-weight; verify exact current licence before redistribution |
| **ConceptCLIP** | `core_fm` | Explainable biomedical image-text-concept representation | E1 [S38] | pretrained with MedConcept-23M (23M biomedical image-text-concept triplets); evaluated across 78 datasets and 10 imaging modalities; uses global image-text plus region-concept alignment; clinician study tested concept explanations across three modalities | broad biomedical scope; concept-level explanations aid audit but do not prove causal reasoning |
| **RAD-DINO** | `core_fm` | CXR-specialized self-supervised representation | E1 peer-reviewed + E2 model release [S53] | important non-generative CXR representation baseline and MAIRA visual lineage | narrower than general medical encoders; useful baseline, not a universal imaging FM |
| **Ark / Ark+** | `core_fm` | CXR heterogeneous-supervision foundation model | E1 [S106,S107] + E2 release | supervised pretraining by cyclically accruing/reusing heterogeneous expert labels across multiple datasets without manual label consolidation; open code/weights; demonstrates an alternative supervised path to broad CXR transfer | CXR-focused evidence; aggregation of public datasets creates provenance/overlap complexity; developer-reported breadth does not replace independent clinical evaluation |

### Expert lesson

Do not jump directly from CNNs to generative VLMs. The reusable **representation layer** is a major part of the field: SSL and image-text encoders are often more appropriate than generative models for classification, retrieval, data curation, active learning, and low-label adaptation. Ark/Ark+ adds another important lesson: broad reusable representations can also emerge from **supervised knowledge accrual across heterogeneously labelled datasets**, so the learning-paradigm map must not collapse all foundation-model progress into self-supervision or image–text pretraining [S106,S107].


### 4.1.1 CXR as an imaging-biomarker / disease-phenomics platform

CXR foundation-model representations are also being used beyond conventional thoracic finding prediction. A 2026 *npj Digital Medicine* study trained linear probes for **1,074 EHR-derived phecodes** and validated the representation across three independent cohorts of 90,911, 79,786 and 60,282 patients, identifying reproducible prevalent and incident disease signals and relating them to radiologist-curated imaging features [S112].

This expands the CXR landscape from `finding classification -> report generation` toward **opportunistic systemic phenotyping and risk prediction**. The evidence should nevertheless remain bounded: association/prediction from a radiograph does not establish causal pathophysiology, clinical net benefit, or prospective screening utility.

------------------------------------------------------------------------
## 4.2 3D segmentation and spatial parsing

| System | System class | Role | Evidence / source | Verified core facts | Critical limitation |
| :---------------------------------- | --- | :------------------------------------------------- | :---------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------ |
| **nnU-Net v2** | `companion` | task-specific supervised comparator | E1/E2 [S9] | self-configuring supervised segmentation framework; modern v2/ResEnc configurations remain essential task-specific comparators | requires task-specific labelled data; not promptable and not a universal FM |
| **VISTA3D / NV-Segment-CT** | `core_fm` | automatic + interactive CT generalist | E1/E2 [S5,S6] | paper reports training on 11,454 volumes and 127 anatomical/lesion classes; current repo metadata exposes 132 CT classes including tumour labels | CT-focused; paper class count and current bundle class count differ because the released bundle evolved |
| **NV-Segment-CTMR** | `fm_derived` | broad automatic CT/MRI generalist | E2 [S7] | fine-tuned from VISTA lineage on >30K CT/MRI scans; current metadata defines 345+ classes; supports CT body, MRI body and MRI brain; automatic only | non-commercial weights; per-class performance varies by modality/training source |
| **SAT3D (Segment Any Tumour 3D)** | `core_fm` | uncertainty-aware whole-body tumour segmentation | E1 [S39] | peer-reviewed Aug 2026; trained on 17,075 3D volume-mask pairs; benchmarked across 11 public datasets; combines shifted-window ViT with critic-guided uncertainty/confidence maps as dense prompts; 3D Slicer plugin provided | tumour-focused; “foundation” generality is bounded by the evaluated organs/modalities/cohorts |
| **MedSAM2** | `core_fm` | promptable 3D-image + video segmentation | E3 + E2 [S4] | fine-tunes SAM2 on >455k 3D image-mask pairs + >76k video frames; large user study reports >85% reduction in manual annotation cost | preprint; do not import unsupported timing/VRAM/Dice claims from unrelated SAM variants |
| **BiomedParse v2** | `core_fm` | text-guided 3D parsing | E2 [S8]; v1 has peer-reviewed lineage | v2 uses BoltzFormer, million-scale pretraining, 200+ 3D anatomies across CT/MRI/US/PET/3D microscopy; won CVPR 2025 text-guided 3D segmentation challenge; built-in object-existence detection | v2 itself should not inherit the peer-review status of v1; repo says 3D inference is slice-by-slice with neighbouring context |
| **Medical SAM3** | `core_fm` | universal text/prompt-driven 2D/3D segmentation | E3 [S3] | full fine-tuning of SAM3 on 33 datasets spanning 10 modalities; paper explicitly shows vanilla SAM3 degrades under medical domain shift without strong geometric prompts | emerging preprint; author-reported frontier, not yet a settled peer-reviewed standard |

### Core landscape tension

The scientifically useful comparison is not “foundation model versus U-Net.” The field is better understood through the trade-off:

> **How much accuracy, calibration, annotation effort, robustness, and adaptation cost are exchanged when moving from a task-specific supervised system to a promptable/generalist system?**

Any benchmark must equalize or explicitly stratify supervision.

------------------------------------------------------------------------
## 4.3 Native volumetric CT/MRI foundation models

This was the largest missing area in the previous dossier.

| System | System class | Modality / role | Evidence / source | Verified scale / capability | Critical limitation |
| :-------------------------------- | --- | :-------------------------------------------------------------------- | :----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CT-RATE / CT-CLIP / CT-CHAT** | `core_fm` | 3D chest CT dataset, contrastive VLM, generative chat model | E1 [S13] | CT-RATE contains 25,692 non-contrast 3D chest CT scans from 21,304 patients with reports; CT-CLIP supports volumetric image-text representation; CT-CHAT is trained with >2.7M generated QA pairs | chest CT and non-contrast dataset scope; synthetic QA may shape behaviour; downstream claims must match protocol |
| **Merlin** | `core_fm` | 3D abdominal CT VLM | E1 [S12] | trained with >6M images from 15,331 CT scans, >1.8M diagnosis codes and >6M report tokens; evaluated across 752 tasks; internal and multi-source external evaluation; model/code and 25,494 CT-report pairs released | abdominal CT-centric pretraining; downstream adaptation varies by task; external success does not imply universal CT coverage |
| **Triad** | `core_fm` | general 3D MRI vision FM for transfer | E1/E2 [S40] | final 2026 *Medical Image Analysis* paper reports ~129K 3D MRI pretraining and evaluation on 25 downstream datasets spanning segmentation, classification and registration; released checkpoints/code | strongest conclusion is modality/task-aligned transfer, not universal MRI intelligence; final paper differs numerically from earlier preprint versions, so cite the final version |
| **Decipher-MR** | `core_fm` | MRI-specific 3D vision-language representation | E1 [S14] | trained on >200k MRI series from >22k studies across anatomy, sequence and pathology; combines SSL with report-guided supervision; frozen encoder evaluated on classification, segmentation/localization and retrieval | proprietary institutional training data; generalization must be tested per anatomy/protocol |
| **Prima** | `core_fm` | report-supervised health-system-scale neuro-MRI foundation model | E1 [S15] | trained on >220k MRI studies; 1-year health-system test of 29,431 studies; evaluated across 52 radiologic diagnoses, reporting mean AUC 92.0%; supports differential diagnosis, worklist priority and referral outputs | brain/neuro-MRI scope; raw data are not public; model is investigational; setting-specific evidence must not be generalized blindly |
| **NeuroVFM** | `core_fm` | imaging-first health-system CT/MRI visual FM | E1 *Nature Medicine* [S58] | trained on 5.24M routine clinical CT/MRI volumes from 566,915 studies using a volumetric joint-embedding predictive architecture; evaluated across 156 diagnostic tasks; shares a CT/MRI neuroanatomic latent space and can support grounded report generation after lightweight visual instruction tuning | one health-system archive dominates pretraining; published comparisons support the health-system-learning paradigm, not universal superiority across all radiology |
| **Brainfound (Patterns, 2026)** | `core_fm` | multimodal brain CT/MRI + language + generation | E1 [S52] | trained on >3M brain CT slices and >7M brain MRI slices paired with reports; evaluated across seven task families spanning diagnosis, segmentation, enhancement/translation, reporting, zero-shot classification and dialogue; code/weights reported public | institutional pretraining data; many tasks are evaluated within a single model family; “approaches expert” claims remain protocol-specific |
| **MedGemma 1.5 4B** | `core_fm` | general multimodal generative model with explicit 3D CT/MRI support | E2 [S16] | supports 3D CT/MRI representations, WSI patch sets and longitudinal CXR; official card reports CT-RATE validation macro-F1 27.0 for 18 conditions | governed by HAI-DEF terms; Google explicitly requires downstream validation; public benchmark contamination is a stated concern; not evidence that MedGemma is the single best native 3D model |

### 4.3.1 Independent comparative CT evidence

A peer-reviewed study published **27 August 2026** systematically evaluated CT foundation models as frozen feature extractors for organ-level abnormality classification. It compared CT-FM, SPECTRE, TAP-CT, Merlin, UMedPT and Curia under fixed splits and common downstream protocols. [S81]

Key findings:

- 3D CT-native models tended to outperform 2D multimodal models in this setting;
- none of the evaluated aggregation strategies significantly outperformed mean pooling;
- focal abnormalities were harder than diffuse abnormalities for the stronger models;
- removing exact AMOS22 scan overlap did not significantly alter SPECTRE/UMedPT performance, but institution/scanner/population/protocol overlap could remain;
- frozen-probe comparison does **not** isolate architecture from pretraining data, objective or scale.

Therefore **SPECTRE, TAP-CT and CT-FM belong in the CT comparator/watchlist**, even if they are not promoted to dedicated mastery modules.

### Correct expert framing

The 2026 3D radiology frontier is **plural**, not “MedGemma = volumetric radiology.” A useful conceptual lineage is:

``` text
3D clinical dataset + report pairing
        -> contrastive volumetric representation
        -> reusable / adaptable 3D encoder
        -> generative volumetric reasoning
        -> grounded physical-space localization
        -> longitudinal change modelling
        -> tool-verified quantitative reasoning
```

CT-RATE/CT-CLIP, Merlin, Triad, Decipher-MR, Prima, NeuroVFM, Brainfound and MedGemma occupy different points in that stack. Prima and NeuroVFM are especially useful as a controlled conceptual pair: both exploit health-system neuroimaging, but they test different supervision/pretraining objectives. [S15,S58]

------------------------------------------------------------------------
## 4.4 Grounded, auditable and tool-augmented radiology

| System | System class | Role | Evidence | Verified contribution | Critical limitation |
| :------------ | --- | :----------------------------------------------------------------------------------------------------- | :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------- |
| **CLEAR** | `core_fm` | auditable CXR concept representation | E1 [S17] | trained on >0.87M image-report pairs from 239,391 patients; maps CXR to a semantically defined concept space so predictions can be decomposed into radiological observations | chest X-ray scope; auditability of concept contributions is not equivalent to causal explanation |
| **MAIRA-2** | `fm_derived` | grounded CXR report generation with priors/context | E3/E2 [S18] + E1+E5A [S102] | accepts current frontal/lateral images plus optional prior and reporting context; can generate narrative or grounded reports with 2D boxes; introduces RadFact evaluation | research model; adult English CXR scope; model card warns against other modalities and clinical production use; an independent 478-patient ED evaluation found higher RADPEER-3b disagreement than radiologist reports (24.5% vs 13.9%), lower standard clinical acceptability (65.6% vs 74.3%), and more hallucinations (17.4% vs 0.1%), showing that developer benchmark strength does not imply clinical report equivalence [S102] |
| **CARE-X** | `fm_derived` | structured CXR generation + auxiliary classification/grounding; separate tool-measurement experiment | E3 [S19] | author reports 94.0% ReXVQA accuracy; auxiliary heads for classification and grounding; separately, Qwen3-VL-4B-Instruct with deterministic measurement tools improves average F1 by 43.6 percentage points over perception-only baselines on five measurement-dependent conditions | preprint; **tool experiment is separate from the CARE-X core model and must not be conflated with it** |

### Independent counter-evidence matters

The independent 2026 emergency-department comparison provides a concrete stress test of this branch: MAIRA-2 performed materially worse than radiologist-written reports on clinically significant disagreement and acceptability and produced substantially more hallucinations in that cohort. [S102] This does **not** establish a universal ranking of report generators—the study is retrospective and single-centre—but it is exactly the kind of `E1+E5A` evidence that should sit beside developer-reported grounding/report-generation results.

### Frontier principle

Clinical radiology generation is moving from:

**“produce plausible prose”**

into:

**“produce structured, localizable, auditable, quantitatively verifiable claims.”**

The highest-value research is therefore increasingly about **grounding, calibration, measurement, consistency, priors, and tool use**, not merely BLEU/ROUGE-style report similarity.

------------------------------------------------------------------------
## 4.5 Ultrasound foundation models: acquisition interaction and examination-level reasoning

Ultrasound should not be treated as a single static-image branch. At least two orthogonal structures matter: **operator-controlled acquisition** and **multi-view/video examination structure**.

| System | System class | Role | Evidence | Verified contribution | Critical limitation |
|---|---|---|---|---|---|
| **EchoPrime** | `core_fm` | comprehensive multi-view echocardiography video-language FM | E1 *Nature* 2026 [S83] | trained on **12,124,168 videos from 275,442 studies / 108,913 patients** at Cedars-Sinai; integrates multiple study videos using view classification + anatomical attention; evaluated across an internal cohort plus four external health systems; code, weights and demo released | training is dominated by one health-system archive; external report-label extraction differs by site; broad retrospective validation is not prospective clinical utility |
| **EchoCLIP** | `core_fm` | earlier echocardiography vision-language FM / single-view lineage | E1 *Nature Medicine* 2024 [S84] | trained on **1,032,975 video-text pairs from 224,685 studies / 99,870 patients**; external validation for cardiac function and other tasks; establishes the earlier contrastive image-text lineage | largely frame/single-view representation relative to EchoPrime; training originates from the same Cedars-Sinai ecosystem as EchoPrime, so the two are not independent pretraining cohorts |
| **Echo-Vision-FM** | `core_fm` | self-supervised echocardiogram video representation framework | E1 *Nature Communications* 2026 [S87] | video-focused pretraining/fine-tuning framework for transferable echocardiography tasks; provides a complementary video-SSL lineage | separate training/evaluation design from EchoPrime; do not infer equivalence across tasks or cohorts from the shared “echo FM” label |
| **Sonomate** | `fm_derived` | fetal-ultrasound video-language assistant; workflow-specialist | E1 [S20] | trained from 525 real-world fetal ultrasound video-audio pairs; aligns video with transcribed sonographer speech and supports anatomy/VQA/acquisition interaction | fetal specialization and small paired corpus; operator/acquisition variability remain central |
| **Breast-ultrasound generative FM** | `core_fm` | breast-US generative/modeling system | E1 [S21] | demonstrates modality-specific generative foundation modelling beyond radiology/pathology | domain-specific; does not establish general ultrasound understanding |

### Two distinct ultrasound questions

**Acquisition interaction:** can the system guide or interpret what the operator is doing in real time? Sonomate is the anchor.

**Examination-level reasoning:** can the system integrate multiple videos/views from one study rather than classify isolated frames? EchoPrime is the strongest anchor in this dossier, with EchoCLIP as its earlier vision-language lineage. [S83,S84]

### Genealogy caveat

EchoCLIP and EchoPrime both draw from Cedars-Sinai echocardiography archives. EchoPrime is a major scale/architecture advance, but **it is not an independent institutional replication of the EchoCLIP pretraining paradigm**. External health-system evaluation strengthens transportability evidence for EchoPrime without making the training corpora independent. [S83,S84]

------------------------------------------------------------------------

## 4.6 Reconstruction, enhancement and foundational registration

These are not side topics. They determine whether the FM era reaches the **inverse problem and spatial correspondence layers** of medical imaging rather than beginning only after a reconstructed image already exists.

| System / study | System class | Problem | Evidence / source | Verified contribution | Red-team limitation |
| :----------------------------------------------------------------- | --- | :------------------------------------------------------ | ------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- |
| **DeepSparse** | `core_fm` | sparse-view CBCT reconstruction | E1 IEEE TMI [S41] | foundation model using multi-view 2D and multi-scale 3D features plus sparse/dense-view pretraining; peer-reviewed 2026 | “first” is author framing; generalization still depends on acquisition geometry and scanner domain |
| **Vision-language-guided fast MRI reconstruction** | `fm_enabled` | undersampled MRI reconstruction | E1 *Magnetic Resonance in Medicine* [S42] | uses FM semantic representations as a reconstruction prior; evaluated on knee and brain fastMRI settings; reports perceptual/reader-study gains while maintaining data fidelity | semantic prior does not replace k-space/data-consistency physics; hallucination risk must be explicitly tested |
| **Universal non-ideal CT enhancement FM** | `core_fm` | enhancement across degraded CT measurement conditions | E1 *Nature Communications* [S34] | demonstrates foundation-model framing upstream of interpretation for CT enhancement | enhancement is not synonymous with raw-projection reconstruction; must preserve quantitative/diagnostic signal |
| **HorusEye** | `core_fm` | self-supervised X-ray tomography restoration | E1 *Nature Computational Science* + E2 code [S105] | trained on >100M images and evaluated across multiple restoration tasks/modalities, including unseen imaging modalities and clinical low-contrast detectability/downstream-task analyses | restoration/post-processing is not raw-projection reconstruction; private-data components and task-specific fine-tuning constrain reproducibility; appearance gains must still be checked for quantitative/anatomical fidelity |
| **uniGradICON** | `core_fm` | general-purpose deformable registration | E1 MICCAI 2024 + open code [S44] | trained/evaluated across 12 public datasets with zero-shot and fine-tuning aims across anatomies/modalities | earlier anchor, not a 2026 release; serves as the registration-FM lineage rather than a current universal winner |
| **LUMIR follow-up: pathway to foundational registration models** | `companion` | large-scale zero-shot brain MRI registration | E1 *Medical Image Analysis*, 1 Sep 2026 [S43] | evaluates in-domain and extensive zero-shot shifts across disease, protocol and species; reports strong deep-registration robustness and plausible diffeomorphic fields | primarily neuro-MRI challenge setting; does not prove universal body/multimodal registration |
| **Triad** | `core_fm` | MRI representation transferred to registration | E1 [S40] | shows MRI-specific pretraining can improve downstream registration as well as segmentation/classification | performance depends strongly on upstream/downstream modality match |

### Reconstruction/registration expert rule

A foundation model must never be allowed to “improve appearance” while violating the forward model, anatomy, topology, or quantitative signal. Evaluation therefore needs **data consistency, topology/Jacobian checks, landmark/TRE metrics, uncertainty and downstream task preservation**, not SSIM/Dice alone.

------------------------------------------------------------------------
## 4.7 Ophthalmic and volumetric OCT foundation models

Ophthalmology is a mature FM subfield and was an important omission from the first rewrite. A 2026 review identified a substantial ophthalmic FM literature, and the current frontier increasingly treats OCT as a **volume**, not a representative 2D B-scan. [S55]

| System / study | System class | Role | Evidence / source | Verified contribution | Limitation |
| :------------------------------------------------- | --- | :------------------------------------------------------ | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| **OCTCube-M** | `core_fm` | 3D multimodal OCT framework | E1 *Nature Biomedical Engineering* [S45] | OCTCube pretrained on 26,605 3D OCT volumes (1.62M slices); OCTCube-IR adds paired IR; OCTCube-EF integrates >4M OCT slices and ~400k en-face images for geographic-atrophy analyses | institutional/proprietary components; some downstream trial data/models are not fully public |
| **Volumetric V-JEPA OCT benchmark** | `companion` | video-FM treatment of OCT volumes | E1 *npj Digital Medicine* [S46] | across five OCT datasets, full-volume V-JEPA averaged AUROC 0.94 versus 0.90 for the best image-based comparator in the study; benchmarks RETFound, VisionFM and DINOv2 | demonstrates value of volumetric context for evaluated AMD/GON tasks, not universal ophthalmic superiority |
| **RETFound Plus** | `core_fm` | longitudinal fundus FM for incidence/progression/risk | E1 *npj Digital Medicine* [S61] | temporally pretrained on 1,304,292 fundus photographs from 304,345 participants; externally evaluated across multi-regional, multi-ethnic cohorts for ocular and systemic longitudinal outcomes | fundus-photo modality; outcome prediction can encode population/health-system correlations and requires transportability/calibration checks |
| **FOCUS** | `fm_enabled` | foundation-model-driven end-to-end 3D OCT workflow | E1 *npj Digital Medicine* [S62] | quality assessment + abnormality detection + multi-disease classification with adaptive slice-to-volume aggregation; external validation across centres/devices | a workflow system using a fine-tuned VFM, not evidence for a single new universal foundation backbone |
| **RETFound / VisionFM / broader ophthalmic-FM lineage** | `companion` | retinal/ophthalmic representation baselines | E1/E5B lineage [S46,S55] | review-level lineage illustrating progression from retinal SSL to multimodal ophthalmic representation learning; named systems require their primary sources before model-specific claims | many OCT implementations historically reduce volumes to central slices; model versions/datasets differ |

------------------------------------------------------------------------
## 4.8 Task-specialized clinical foundation models: a necessary counterweight to “universal” models

The 2026 literature also shows the opposite trend from universal generalists: very large **domain- or workflow-specific** foundation models can deliver stronger clinical evidence because the task, population and workflow are precisely defined.

| System / study | System class | Scope | Evidence / source | Why it matters | Limitation |
| :--------------------------------------------------------------- | --- | :---------------------------------------------------- | :------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| **AbdomenNet** | `fm_derived` | acute-abdomen NCCT triage; workflow-specialist | E1 *Nature Communications* [S48] | pretrained on 103,989 NCCT exams; fine-tuned on 5,816 annotated cases; externally evaluated in 2,528 patients across three cohorts; reader study and retrospective workflow reconstruction included | one acute-abdomen workflow; retrospective workflow simulation is not prospective deployment |
| **OMAFound** | `core_fm` | breast + lung cancer screening from non-contrast CT | E1 *Nature Health* [S49] | developed/tested on 325,197 CT volumes from 151,386 patients across 10 datasets; illustrates organ- and patient-level multi-cancer screening | screening setting and cohort composition constrain generalization |
| **Explainable multiview mammography image-report pretraining** | `core_fm` | mammography representation/localization | E1 *Radiology: AI* [S50] | explicitly models multiview mammograms and reports and evaluates label-efficient malignancy/segmentation/localization across external centres | mammography-specific; does not imply cross-modality transfer |
| **GastroNet-5M** | `companion` | GI endoscopy pretraining resource/FM | E1 *Gastroenterology* [S51] | 4,820,653 endoscopic images from ~500,000 procedures across eight Dutch hospitals; establishes large-scale endoscopy-specific SSL | procedure/image correlations and frame-level evaluation need patient-level safeguards |
| **LCTfound** | `core_fm` | lung-CT multi-task vision FM | E1 *Nature Communications* [S64] | pretrained on LungCT-28M derived from 105,184 CT scans; evaluated across eight tasks spanning enhancement/reconstruction, segmentation, diagnosis, prognosis, response prediction and 3D navigation | lung-CT-specific; broad task coverage inside one ecosystem is not the same as cross-organ universality |
| **SKELEX** | `core_fm` | musculoskeletal radiograph FM | E1 *npj Digital Medicine* [S63] | self-supervised pretraining on >1.2M radiographs across 15 body regions; evaluated on 12 diagnostic tasks plus anomaly localization | strongest external-validity evidence is task-specific (notably bone-tumour applications); do not generalize to all MSK workflows |
| **NeuroSTORM** | `core_fm` | 4D fMRI spatiotemporal FM | E1 *Nature Biomedical Engineering* [S59] | pretrained on 28.65M fMRI frames from >50,000 participants across centres and ages 5–100; evaluated on five downstream task families including diagnosis and state/phenotype prediction | functional MRI has distinct preprocessing, temporal and nuisance-confound structure; not directly comparable with structural MRI FMs |

These systems are important because they falsify a simplistic scaling story: **clinical specialization and workflow fit can be more valuable than universality**.

------------------------------------------------------------------------
## 4.9 PET/CT and nuclear-medicine foundation-model frontier

Nuclear medicine should no longer be represented only through generic segmentation systems. The 2026 evidence is still less mature than CT/MRI/CXR, but it is scientifically valuable because PET/CT tests whether “foundation” representations survive a major shift in image statistics, acquisition physics and multimodal semantics.

| System / study | System class | Evidence profile | Contribution | Red-team interpretation |
| :---------------------------------------------- | --- | :--------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Open multicentre whole-body FDG PET/CT FM** | `core_fm` | E3 preprint [S76] | 4,997 harmonized PET/CT scans from four public datasets; joint anatomical-metabolic masked pretraining; downstream AutoPET label-efficiency and few-shot experiments | promising open PET/CT-specific representation learning, but not yet peer-reviewed proof of general-purpose nuclear-medicine transfer |
| **Dual-Stream DINOv3 PET/CT** | `fm_derived` | E1 peer-reviewed MIDL 2026 / PMLR evidence [S77] | explicitly tests natural-image versus initialization for PET/CT segmentation | important negative-control domain-aligned result: generic natural-image pretraining can be harmful under severe modality mismatch; “pretrained” is not automatically better |
| **autoPET** | `companion` | dataset/challenge anchor [D11A] | whole-body FDG PET/CT lesion segmentation | useful task benchmark, but its training cohort alone is not multicentre external-validation evidence |

### Nuclear-medicine expert rule

PET/CT models—and any future dedicated SPECT models—must be evaluated with modality-specific variables that generic image benchmarks often omit: tracer, uptake time, injected activity, attenuation correction, reconstruction method, SUV normalization, PET/CT registration, lesion avidity and scanner/site effects.

The central scientific question is not “does a large vision encoder transfer?” It is:

> **Which parts of a representation survive the transition from anatomical image texture to quantitative tracer-distribution imaging, and when does domain-aligned pretraining outperform generic visual scale?**

------------------------------------------------------------------------
## 4.10 Radiotherapy imaging and treatment-planning semantics

Radiotherapy introduces a different medical-imaging problem: AI outputs must interoperate with spatially linked DICOM objects and treatment-planning conventions rather than merely predict a label from an image.

A 2026 *JCO Clinical Cancer Informatics* study evaluated a retraining-free multistage foundation-model workflow for ROI classification and renaming using standard CT + DICOM RTSTRUCT data across **600 patients, three institutions and five disease sites**. The workflow combines language-model reasoning with CLIP-based laterality verification and maps local ROI names toward customizable/TG-263-style conventions. [S78]

This is **workflow evidence**, not proof of a universal radiotherapy foundation model. It matters because it exposes an expert-level deployment boundary:

``` text
image understanding
    !=
spatial-object correctness
    !=
semantic interoperability
    !=
treatment-planning safety
```

A radiotherapy FM landscape assessment should separately track:

- RTSTRUCT contour geometry and Frame-of-Reference consistency;
- ROI naming/ontology correctness;
- laterality;
- CT/RTSTRUCT/RTDOSE/RTPLAN linkage;
- contour quality and clinician edit burden;
- dose-volume consequences of geometric error;
- site/treatment-planning-system transfer;
- whether automation changes downstream planning decisions safely.

------------------------------------------------------------------------

## 4.11 Dermatology: multimodal specialty foundation models

**PanDerm** is a peer-reviewed *Nature Medicine* specialty FM that makes dermatology too mature to leave implicit inside general medical encoders. It was pretrained on **2,149,706 dermatology images from 11 data sources across four imaging modalities**: total-body-photography lesion tiles, dermatopathology, clinical photography and dermoscopy. It was evaluated across **28 benchmarks** spanning diagnosis, screening/risk stratification, segmentation, longitudinal lesion monitoring, metastasis/prognosis and related tasks, with three reader studies and released encoder code/weights. [S88]

| System | System class | Scope | Evidence | Why it matters | Red-team limitation |
|---|---|---|---|---|---|
| **PanDerm** | `core_fm` | `specialty_generalist` | E1 *Nature Medicine* [S88] | unifies several clinically distinct dermatologic imaging modalities and tasks; strong label-efficiency and external-dataset evidence | specialty-specific; public/in-house pretraining genealogy must be audited per benchmark; retrospective reader studies and benchmark success are not prospective clinical utility |

### Dermatology expert rule

A dermatology benchmark must preserve `patient -> visit -> lesion -> image/modality` hierarchy. Total-body-photography tiles from one patient are not independent lesions or patients. Longitudinal lesion identity, skin tone, geography, camera/device, clinical-vs-dermoscopic acquisition and pathology confirmation are first-class shift/reference-standard variables.

------------------------------------------------------------------------

## 4.12 Biomedical microscopy and computational-imaging foundation models

**UniFMIR** provides the earlier peer-reviewed foundation-model lineage for fluorescence-microscopy restoration: five restoration tasks across 14 datasets with released code/models. [S100]

**FluoResFM** extends that FM landscape into a larger 2026 cross-distribution fluorescence-microscopy restoration program. The peer-reviewed 2026 *Nature Communications* study trained on **4,303,086 paired low/high-quality patches** across denoising, deconvolution and 2x super-resolution, covering **>20 biological structures**, and evaluated **302 internal plus 51 unseen external datasets**. The framework can be adapted to 3D restoration, surface projection and isotropic reconstruction. [S89]

| System | System class | Scope | Evidence | Why it matters | Red-team limitation |
|---|---|---|---|---|---|
| **UniFMIR** | `core_fm` | modality-generalist fluorescence restoration | E1 *Nature Methods* [S100] | pretrained reusable restoration model demonstrated across five tasks and 14 datasets with public code/models | 2024 lineage; task/data distribution remains fluorescence-microscopy-specific |
| **FluoResFM** | `core_fm` | modality-generalist fluorescence restoration | E1 *Nature Communications* [S89] | demonstrates multi-task, cross-distribution restoration with external unseen datasets and low-shot adaptation | text/structure priors can themselves induce incorrect morphology: the paper shows mismatched structure prompts can distort restored structures; restoration quality metrics do not prove biological truth |

### Microscopy expert rule

For restoration FMs, the central safety question is not whether the output looks cleaner. It is whether the method preserves or invents biologically meaningful structure under a known forward/acquisition model. Evaluation should include physically meaningful reference images, structure-specific measurements, prompt/prior mismatch tests, microscope/acquisition shift and downstream-analysis preservation.

------------------------------------------------------------------------

## 4.13 Dental and oral imaging: panoramic foundation encoders and specialty VLMs

Dental imaging is now too mature to remain an implicit subcase of generic radiography. Three 2026 peer-reviewed systems establish distinct points in the specialty stack. [S92–S94]

| System | `system_class` | Scope | Evidence | Verified contribution | Red-team limitation |
|---|---|---|---|---|---|
| **PanoFM** | `core_fm` | `modality_generalist` panoramic radiography | E1 *Pattern Recognition* [S92] | reusable panoramic encoder trained/validated on **127,878 radiographs from six geographically diverse clinical centres in China**; two-stage self-supervised pretraining with downstream anatomical/LLM semantic integration | six-centre evidence is geographically diverse within one country; downstream LLM/context gains and encoder quality should be disentangled |
| **DentFound** | `fm_derived` | `workflow_specialist` panoramic diagnosis/reporting | E1 *Nature Biomedical Engineering* [S93] | >101,000-patient panoramic dataset, ages 2–98, 98 diseases + 11 post-treatment categories; multicentre diagnosis/report evaluation; 12-dentist/radiologist expert assessment; code released | specialized VLM rather than independently demonstrated reusable dental foundation encoder; an Author Correction was issued 8 Jul 2026 and the current corrected article is the authority |
| **DentVLM** | `fm_derived` | `specialty_generalist` dental imaging | E1 *Nature Communications* [S94] | Qwen2-VL-7B-derived dental VLM trained on **20,741 patients / 110,447 images / 2,458,084 bilingual VQA pairs**, spanning seven image modalities and 36 tasks; external tests plus 32-participant human–AI study | largely China-sourced training cohort; VQA/rationale construction includes model-assisted processing; human–AI results do not establish autonomous clinical safety |

### Dental expert rule

Preserve the hierarchy `patient -> encounter -> tooth/region -> modality/view -> image`. Tooth-level findings from one panoramic image are not independent patients; multiple intraoral views are correlated; panoramic and intraoral photographs encode different geometry; prior treatment creates strong label/context correlations. Report site, device, age/dentition stage, tooth numbering convention, repeated-exam status, pathology/reference-standard provenance and patient-level split rules.

### Correct interpretation

PanoFM provides the clearest **reusable panoramic representation** claim. DentFound and DentVLM provide strong **specialty VLM/workflow** evidence, but their clinical breadth should not be used to bypass the dossier’s Core-FM eligibility rule.

------------------------------------------------------------------------

# 5. Computational Pathology: From Tile Encoders to Clinical Slide Systems

## 5.1 Tile and representation layer

| System | System class | Evidence | Verified scale | Availability / note |
| :-------------------------------- | --- | :------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Virchow2** | `core_fm` | E3 arXiv/model release; also included in independent E1 benchmarks | 632M ViT; Virchow2G 1.9B; trained on 3.1M histopathology WSIs with mixed magnification | do not call Virchow2 itself a 2024 Nature Medicine paper; the peer-reviewed 2024 paper was **Virchow**, while Virchow2 is a later scaling work/preprint |
| **UNI / UNI2-h** | `core_fm` | UNI: E1 Nature Medicine 2024; UNI2-h: E2 official model card | UNI peer-reviewed on >100M images from >100k WSIs; UNI2-h card reports >200M tiles from >350k H&E/IHC slides and a custom ViT-H with **681M** parameters | UNI2-h uses CC-BY-NC-ND-4.0; not permissive open source |
| **H-Optimus-1** | `core_fm` | E3 scientific evidence currently represented here by an AACR 2026 conference abstract + E2 model card | 1.1B ViT; >1M WSIs from >800k patients, >50 organs, 3 scanner types, >4,000 clinical centres | released Apr 2025; CC-BY-NC-ND 4.0 non-commercial; abstract-level evidence is weaker than a full peer-reviewed article |
| **Prov-GigaPath tile encoder** | `core_fm` | E1 Nature 2024 | pretraining corpus contains 1,384,860,229 tiles from **171,189** H&E/IHC slides from >30k patients across 31 tissue types | open weights/code; important correction to older erroneous “31,216 slides” figure |
| **GigaPath-Flash tile encoder** | `core_fm` | E3 arXiv 20 Jul 2026 | 22M ViT-S distilled from the large GigaPath teacher | Apache-2.0 family per paper; emerging efficiency result |

------------------------------------------------------------------------
## 5.2 Whole-slide aggregation and multimodal systems

| System | System class | Evidence | Verified contribution | Red-team note |
| :------------------ | --- | :--------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Prov-GigaPath** | `core_fm` | E1 Nature 2024 | LongNet-based whole-slide modelling over tens of thousands of tiles; trained on 171,189 slides / 1.38B tiles | original work is strong, but “one slide encoder wins everything” is not supported by later benchmarks |
| **TITAN** | `core_fm` | E1 Nature Medicine 2025 | multimodal whole-slide FM trained on 335,645 WSIs; visual + language supervision including 423,122 synthetic captions | performance depends on downstream evaluation protocol; slide representation and tile backbone must be disentangled when comparing systems |
| **PRISM2** | `core_fm` | E1 Nature Medicine 31 Jul 2026 | 4.6B multimodal slide-level model trained on 2.3M WSIs and 14M QA pairs derived from ~700k pathology reports; prompt inference reaches/exceeds evaluated clinical-grade product balanced accuracy in specific prostate/breast/breast-LN cancer-detection settings | authors explicitly note PRISM2 and Virchow2 tile model were trained only on MSK-scanned slides; report completion remains challenging; robustness needs further study |
| **nnMIL** | `companion` | E1 Nature Biomedical Engineering 25 Aug 2026 | aggregation framework evaluated on 40k WSIs, 35 clinical tasks and 4 pathology FMs; random patch/feature sampling, large-batch optimization, uncertainty estimation; reports gains over existing MIL methods | very recent; an aggregation framework is not a replacement for studying tile encoder quality and pretraining bias |
| **CARE** | `core_fm` | E1 CVPR 2026 [S90] | adaptive morphologically meaningful WSI regions plus RNA/protein-guided cross-modal alignment; pretrained on **34,277 WSIs** and evaluated across **33 downstream benchmarks** spanning morphology, molecular prediction and survival | developer-reported benchmark advantage; adaptive-region and molecular-guidance gains need independent evaluation and contamination/provenance audit |
### 5.2.1 Adaptive region + molecular-guided WSI modelling

CARE adds a distinct architectural level between fixed tiles and slide aggregation: it learns **adaptive morphological regions** and then aligns them with RNA/protein information. This makes it particularly relevant to the dossier's aggregation and spatial-biology questions, but it should not be read as proof that molecular guidance universally improves pathology transfer. [S90]

### 5.2.2 The independent benchmark reality

The July 24, 2026 *Nature Communications* benchmark evaluated **32 foundation models across 41 tasks** under a standardized feature-extraction and linear-probing protocol. It found a close leading cluster rather than one universal winner; for example, Virchow2 and Prov-GigaPath had nearly identical mean AUROC in the reported aggregate analysis. The paper also explicitly warns that TCGA likely appears in the pretraining data of many evaluated systems.

**Correct bibliographic identifier:** *Nature Communications* **17, Article 9012 (2026)** — not Article 5821.

### 5.2.3 Independent multi-cohort pathology benchmark

A 2025 *Nature Biomedical Engineering* study independently benchmarked **19 histopathology foundation models across 13 cohorts, 6,818 patients and 9,528 slides** spanning lung, colorectal, gastric and breast cancers on weakly supervised biomarker, morphology and prognostic tasks. [S82]

This is important because independent frozen-feature benchmarking estimates **representation utility under a defined adaptation regime**, not the maximum performance achievable after fine-tuning.

### 5.2.4 September 2026 model-selection evidence

A *Scientific Reports* paper published **2 September 2026** benchmarked six pathology FMs (Lunit, Kaiko-Base, Phikon-v2, UNI2, Virchow2 and Kaiko-Midnight) across WSI classification, survival prediction and ROI classification, with explicit efficiency and training-fraction analyses. The rankings changed by task and data regime; larger models were not universally better, and throughput differed materially. [S47]

This independently reinforces the dossier rule: **model selection is a multi-objective decision over task, data regime, uncertainty, stability, throughput and deployment cost — not a parameter-count contest.**

### 5.2.5 The robustness reality

A separate June 2026 *Nature Communications* study, **“Towards robust foundation models for digital pathology,”** introduced the **PathoROB** benchmark and demonstrated that pathology FM embeddings can strongly encode non-biological technical features such as medical centre, staining and scanner information. In one analysis, medical centre could be predicted from embeddings with very high accuracy. Therefore:

> **Pathology FM evaluation must measure biological usefulness and technical-confound robustness separately.**

A leaderboard based only on task AUROC is incomplete.

## 5.3 Freshness delta — 10 September 2026: natural-language pathology segmentation and intraoperative translation

Two peer-reviewed papers published **10 September 2026** materially extend the pathology map beyond tile/slide representation and retrospective benchmark comparison.

| System | `system_class` | Evidence | Verified contribution | Red-team interpretation |
|---|---|---|---|---|
| **PathSegmentor** | `core_fm` | E1 *Nature Computational Science* [S103] | natural-language-guided pathology segmentation across a hierarchical PathSeg resource built from 21 public datasets and 275,200 image–mask–label triples; the paper reports 160 pathological categories, comparisons across 16 internal datasets, and generalization to external public/clinical cohorts | strong new promptable-pathology branch, but aggregate public-dataset genealogy, pretraining overlap, prompt-selection protocol and clinical-cohort independence still need explicit audit |
| **CRISP** | `core_fm` | E1 *Nature Medicine* [S104] | clinically oriented frozen-section FM developed on >100,000 frozen sections from ten centres; evaluated on >15,000 intraoperative slides across nearly 100 retrospective tasks, with reported generalization across six institutions/14 tumour types/24 anatomical sites and a prospective cohort of >3,000 patients; human–AI collaboration reduced reported diagnostic workload by 35% | unusually strong translational evidence for a pathology FM, but it remains a developer-led, workflow-specific frozen-section system; prospective evidence does not automatically establish autonomous safety, global transportability or performance outside the evaluated surgical workflow |

### Why these change the landscape

**PathSegmentor** shows that promptable/natural-language segmentation is no longer only a radiology/3D-SAM story; pathology now has a peer-reviewed semantic-prompt segmentation branch. **CRISP** is more consequential for clinical translation: it is a concrete counterexample to the claim that essentially all pathology FMs remain confined to retrospective offline benchmarking. It should therefore be treated as an important translational anchor while keeping the exact intended use and prospective-study design explicit. [S103,S104]

------------------------------------------------------------------------

# 6. Virtual Spatial Biology and Generative Medical Imaging

## 6.1 GigaTIME

**Evidence:** E1 [S32], *Cell*, volume 189 issue 2, 22 Jan 2026; DOI `10.1016/j.cell.2025.11.016`.

Verified facts:

- cross-modal H&E -> virtual multiplex immunofluorescence framework;
- paired training data of ~40 million cells across 21 protein channels;
- applied to 14,256 patients from 51 hospitals and >1,000 clinics;
- generated 299,376 virtual mIF slides;
- spans 24 cancer types and 306 subtypes;
- external validation analyses included TCGA.

### Red-team interpretation

“Virtual staining” must not be treated as measured molecular truth. A model predicts molecular/spatial signals conditioned on morphology and its training distribution. Valid research therefore needs:

- paired physical mIF ground truth on external sites;
- channel-wise calibration and uncertainty;
- failure analysis by cancer type/stain/scanner;
- downstream biological reproducibility;
- prospective test of whether virtual markers preserve clinically relevant decisions.

### 6.1.1 Complementary frontier: VirTues models measured spatial proteomics directly

**VirTues (Virtual Tissues)** is a different scientific object from GigaTIME. Rather than inferring multiplex markers from H&E, VirTues is a 2026 *Nature* foundation model trained directly on multiplex spatial-proteomics imaging and learns marker-aware representations across proteins, cells, niches and tissues. The reported system supports marker reconstruction, cell segmentation/typing, niche annotation, spatial biomarker discovery and patient stratification, including zero-shot transfer across heterogeneous marker panels. [S60]

This distinction is fundamental:

- **GigaTIME:** morphology (H&E) -> inferred molecular/spatial marker signal;
- **VirTues:** measured multiplex marker images -> reusable multi-scale spatial-proteomics representation.

They should not be compared as if they solve the same prediction problem. Together they show that the spatial-biology frontier includes both **cross-modal inference** and **direct foundation modelling of measured multiplex tissue data**.

## 6.2 GigaTIME-Flash / GigaPath-Flash

**Evidence:** E3 [S33] arXiv, submitted **20 Jul 2026**, not 31 Aug 2026.

Author-reported facts:

- GigaPath-Flash: 22M ViT-S tile encoder + 21M LongNet slide encoder;
- retains 97% of GigaPath average slide-level performance with 50x less compute under the paper’s evaluated protocol;
- GigaTIME-Flash: author reports 6x faster inference and 8x less GPU memory than original GigaTIME, with improved prediction quality.

These are promising engineering claims but remain **preprint-level** until stronger independent/peer-reviewed validation.

## 6.3 MAISI-v2 / NV-Generate-CTMR

Official NVIDIA/MONAI engineering sources describe MAISI-v2 as a rectified-flow 3D synthesis family and NV-Generate-CTMR as an end-to-end CT/MRI generation framework. Current repository metadata distinguishes commercial-friendly and non-commercial weights by checkpoint. Vendor-reported speedups and quality metrics must be treated as configuration-specific and independently reproduced before being generalized.

## 6.4 Reconstruction/enhancement branch

The inclusion of a 2026 *Nature Communications* **foundation model for universal enhancement of non-ideal measurement CT** [S34] confirms that foundation-model thinking now extends toward the measurement/reconstruction side of imaging.

This dossier deliberately does not pretend that one paper covers reconstruction expertise. It marks the branch and routes the learner to a separate acquisition/reconstruction curriculum.

## 6.5 XGeM: multimodal generative synthesis — broad architecture, narrower evidence

**XGeM** is a peer-reviewed 2026 *Computerized Medical Imaging and Graphics* multimodal generative FM with **6.77B parameters** and an any-to-any multi-prompt generation design. The published validation includes comparison against five competitors on **MIMIC-CXR**, an expert-radiologist visual Turing test, and synthetic-data experiments for anonymization, class imbalance and scarcity. [S91]

**System class:** `fm_derived` under this dossier’s conservative eligibility rule; **evidence scope:** strongly CXR/report-centric in the published study. The architecture is broad, but current published transfer evidence is not yet broad enough for automatic `core_fm` promotion.

Do **not** convert “any-to-any architecture” into “clinically validated across arbitrary medical modalities.” XGeM belongs in the frontier synthesis map because its architecture is broad, but the evidence base currently supports a much narrower set of claims.

------------------------------------------------------------------------

# 7. Clinical Infrastructure and Deployment

## 7.1 MONAI

**MONAI 1.6.0** is verified on PyPI as released on **22 Jun 2026** under Apache-2.0. MONAI remains a major open PyTorch-based medical-imaging framework for transforms, networks, inferers, data handling and reproducible pipelines.

Do not call MONAI itself “a foundation model.” It is infrastructure.

## 7.2 MONAI Deploy

Official MONAI Deploy documentation defines a deployment stack around:

- MONAI Application Packages (MAPs);
- App SDK;
- DICOM-oriented input/output;
- DICOM/FHIR interoperability through the Informatics Gateway;
- workflow orchestration and packaging.

### Critical distinction

**Packaging a model into a DICOM/FHIR-capable container is not regulatory clearance.** Clinical-device quality management, cybersecurity, validation, monitoring, intended-use definition and jurisdiction-specific regulatory requirements remain separate obligations.

## 7.3 Geometry is a first-class competency

Any expert working with 3D medical imaging should be able to reason explicitly about:

- DICOM Study / Series / SOP Instance structure;
- ImagePositionPatient and ImageOrientationPatient;
- voxel-to-patient transforms;
- LPS versus RAS conventions;
- anisotropic spacing;
- resampling interpolation by data type;
- orientation changes;
- native-space reconstruction of predictions;
- sequence/phase/series selection;
- patient-level and study-level identity and chronology.

A “3D model” benchmark that silently loses physical geometry is methodologically suspect.

------------------------------------------------------------------------

# 8. Benchmarking Doctrine: How to Evaluate Frontier Models Without Fooling Yourself

The 4 September 2026 *Nature Methods* perspective **Benchmarking biomedical foundation models** is a direct methodological anchor for this section: it emphasizes reproducibility, generalization, replicability, the difficulty of defining falsifiable FM evaluations, and the need for community-level benchmark design [S113]. This dossier adopts the same core stance: benchmark utility must be interpreted through provenance, independence, scope and failure evidence rather than leaderboard rank alone.

## 8.1 Before running a model

For every experiment, write down:

1.  **Clinical target:** what decision or intermediate task is being solved?
2.  **Unit of analysis:** slice, series, study, lesion, organ, WSI, patient, encounter?
3.  **Input semantics:** which series, phase, sequence, prior, report, prompt, metadata?
4.  **Supervision regime:** zero-shot, prompt-only, linear probe, frozen encoder + head, LoRA, full fine-tune, task-specific training?
5.  **Comparator fairness:** is the baseline receiving the same labels/prompts/context?
6.  **Pretraining overlap:** could the test cohort or close derivatives have appeared in pretraining?
7.  **Externality:** new hospital? new scanner? new protocol? new demographic distribution?
8.  **Metric:** does it measure the clinically important failure mode?
9.  **Uncertainty/calibration:** can confidence be trusted?
10. **Compute:** accuracy per annotation hour, training GPU-hour, inference latency, memory and energy — not parameter count alone.

## 8.2 Mandatory split discipline

- split by **patient**, never by image/slice when the task is patient/study-level;
- preserve temporal leakage barriers for longitudinal data;
- explicitly audit duplicated or near-duplicated scans;
- for pathology, audit TCGA overlap and specimen-level duplication;
- for report-generated labels, separate label-generation error from model error;
- when using synthetic QA, test on independently authored clinical questions.

## 8.3 Domain-shift matrix

A frontier benchmark should intentionally stress:

| Shift                 | Example                                           |
|:----------------------|:--------------------------------------------------|
| institution           | academic tertiary centre -> community hospital   |
| scanner/vendor        | Siemens -> GE -> Philips; Aperio -> Hamamatsu  |
| acquisition           | low-dose, motion, different reconstruction kernel |
| protocol              | contrast phase, MRI sequence, slice thickness     |
| population            | age, sex, ethnicity, disease prevalence           |
| annotation            | different radiologist/pathologist conventions     |
| prompt                | box jitter, point error, ambiguous language       |
| pathology preparation | stain intensity, tissue thickness, lab process    |
| temporal              | later-year deployment data                        |

## 8.4 Metrics beyond headline accuracy

### Segmentation

- Dice / DSC;
- surface Dice;
- Hausdorff distance (HD95);
- lesion-wise sensitivity / precision;
- small-object recall;
- calibration where probabilistic outputs are used;
- annotation time saved.

### Classification / detection

- AUROC **and** AUPRC under realistic prevalence;
- sensitivity at fixed specificity and vice versa;
- calibration error / Brier score;
- decision-curve or net-benefit analysis when justified.

### VLM/reporting

- clinically factual correctness;
- omission rate;
- contradiction rate;
- localization correctness;
- measurement error;
- prior-comparison correctness;
- hallucination rate;
- abstention/selective prediction behaviour.

### Pathology

- external AUROC/AUPRC;
- scanner/lab robustness;
- pretraining-overlap sensitivity;
- uncertainty;
- slide aggregation sensitivity;
- molecular/biomarker performance with appropriate confidence intervals.

------------------------------------------------------------------------

## 8.5 Privacy: separate re-identification from membership inference and other threats

Privacy is not one metric. Use an explicit threat taxonomy:

```text
re-identification / linkage
membership inference
model inversion / reconstruction
attribute inference
memorization / training-data extraction
federated-gradient / update leakage
```

### Re-identification / linkage

Medical-image representations can preserve patient-specific information. A 2025 *npj Digital Medicine* study reported non-trivial patient re-identification from frozen retinal/CXR foundation-model features and much higher rates after supervised re-identification training. [S65]

However, a 2026 *Matters Arising* replication argued that much of the apparent signal can be explained by near-identical longitudinal images and showed that raw pixels or a small untrained CNN could reproduce substantial re-identification performance. [S66]

**Audit rule:** do not write “foundation models uniquely cause re-identification.” Audit near-duplicates, compare raw-pixel/weak baselines, state attacker knowledge and separate individual identification from demographic inference.

### Patient-level membership inference

A 2026 *Nature* study performs a patient-level privacy audit across seven real-world medical datasets and shows that **aggregate MIA metrics can look close to random while individual patients experience near-perfect attack success**. Vulnerability increased with model capacity, and underrepresented groups were disproportionately represented among highly vulnerable records. Differential privacy reduced measured risk in the evaluated settings. [S95]

This changes the minimum privacy audit. Report:

- aggregate **and patient-level** membership-inference risk;
- attacker access assumptions (black-box/white-box/update access);
- multiple records per patient;
- subgroup/tail risk, not only mean AUC;
- model capacity;
- privacy–utility trade-off under a defined mitigation such as patient-level DP where applicable.

**Boundary:** the *Nature* paper evaluates membership inference, not every privacy attack. Do not generalize its disparate-risk result automatically to inversion, attribute inference or extraction. [S95]

Privacy evaluation is part of model release and dataset governance, not an optional deployment appendix.

## 8.6 Statistical inference doctrine

A provenance-clean benchmark can still produce weak science if the inferential design is poor. Every comparative study should predefine the statistical unit, endpoint and uncertainty analysis before inspecting final test results.

### Minimum statistical requirements

1.  **Resample at the independent unit.** If the clinical unit is the patient, do not bootstrap slices, tiles or lesions as if they were independent patients. Use patient-level or appropriately clustered resampling.
2.  **Prefer paired comparisons.** When two models are evaluated on the same cases, report uncertainty on the **difference** in performance, not only two separate confidence intervals.
3.  **Predefine a primary endpoint.** Large benchmark matrices create multiplicity. Distinguish confirmatory primary analyses from exploratory secondary analyses and control multiplicity where inferential claims are made.
4.  **Model site heterogeneity.** Multicentre results should report per-site performance and, where appropriate, hierarchical/random-effects estimates rather than only a pooled average.
5.  **Report prevalence-sensitive metrics.** AUROC alone is inadequate for rare findings; include AUPRC, sensitivity/specificity operating points, calibration and decision-relevant measures.
6.  **Power the claim being made.** Reader studies, non-inferiority/equivalence claims and prospective evaluations require explicit sample-size/power justification.
7.  **Do not use “no significant difference” as proof of equivalence.** Equivalence/non-inferiority requires a prespecified clinically meaningful margin and suitable analysis.
8.  **Quantify calibration uncertainty.** Calibration curves and summary scores should include uncertainty and be reassessed after domain shift.
9.  **Reader studies require reader/case structure.** Account for repeated measurements by readers and cases; report inter-reader variation and the human–AI interaction design.
10. **Do not hide subgroup instability.** Report subgroup sample sizes and uncertainty; avoid overinterpreting tiny strata.

### Default reporting package

For a serious model comparison, prefer:

``` text
point estimate
+ 95% confidence interval
+ paired difference vs prespecified reference
+ per-site / per-domain results
+ calibration
+ clinically relevant operating point
+ failure counts
+ multiplicity statement
+ sample-size / power rationale when confirmatory
```

------------------------------------------------------------------------

## 8.7 Reference-standard taxonomy

“Ground truth” should be reserved for targets that are genuinely deterministic. Most medical-imaging labels are **reference standards with uncertainty**.

Record at least:

| Field             | Examples                                                                                                                   |
|:------------------|:---------------------------------------------------------------------------------------------------------------------------|
| source            | report-derived / single reader / multiple readers / pathology / surgery / follow-up / laboratory test / device measurement |
| expertise         | trainee / generalist / subspecialist / board-certified specialist                                                          |
| process           | independent reads / consensus / adjudication / majority vote                                                               |
| blinding          | blinded to model / outcome / other readers / clinical data?                                                                |
| temporal relation | same encounter / follow-up / retrospective adjudication                                                                    |
| uncertainty       | disagreement rate / equivocal class / confidence / excluded cases                                                          |
| independence      | was the reference standard derived from information also supplied to the model?                                            |

A model cannot be more precisely evaluated than its reference standard permits. Label noise, incorporation bias and adjudication leakage should be treated as properties of the experiment, not as footnotes. CLAIM 2024 and STARD-AI are useful reporting anchors for this discipline. [S70,S71]

------------------------------------------------------------------------

## 8.8 Prompt and instruction robustness

Prompted systems introduce another researcher degree of freedom.

For text-, point-, box- or instruction-conditioned models:

- publish the exact prompt templates;
- report how prompts were selected;
- prohibit test-set prompt optimization;
- test multiple semantically equivalent text templates;
- report prompt ensembles separately from single-prompt results;
- perturb box/point prompts according to realistic human error;
- distinguish oracle prompts from automatically generated prompts;
- for VLMs, include text-only and image-shuffled controls where appropriate.

A “zero-shot” score tuned through repeated benchmark prompt engineering is not a clean zero-shot estimate.

------------------------------------------------------------------------

## 8.9 Reporting standards versus evidentiary strength

Use reporting/evaluation guidance according to study type:

- **CLAIM 2024** — medical-imaging AI manuscript/reporting structure and terminology; [S70]
- **STARD-AI** — diagnostic-accuracy studies using AI; [S71]
- **TRIPOD+AI** — development/evaluation of clinical prediction models; [S74]
- **DECIDE-AI** — early-stage live clinical evaluation and human factors; [S72]
- **SPIRIT-AI** — protocols for prospective/interventional AI trials; [S80]
- **CONSORT-AI** — randomized clinical trials involving AI interventions; [S75]
- **PROBAST+AI** — risk-of-bias/applicability assessment for prediction-model development/evaluation; [S79]
- **FUTURE-AI** — lifecycle-oriented trustworthy/deployable healthcare-AI principles. [S73]

These frameworks improve transparency and design discipline. **They are not evidence grades.** A perfectly reported retrospective internal study remains retrospective internal evidence.

### Risk-of-bias / applicability layer

Reporting completeness is not risk-of-bias assessment. For diagnostic/prognostic prediction-model claims, use **PROBAST+AI** as a separate layer. It distinguishes model development from model evaluation and assesses participants/data sources, predictors, outcome/reference standard, analysis and applicability. [S79]

A study can therefore be:

``` text
well reported
+ externally evaluated
+ peer reviewed
+ still high risk of bias
```

The evidence profile should record **reporting completeness**, **risk of bias/applicability**, **artifacts available**, and **replication status** separately.

For prospective AI intervention trials, use **SPIRIT-AI** at protocol stage and **CONSORT-AI** for trial reporting. [S80,S75]

### Study-design-specific bias map

No single risk-of-bias instrument covers this entire dossier. Use domain-appropriate bias questions:

| Study type | Bias domains that must be explicit |
|---|---|
| diagnosis / prediction | participant selection, predictors, outcome/reference standard, analysis, applicability; PROBAST+AI where appropriate |
| diagnostic-accuracy evaluation | sampling, index test, reference standard, flow/timing; STARD-AI for reporting plus an appropriate diagnostic-accuracy RoB assessment |
| human–AI / prospective intervention | allocation, protocol deviations, missing outcomes, human factors, learning effects; SPIRIT-AI/CONSORT-AI/DECIDE-AI as appropriate |
| segmentation / localization / registration | annotation/reference-standard quality, patient/source splits, rater dependence, geometry/protocol leakage, post-processing and task-selection bias |
| reconstruction / restoration | acquisition/forward-model validity, simulated-versus-real degradation, reference-image validity, hallucinated structure, metric perceptual bias |
| VLM / generative reporting | prompt selection, benchmark contamination, reference-answer/evaluator bias, text-only shortcuts, hallucination adjudication, model-as-judge dependence |

The purpose is not to invent a universal score. It is to prevent a low-risk judgment in one methodological framework from being generalized to tasks that framework was not designed to assess.

------------------------------------------------------------------------

## 8.10 Closed-form benchmark success is not clinical reasoning

The 3 September 2026 B&J benchmark is a useful warning against benchmark theatre: 14 VLMs and six LLMs were tested on 1,245 questions derived from real orthopaedic/sports-medicine cases across seven reasoning tasks. Leading systems exceeded 90% on structured multiple-choice questions but open-ended multimodal accuracy scarcely reached 60%; image interpretation remained weak and text-driven hallucinations were common. Medical-specific models did not show a consistent advantage over general-purpose counterparts. [S67]

**Benchmark rule:** every generative medical-imaging system should be evaluated separately on:

- closed-form recognition/QA;
- open-ended image-dependent interpretation;
- evidence localization/grounding;
- management or next-step reasoning only when the intended use actually requires it;
- rationale/evidence consistency;
- text-only ablation to detect whether the image is being ignored.

A model that answers the question correctly for the wrong reason has not passed an imaging benchmark.

## 8.11 Federated foundation models: emerging paradigm with bounded empirical systems evidence

A 1 September 2026 *npj Digital Medicine* perspective formalizes multimodal, multi-task federated foundation models for distributed biomedical sensing/imaging as an emerging research paradigm. [S69]

An independent 2026 empirical study adds a narrower systems result: using pretrained FM image embeddings reduced the **trainable federated model size by >90%** in the evaluated medical-imaging setup, thereby reducing estimated communication overhead; the study also integrated secure multiparty computation and explicitly discussed limits of FM embeddings under cohort bias. [S101]

This is **not evidence that federated FMs solve privacy or cross-institutional learning**. Precomputed embeddings can themselves contain sensitive or site-specific information, and federated training does not protect against post-training membership inference by itself. [S95,S101]

Any federated-FM study should report:

- client/site heterogeneity and participation assumptions;
- communication and compute cost;
- privacy threat model rather than a generic “data stay local” claim;
- central-training and local-training references where legally/ethically possible;
- robustness to non-IID disease prevalence, scanners, and protocols;
- failure under client dropout, poisoning, or distribution drift where relevant.

## 8.12 Medical VLM security: prompt injection and transferable adversarial attacks

Multimodal clinical systems create an attack surface that ordinary retrospective accuracy benchmarks do not measure.

Three peer-reviewed evidence anchors establish that this is not hypothetical:

- a 2025 *Nature Communications* oncology study tested **594 prompt-injection attacks** across four VLMs and showed that sub-visual prompts embedded in medical imagery could redirect outputs toward harmful content; [S96]
- a 2026 *npj Digital Surgery* study tested textual and temporally varying visual prompt injections across **100 surgical video clips / 8 decision-support tasks** and found significant performance degradation across all tested VLMs, with prolonged visual attacks more disruptive than single-frame attacks; [S97]
- ACL 2026 **MedFocusLeak** demonstrates transferable black-box attacks across six medical imaging modalities using imperceptible perturbations concentrated in non-diagnostic background regions. [S98]

### Security threat model

At minimum, distinguish:

```text
visible / sub-visual prompt injection
OCR / text-overlay injection
multiframe / temporal injection
imperceptible adversarial perturbation
background-region attention diversion
tool / API / retrieved-context injection
training-data / provenance poisoning
model extraction / stealing
```

### Security benchmark rule

Report attack success **and** benign-task preservation, image perceptibility/fidelity, transferability, attacker knowledge, query budget, modality, clinically meaningful error severity and abstention/fail-safe behaviour. A defense that blocks attacks by rejecting ordinary clinical inputs is not clinically acceptable robustness.

**Do not conflate safety alignment with diagnostic robustness:** refusing harmful instructions, resisting adversarial visual manipulation and maintaining correct medical interpretation are distinct objectives.

------------------------------------------------------------------------

# 9. Open Frontiers and Unresolved Areas

This section does **not** propose projects. It records where the 2026 landscape is technically active but scientifically or clinically unsettled.

## 9.1 Promptable/generalist 3D segmentation

**Current state:** promptable and generalist 3D segmentation is now a substantial branch rather than a medical adaptation footnote. VISTA3D, MedSAM2, BiomedParse v2, SAT3D, NV-Segment-CTMR and Medical SAM3 represent different automatic, interactive, text-guided and uncertainty-aware directions. [S3–S8,S39]

**What is established:** broad anatomical/tumour coverage and interactive flexibility are feasible; some systems report large annotation-efficiency gains.

**What remains unresolved:** how consistently those gains survive strong task-specific supervised comparators, realistic prompt error, external-site shift, UI/latency differences and expert correction burden.

## 9.2 Native 3D CT/MRI representation and reasoning

**Current state:** volumetric radiology is plural rather than centred on one model family. CT-CLIP/CT-CHAT, Merlin, Triad, Decipher-MR, Prima, NeuroVFM, Brainfound and MedGemma occupy different points across contrastive representation, health-system learning, generative reasoning and multimodal transfer. [S12–S16,S40,S52,S58]

**What is established:** native volumetric representation is viable and often strong; independent CT and OCT evidence supports the importance of volumetric information in selected settings. [S46,S81]

**What remains unresolved:** the causal contribution of dimensionality itself versus architecture, corpus, objective, information exposure and compute; the relative value of imaging-first versus report-supervised health-system learning; and how general these representations remain across protocols/anatomies.

## 9.3 Physical-space grounding, longitudinal correspondence and quantitative change

**Current state:** radiology systems are moving from free-text description toward localization, boxes/masks, physical coordinates, priors and measurements. CT-RATE derivatives, AbdomenAtlas 3.0, MAIRA-2 and related grounded systems provide increasingly structured supervision. [D5,D20–D22,S18]

**What is established:** static spatial grounding is technically feasible in several modalities.

**What remains unresolved:** robust patient-coordinate grounding under DICOM geometry, lesion identity across serial studies, new/disappeared/split/merged lesions, registration dependence, and trustworthy quantitative response/change estimation.

## 9.4 Tool-augmented and quantitatively verifiable radiology

**Current state:** a new systems direction delegates measurements or deterministic operations to external tools rather than forcing a VLM to infer every quantity from pixels. CARE-X provides early evidence for measurement-dependent gains. [S19]

**What is established:** tool augmentation can improve selected measurement-oriented tasks in developer evidence.

**What remains unresolved:** independent replication, validated-tool requirements, propagation of localization errors, unit/spacing failures, report–tool contradiction, fail-closed behavior and whether rare severe failures outweigh mean gains.

## 9.5 Technical shortcuts, site identity and domain entanglement

**Current state:** pathology and mammography independently show that FM embeddings can make dataset/site/scanner/stain information highly accessible. [S29,S99]

**What is established:** technical/domain identity can be strongly encoded.

**What remains unresolved:** when downstream predictions causally rely on those signals, how much of site predictability reflects real population/prevalence differences, and which mitigation strategies improve robustness without destroying clinically useful variation.

## 9.6 Model selection, aggregation and efficiency

**Current state:** pathology now has unusually rich independent evidence that parameter count, benchmark average and architectural complexity do not produce one stable universal ranking. CT evidence also questions whether sophisticated aggregation reliably beats mean pooling. [S28,S30,S47,S81,S82]

**What is established:** model selection is multi-objective and task/data-regime dependent.

**What remains unresolved:** clinically useful model-selection rules that jointly account for transfer, robustness, calibration, throughput, energy/memory, licence and deployment cost.

## 9.7 Spatial biology: inferred versus measured molecular context

**Current state:** GigaTIME and VirTues represent two distinct frontiers: H&E-to-virtual-multiplex inference versus foundation modelling directly on measured multiplex spatial-proteomics images. [S32,S60]

**What is established:** both cross-modal virtual inference and multi-scale representation of measured spatial biology are technically credible at large scale.

**What remains unresolved:** external marker fidelity, calibration, panel/site/platform shift, biological reproducibility, patient-level inferential validity and the observability limits of morphology-derived molecular prediction.

## 9.8 Ultrasound: acquisition interaction and study-level reasoning

**Current state:** ultrasound contains at least two different FM problems. Sonomate addresses operator/acquisition interaction, whereas EchoCLIP, EchoPrime and Echo-Vision-FM address echocardiography representation from frames/videos toward complete multi-view studies. [S20,S83,S84,S87]

**What is established:** language/video interaction and study-level multi-view representation are feasible; EchoPrime has substantial retrospective external evaluation.

**What remains unresolved:** operator-generalization, prospective acquisition guidance, machine/vendor shift, missing-view robustness, and independent causal replication of study-level aggregation benefits.

## 9.9 Clinical translation and lifecycle maturity

**Current state:** most FM papers remain retrospective/offline, but evidence is becoming more heterogeneous. CRISP adds unusually strong prospective intraoperative pathology evidence; independent report-generation studies expose clinically meaningful failure differences among systems. [S68,S102,S104]

**What is established:** retrospective benchmark performance is not a surrogate for clinical readiness; technical, operational, human–AI, prospective and real-world stages are distinct. [S2,S67,S68]

**What remains unresolved:** which branches can demonstrate prospective utility, workflow benefit, stable calibration, safety monitoring and post-update performance under a defined intended use.

## 9.10 Longitudinal and spatiotemporal foundation modelling

**Current state:** RETFound Plus and NeuroSTORM show that time can be represented explicitly in retinal longitudinal imaging and 4D fMRI rather than discarded through cross-sectional abstraction. [S59,S61]

**What is established:** temporal/spatiotemporal pretraining can add predictive information in evaluated settings.

**What remains unresolved:** separation of biological dynamics from identity, treatment, follow-up behavior, utilization, censoring, site/protocol and near-duplicate signal.

## 9.11 PET/CT and nuclear-medicine representation learning

**Current state:** dedicated PET/CT FM work is credible but substantially less mature than CT/MRI/CXR. Whole-body FDG PET/CT pretraining remains partly preprint-level, while MIDL 2026 provides peer-reviewed evidence that natural-image initialization may be poorly matched to PET/CT segmentation. [S76,S77]

**What is established:** domain mismatch matters and PET/CT warrants modality-specific treatment.

**What remains unresolved:** cross-tracer transfer, SUV/quantitative semantics, tracer/reconstruction/site shift, PET–CT fusion strategy and whether general-purpose nuclear-medicine representations emerge beyond FDG tumour segmentation. **SPECT remains under-audited.**

## 9.12 Radiotherapy semantic and spatial interoperability

**Current state:** foundation-model-era workflows now interact with RTSTRUCT semantics, laterality and nomenclature rather than only image labels. The 2026 ROI renaming/classification work demonstrates cross-institution workflow relevance. [S78]

**What is established:** semantic normalization can be assisted without task-specific retraining in evaluated settings.

**What remains unresolved:** contour/name mismatch, Frame-of-Reference integrity, dose-volume consequences, treatment-planning-system transfer, clinician correction burden and rare high-severity spatial errors.

## 9.13 Patient privacy

**Current state:** privacy evidence has moved beyond generic statements about de-identification. Re-identification/linkage and patient-level membership inference are distinct risks, and the 2026 literature shows why aggregate attack metrics can conceal highly vulnerable individuals. [S65,S66,S95]

**What is established:** representations/models can preserve patient-specific information; near-duplicates can inflate identity claims; patient-level membership risk can be highly heterogeneous.

**What remains unresolved:** modality/model-family differences, repeated-record effects, subgroup/tail risk, realistic attacker assumptions and privacy–utility trade-offs under mitigation.

## 9.14 Multimodal security

**Current state:** medical VLM attack evidence now includes visible/sub-visual prompt injection, temporal surgical-video injection and transferable imperceptible attacks across imaging modalities. [S96–S98]

**What is established:** clean-input diagnostic performance and conventional language safety do not establish multimodal attack robustness.

**What remains unresolved:** clinically acceptable defenses that preserve benign performance, robustness across preprocessing/compression/workflow transformations, tool/retrieval-context injection, provenance poisoning and fail-safe behavior.

## 9.15 Label-efficient and data-centric learning

The annotation bottleneck remains structurally important despite FM pretraining. The field contains mature weak/semi-supervised and active-learning traditions plus newer heterogeneous-label and FM-assisted strategies [S108]. The unresolved landscape question is not whether labels can be reduced in principle, but **which supervision strategy is most robust under realistic expert cost, partial labels, noisy provenance and domain shift**.

## 9.16 Robust adaptation, OOD and continual model evolution

Cross-site generalization remains one of the central failure modes of medical-imaging AI. Domain-generalization/adaptation methods and continual-learning strategies form active ecosystems that may complement or outperform static large-scale pretraining in deployment settings [S109,S110]. The unresolved field issue is how to update or adapt safely without leakage, catastrophic forgetting, hidden calibration drift or loss of previously validated performance.

## 9.17 Acquisition quality, harmonization and failure detection

Quality-control AI sits upstream of all interpretation models. The branch spans protocol adequacy, artifact/motion detection, harmonization, series selection and automated abstention/repeat recommendations [S115]. Its importance is often underrepresented because it is infrastructure-like, yet failures here propagate into every downstream endpoint.

## 9.18 Quantitative imaging, radiomics and imaging phenomics

Radiomics and deep imaging biomarkers remain active parallel approaches to extracting prognostic, molecular and response information from images [S116]. Foundation-model embeddings increasingly intersect this branch, as illustrated by CXR disease-phenomics work [S112]. The unresolved field issue is reproducible biomarker validity across acquisition/site shift and whether predictive associations add clinical utility beyond conventional imaging and non-imaging covariates.

## 9.19 Synthetic data and simulation

Generative imaging is now both a model frontier and a **data-engineering strategy** [S111]. Key unresolved issues include whether synthetic cohorts improve rare-event/generalization performance, whether they preserve pathology/biology, how privacy should actually be measured, and whether synthetic-data benefits survive independent external validation.

## 9.20 The overarching frontier pattern

Across these branches, the field is moving simultaneously in several directions:

``` text
2D -> native 3D / 4D
single image -> study / patient / longitudinal context
fixed task -> reusable representation / prompting / adaptation
prediction -> grounding / measurement / auditability / tools
single site -> multicentre / health-system scale
offline benchmark -> reader study / workflow / prospective evidence
pixels alone -> image + text + concepts + metadata + molecular/spatial context
headline accuracy -> calibration + robustness + privacy + security + efficiency
```

The important landscape lesson is that these transitions occur at **different maturity levels in different modalities**. There is no single “foundation-model era” shared uniformly across medical imaging.

------------------------------------------------------------------------

# 10. Expert Landscape Learning Map

The learning map is organized by **conceptual dependency and field structure**, not publication hype. A learner should understand the modality, paradigm, data regime, evidence maturity and failure modes before memorizing individual model names.

### Curriculum balance rule

Target approximately **60% field structure/methodology/independent evidence/failure analysis** and **40% individual model families**. A model earns dedicated attention because it represents an important paradigm, modality branch, evidence transition, or historically influential lineage—not merely because it is new.

## Phase 0 — prerequisites

### M00. Medical imaging geometry and data semantics

DICOM study/series/instance hierarchy; NIfTI; LPS/RAS; spacing; orientation; resampling; anisotropy; physical coordinates; acquisition protocol metadata.

### M01. Imaging physics and inverse problems

MRI k-space/parallel imaging/compressed sensing; CT projections/dose/reconstruction kernels; ultrasound acquisition physics; OCT volume formation; why “image quality” cannot be separated from the forward model.

### M02. Representation learning foundations

DINO/DINOv2/**DINOv3**, MAE/SimMIM, contrastive image-text learning, masked modelling, diffusion representations, concept alignment, linear probing, fine-tuning and PEFT. Study generic-pretraining transfer as an empirical question: DINOv3 can be highly competitive on some medical tasks yet degrade on highly specialized PET/WSI/EM settings. [S86]

### M03. Evaluation and trustworthiness

Calibration, uncertainty, shift, confounding, contamination, leakage, fairness, statistical uncertainty, external/prospective validation, reader studies and workflow outcomes; patient re-identification/near-duplicate controls; closed-form versus open-ended clinical-competency evaluation; lifecycle validation and re-entry after drift or model updates. [S65–S68]

### M03A. Dataset provenance, genealogy and access

Patient/study/series/reconstruction/image/WSI/patch units; human versus weak/model/LLM-generated supervision; DUA/gated/challenge access; parent→derivative datasets; pretraining overlap; version pinning; external-cohort design. Use the Section 3A dataset ledger as the minimum practical curriculum.

### M03B. Statistical inference for imaging AI

Independent statistical unit; clustered/patient-level bootstrap; paired model comparison; confidence intervals on differences; multiplicity; hierarchical site effects; calibration; sample-size/power; non-inferiority/equivalence; reader-study analysis.

### M03C. Reference standards and reporting frameworks

Reference-standard quality, adjudication and incorporation bias; CLAIM 2024; STARD-AI; TRIPOD+AI; DECIDE-AI; SPIRIT-AI; CONSORT-AI; PROBAST+AI; FUTURE-AI. Learn why reporting completeness, risk of bias and evidentiary strength are different quantities. [S70–S75,S79,S80]


### M03D. Privacy and multimodal security

Re-identification versus membership inference; patient-level privacy tails; differential privacy; attacker knowledge; prompt/OCR injection; temporal visual attacks; transferable imperceptible attacks; benign-performance/false-refusal trade-offs; federated learning does not automatically eliminate post-training privacy risk. [S65,S66,S95–S98,S101]


### M03E. Label-efficient and data-centric medical imaging

Weak/semi-supervised learning, active learning, pseudo-labelling, partial labels, heterogeneous-label training, annotation cost and label provenance. Use the 2026 350+ study survey and Ark/Ark+ as anchors for why “better data/supervision” is a frontier independent of model scale. [S108,S106,S107]

### M03F. Domain robustness and model evolution

Domain generalization/adaptation, OOD/selective prediction, test-time adaptation, harmonization, drift, recalibration and continual/domain-incremental learning. [S109,S110]

### M03G. Synthetic data and simulation

GAN/diffusion/other generators as data tools; fidelity versus utility versus privacy; rare-case synthesis; paired translation; simulation and synthetic pretraining. [S111]

### M03H. Quantitative imaging and imaging biomarkers

Radiomics, deep radiomics, phenomics, opportunistic screening, prognosis, treatment-response biomarkers, feature stability, harmonization and external/prospective validation. [S112,S116]

### M03I. Acquisition quality and harmonization

Automated image-quality assessment, motion/artifact detection, protocol adequacy, series selection, repeat-scan logic and scanner/site harmonization. [S115]

## Phase 1 — supervised and generalist segmentation

### M04. nnU-Net v2

Reproduce a task-specific baseline and understand why self-configuration remains difficult to beat.

### M05. VISTA3D + NV-Segment-CT/CTMR

Automatic versus interactive generalists; ontology/version drift; CT-to-CT/MRI expansion.

### M06. SAT3D

Uncertainty-aware tumour segmentation; confidence maps as prompts; whole-body generalization claims.

### M07. MedSAM2

3D-as-video propagation, prompt persistence, annotation-efficiency evidence.

### M08. BiomedParse v1 -> v2

Text-conditioned parsing; 2D-to-3D lineage; existence detection; challenge evidence versus peer-review evidence.

### M09. Medical SAM3

Natural-image-to-medical adaptation; prompt dependence; full fine-tuning across heterogeneous datasets.

## Phase 2 — reusable and auditable medical representations

### M10. MedSigLIP + MedImageInsight + RAD-DINO

General versus modality-specific encoders; retrieval/classification/low-label adaptation.

### M11. ConceptCLIP

Global image-text alignment versus region-concept alignment; UMLS-linked concept spaces; interpretability versus causality.

## Phase 3 — native volumetric radiology

### M12. RadFM as historical 2D/3D bridge

Understand the earlier generalist 2D/3D VLM lineage before judging 2026 models.

### M13. CT-RATE / CT-CLIP / CT-CHAT

Dataset construction, 3D contrastive learning, synthetic instruction generation.

### M14. Merlin

EHR diagnosis codes + radiology reports + 3D CT; internal versus large external evaluation.

### M15. Triad

MRI-specific SSL and the evidence that modality/task match can matter more than raw scale.

### M16. Decipher-MR

3D MRI vision-language representation across sequence/anatomy/pathology.

### M17. Prima + NeuroVFM: health-system learning

Compare report-supervised health-system MRI learning (Prima) with imaging-first volumetric predictive learning across routine CT/MRI (NeuroVFM). Study what changes when supervision, data curation and task definition change while the health-system setting remains central.

### M18. Brainfound versus BrainFound naming collision

Study the *Patterns* multimodal CT/MRI Brainfound and separately recognize the *npj Imaging* self-supervised brain-MRI BrainFound; never conflate their evidence, datasets or capabilities.

### M19. MedGemma 1.5

General multimodal 3D-capable model; official benchmark limitations, contamination warnings and deployment terms.

## Phase 4 — grounded/auditable radiology

### M20. CLEAR

Concept bottlenecks/embeddings and decomposable predictions.

### M21. MAIRA-2

Grounded report generation, priors, reporting context, RadFact.

### M22. CARE-X + deterministic tools

Separate auxiliary-supervision/reward-alignment evidence from the independent measurement-tool experiment.

## Phase 5 — reconstruction and registration

### M23. DeepSparse + universal CT enhancement

Sparse-view reconstruction versus post-reconstruction enhancement; physics/data-consistency evaluation.

### M24. Vision-language-guided fast MRI reconstruction

Semantic priors, data fidelity and hallucination testing.

### M25. uniGradICON + LUMIR 2026

Zero-shot registration, diffeomorphism/topology, TRE/Jacobian metrics, protocol/species shift.

## Phase 6 — modality-specific FMs

### M26. OCTCube-M + volumetric OCT + RETFound Plus + FOCUS

Study four distinct axes: native 3D OCT representation, volume-versus-slice modelling, longitudinal fundus risk modelling, and end-to-end 3D OCT workflow integration.

### M27. NeuroSTORM + operator-interactive ultrasound

Spatiotemporal fMRI and Sonomate-style operator-interactive ultrasound: learn why time/acquisition process is part of the learning problem. [S20,S59]

### M27A. EchoCLIP -> EchoPrime + Echo-Vision-FM

Study the transition from single-view/frame-centric echocardiography VLMs to video- and **study-level multi-view** representation. Audit shared Cedars-Sinai lineage, view classification, anatomical attention, retrieval-augmented interpretation, external health-system evaluation and the distinction between retrospective performance and clinical workflow utility. [S83,S84,S87]

### M28. LCTfound + SKELEX + mammography + endoscopy case studies

Workflow-bounded modality specialists: lung CT across image formation and clinical tasks, musculoskeletal radiographs, mammographic multiview/priors, and procedure-level endoscopy. Use these as counterexamples to the assumption that one universal encoder is automatically optimal.

### M28A. PET/CT and nuclear-medicine representation learning

Study whole-body FDG PET/CT pretraining, autoPET, tracer/reconstruction/SUV semantics, early versus late PET–CT fusion, and the negative lesson that natural-image pretraining may not transfer cleanly to functional imaging. [S76,S77,D11A]

### M28B. Radiotherapy imaging semantics

DICOM RTSTRUCT/RTDOSE/RTPLAN linkage, Frame of Reference, TG-263 nomenclature, ROI classification/renaming, laterality verification and downstream spatial-safety consequences. [S78]

### M28C. PanDerm and multimodal dermatology

Clinical photography, dermoscopy, total-body-photography lesion tiles and dermatopathology in one specialty FM; patient/lesion/visit hierarchy; skin-tone/device/geographic shift; longitudinal lesion identity; reader-study versus prospective-utility evidence. [S88]

### M28D. FluoResFM and computational microscopy restoration

Denoising/deconvolution/super-resolution across heterogeneous microscopy structures and acquisition conditions; textual prior conditioning; external unseen datasets; prompt-mismatch hallucination; preservation of quantitative morphology. [S89]


### M28E. Dental/oral imaging: PanoFM, DentFound and DentVLM

Separate reusable panoramic representation learning from downstream panoramic-reporting VLMs and multimodal dental assistants. Study patient/tooth/view hierarchy, dentition stage, site/device shift, bilingual VQA construction, human–AI collaboration and the difference between broad specialty task coverage and Core-FM eligibility. [S92–S94]

## Phase 7 — computational pathology

### M29. Prov-GigaPath + Flash

Tile-to-slide hierarchy, LongNet, efficiency claims and preprint/evidence separation.

### M30. Virchow / Virchow2, UNI / UNI2-h, H-Optimus-1

Scale, data composition, magnification, licensing and representation differences.

### M31. TITAN + PRISM2

Slide-language integration and clinical dialogue.

### M31A. CARE: adaptive morphological regions + molecular guidance

Study adaptive region construction as an alternative to fixed patch aggregation, and ask whether RNA/protein alignment improves biological transfer beyond morphology-only pretraining under independent evaluation. [S90]

### M32. Independent model-selection + robustness benchmarks

32-model PathBench, Sep-2026 six-model trade-off study, PathoROB, and the ten-dataset mammography shortcut study; compare technical/dataset identity leakage across modalities rather than assuming pathology is unique. [S29,S47,S82,S99]

### M33. nnMIL

Slide-level aggregation, stochastic feature sampling and uncertainty.

### M33A. PathSegmentor + CRISP

Natural-language pathology segmentation versus clinically oriented intraoperative frozen-section modelling. Study dataset genealogy/prompt robustness for PathSegmentor and intended-use/prospective human–AI evidence for CRISP; do not collapse promptable spatial parsing and intraoperative diagnostic support into one capability claim. [S103,S104]

### M34. GigaTIME + Flash + VirTues

Contrast morphology-to-virtual-mIF inference with direct multiplex spatial-proteomics foundation modelling; paired validation, marker-panel shift and biological limits.

### M34A. XGeM and multimodal generative synthesis

Any-to-any multimodal generation, shared latent spaces and multi-prompt conditioning; distinguish architectural breadth from evidence breadth and understand why this dossier withholds `core_fm` promotion until broader demonstrated transfer exists; audit synthetic-data utility, privacy claims, realism and clinical consistency separately. [S91]

## Phase 8 — clinical translation and engineering

### M35. Task-specialized clinical FMs

AbdomenNet, OMAFound and other workflow-bounded systems as counterexamples to “universal is always better.”

### M36. MONAI + MONAI Deploy

Reproducible transforms/inference, DICOM/FHIR, packaging and deployment boundaries.

### M37. REAL-FM / clinical translation

Technical readiness, clinical value, workflow integration, responsible AI and prospective evidence.

### M38. Clinical-competency and lifecycle evaluation

Study the B&J benchmark and the 2026 five-phase evaluation framework. Learn to separate multiple-choice/closed-form success from open-ended image-dependent competence, and map technical validation -> operational robustness -> controlled human-AI interaction -> clinical evidence -> real-world integration. Overlay the appropriate reporting and risk-of-bias framework without mistaking checklist compliance for stronger evidence. [S67,S68,S70–S75,S79,S80]

------------------------------------------------------------------------

# 11. Landscape Entry Template

Every future model-family or modality note should use the same structure so that the dossier remains a comparable landscape rather than a collection of paper summaries.

## I — Identity and place in the field

- object type: model / method / dataset / benchmark / workflow / review / infrastructure;
- canonical `system_class` (`core_fm / fm_derived / fm_enabled / companion`) **when applicable**;
- canonical `scope` (`generalist / modality_generalist / specialty_generalist / organ_specialist / workflow_specialist`) **when applicable**;
- `foundation_lineage` where relevant;
- imaging-problem-stack position (Axis A);
- learning/data paradigm (Axis B);
- clinical task/use role (Axis C);
- modality / specialty;
- primary technical role;
- what branch or lineage it represents;
- `audit_completeness` for the surrounding branch.

## A — Architecture and learning regime

- input representation;
- vision backbone;
- text/language component where relevant;
- spatial/temporal/study-level context;
- pretraining objectives;
- adaptation/prompt/tool interface;
- parameter count only when primary/officially sourced.

## D — Data and provenance

- pretraining data scale and counting unit;
- patient/study/series/image/WSI/patch hierarchy;
- dataset genealogy and parent/derivative relationships;
- label/reference-standard provenance;
- geography/institution/acquisition scope;
- public/private/gated status;
- likely pretraining overlap or contamination concerns.

## E — Evidence profile

- publication maturity and evidence code(s);
- evaluator independence;
- cohort/institution/geographic/temporal/acquisition independence;
- adaptation regime;
- comparator fairness;
- primary metrics and uncertainty;
- reference-standard quality;
- risk of bias/applicability;
- study temporality and workflow exposure;
- artifacts and replication status.

## C — Counter-evidence and scope boundary

- strongest independent evaluation, if available;
- strongest negative result or contradictory benchmark;
- known failure modes;
- evidence that narrows developer claims;
- `counterevidence_status = found / searched_none_found_as_of_DATE / not_audited`;
- what the current evidence **does not** establish.

## M — Maturity and current role

Record separately:

``` text
technical_maturity
independent_evidence_maturity
external_validation_maturity
clinical_translation_maturity
openness_reproducibility_maturity
```

Then summarize:

- why the system matters to the current landscape;
- whether it is an anchor, comparator, historical lineage, emerging watchlist item, or workflow example;
- which field uncertainty it informs;
- which neighboring systems must be understood alongside it.

## R — Reproducibility / availability

- official repository/model card;
- exact version/checkpoint;
- artifact axis: code / weights / data / executable pipeline;
- access axis;
- reuse-rights axis;
- smallest supported inference path;
- hardware/precision/input configuration when measured;
- known implementation/version drift.

The purpose of this template is **descriptive and comparative**. It should not end by inventing a research project for every model.

------------------------------------------------------------------------

# 12. Hardware and Reproduction Policy

The previous dossier included a large table of estimated VRAM values. It has been removed because most entries were not traceable to a common, reproducible protocol.

## New rule

Record hardware only as **measured experiment metadata**:

``` yaml
model: exact checkpoint
commit: git SHA / model revision
hardware: GPU model + VRAM
precision: fp32 / bf16 / fp16 / int8 / int4
input: exact spatial dimensions / number of slices / number of tiles
batch_size: N
backend: PyTorch / TensorRT / vLLM / etc.
peak_vram_gb: measured
latency_s: measured, including or excluding preprocessing explicitly
throughput: cases/hour or tiles/s
```

A number without those conditions should not be presented as a property of the model.

------------------------------------------------------------------------

# 13. Reproduction Entry Points — No Simulated Clinical Outputs

The previous dossier contained “conceptual interfaces” that returned fabricated masks, diagnoses, measurements, or reports. Those examples have been removed.

### Why

A fake API can accidentally teach an interface, tensor convention, capability, or output structure that the actual project does not provide. For an expert curriculum, that is worse than having no code.

### Replacement policy

For each model:

1.  use the official repository/model card;
2.  pin a release/commit/checkpoint;
3.  run the authors’ smallest supported example;
4.  record preprocessing exactly;
5.  reproduce one published benchmark component when feasible;
6.  only then create a local wrapper.

### Verified official starting points

- VISTA3D / NV-Segment: Project MONAI / NVIDIA-Medtech repositories;
- MedSAM2: official project/repository;
- BiomedParse v2: `microsoft/BiomedParse`;
- Medical SAM3: `AIM-Research-Lab/Medical-SAM3` (preprint status retained);
- Merlin: `StanfordMIMI/Merlin`, Hugging Face and PyPI referenced by the Nature paper;
- MedGemma / MedSigLIP: Google Health AI Developer Foundations model cards and notebooks;
- Prov-GigaPath: `prov-gigapath/prov-gigapath`;
- UNI2-h: MahmoodLab Hugging Face/model repository;
- H-Optimus-1: Bioptimus Hugging Face/model card;
- nnMIL: repository linked from the 2026 Nature Biomedical Engineering paper;
- MONAI / MONAI Deploy: official Project MONAI repositories and documentation.
- PanDerm: official encoder code/weights linked by the *Nature Medicine* paper [S88].
- EchoPrime: public code/weights/demo linked by the *Nature* paper [S83].
- FluoResFM: implementation/napari workflow linked by the *Nature Communications* paper [S89].

------------------------------------------------------------------------

# 14. Red-Team Audit of the Previous Dossier

The following defects were found and corrected during this rebuild.

| Previous issue                                                                            | Audit result                                                                                                                                | Corrective action                                                                                                                                   |
|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------|
| “9.9/10 — Approved & Frozen” / “FROZEN GOLD STANDARD”                                     | scientifically inappropriate for a fast-moving 2026 field                                                                                   | replaced with living evidence-audited snapshot                                                                                                      |
| four-paradigm map presented as the field                                                  | omitted major representation, native 3D radiology, ultrasound, reconstruction/enhancement and clinical-readiness branches                   | replaced by a problem-centric map                                                                                                        |
| radiology frontier centred mainly on MedGemma 1.5                                         | omitted Merlin, CT-RATE/CT-CLIP/CT-CHAT, Decipher-MR, Prima, CLEAR                                                                          | added as core anchors                                                                                                                               |
| pathology benchmark cited as Nature Communications Article 5821                           | incorrect                                                                                                                                   | corrected to **17, Article 9012 (2026)**                                                                                                            |
| Prov-GigaPath described as trained on 31,216 WSIs                                         | incorrect                                                                                                                                   | corrected to **171,189 slides**, 1,384,860,229 tiles, >30k patients                                                                                |
| GigaPath-Flash dated 31 Aug 2026                                                          | primary preprint submitted 20 Jul 2026                                                                                                      | corrected date and labelled E3 preprint                                                                                                             |
| UNI2-h listed as 632M                                                                     | official model card says custom ViT-H **681M**                                                                                              | corrected                                                                                                                                           |
| Virchow-2 treated as Nature Medicine 2024 publication                                     | 2024 Nature Medicine paper is **Virchow**; Virchow2 is later scaling work/preprint/model                                                    | evidence lineage separated                                                                                                                          |
| H-Optimus-1 treated as 2026 model release                                                 | model launched Apr 2025; 2026 evidence surfaced as AACR Cancer Research abstract                                                            | release date and evidence level separated                                                                                                           |
| MedSAM2 exact parameter/VRAM/timing/BTCV/cardiac metrics                                  | not supported by the primary MedSAM2 sources checked; likely conflation with other SAM-derived systems/results                              | removed; retained only verified dataset/user-study claims                                                                                           |
| BiomedParse v2 inherited v1 peer-review status                                            | v1 is Nature Methods; v2 is an evolving 3D repo/challenge release                                                                           | evidence classes separated                                                                                                                          |
| BiomedParse v2 exact false-positive percentage and VRAM                                   | not verified from primary v2 source in this audit                                                                                           | removed                                                                                                                                             |
| VISTA3D “127 classes” treated as fixed                                                    | paper/research README says 127; current bundle comparison says 132                                                                          | both recorded with version context                                                                                                                  |
| CARE-X + Tools described as one integrated architecture                                   | preprint states the deterministic-tool measurement experiment is **separate**, using Qwen3-VL-4B-Instruct                                   | separated explicitly                                                                                                                                |
| MedGemma 1.5 described as if it defined the volumetric frontier                           | official card shows promising but limited benchmark results and explicit validation/contamination caveats                                   | reframed as one general-purpose 3D-capable VLM among several native 3D systems                                                                      |
| blanket “open-source” phrasing                                                            | several models use custom, gated, non-commercial or no-derivatives terms                                                                    | added availability taxonomy                                                                                                                         |
| estimated VRAM table                                                                      | estimates mixed incompatible configurations and looked more authoritative than evidence justified                                           | removed; replaced by measurement protocol                                                                                                           |
| simulated Medical SAM3 / MedGemma / CARE-X interfaces                                     | fabricated outputs and non-authoritative APIs risk teaching false implementation details                                                    | removed; replaced with official reproduction entry points                                                                                           |
| “five golden research challenges”                                                         | implied exclusivity and certainty                                                                                                           | replaced with falsifiable research programs and null hypotheses                                                                                     |
| first evidence-audited rewrite still omitted ConceptCLIP                                  | missed a peer-reviewed Aug-2026 explainable biomedical FM covering 78 datasets/10 modalities                                                | promoted ConceptCLIP to the core representation branch                                                                                              |
| first rewrite still omitted SAT3D                                                         | missed peer-reviewed Aug-2026 uncertainty-aware whole-body tumour segmentation                                                              | added SAT3D to core segmentation anchors                                                                                                            |
| reconstruction/registration treated mainly as companion material                          | no longer defensible after DeepSparse, Triad and the Sep-2026 LUMIR follow-up                                                               | added a dedicated inverse-imaging/registration frontier branch                                                                                      |
| ophthalmology/OCT absent                                                                  | omitted a mature FM subfield and 2026 3D multimodal OCT evidence                                                                            | added OCTCube-M and the volumetric V-JEPA benchmark                                                                                                 |
| no explicit task-specialized FM counterweight                                             | risked implying the field only moves toward universal generalists                                                                           | added AbdomenNet, OMAFound, mammography and endoscopy case studies                                                                                  |
| pathology evidence stopped before September                                               | missed the 2 Sep 2026 six-model model-selection/efficiency study                                                                            | added a dated September evidence delta                                                                                                              |
| possible Brainfound/BrainFound conflation                                                 | two distinct 2026 neuroimaging model lineages use near-identical names                                                                      | added an explicit naming-collision warning                                                                                                          |
| future-event metadata can look already completed                                          | MICCAI 2026 is scheduled for 27 Sep–1 Oct 2026, after this dossier snapshot                                                                 | added temporal verification rule; no MICCAI-2026 conference result is treated as already presented                                                  |
| second rewrite still underrepresented health-system learning                              | Prima alone missed the July 2026 *Nature Medicine* NeuroVFM evidence and direct comparison of pretraining paradigms                         | added NeuroVFM and explicitly paired it with Prima                                                                                                  |
| structural imaging emphasis hid 4D/temporal FM work                                       | omitted NeuroSTORM fMRI and RETFound Plus longitudinal retinal learning                                                                     | added both as separate spatiotemporal/longitudinal anchors                                                                                          |
| spatial biology was framed mainly as H&E -> virtual mIF                                  | omitted direct foundation modelling of measured multiplex spatial proteomics                                                                | added VirTues as the complementary direct-measurement branch                                                                                        |
| ophthalmic branch stopped at representation benchmarks                                    | omitted Aug-2026 FOCUS workflow validation                                                                                                  | added FOCUS but explicitly labelled it a foundation-model-driven workflow rather than a new universal backbone                                      |
| privacy risk could be stated too simplistically                                           | re-identification evidence has a 2026 methodological challenge implicating near-duplicate images                                            | added claim-versus-counterevidence privacy audit rule                                                                                               |
| evaluation doctrine still over-relied on retrospective benchmark thinking                 | 3 Sep 2026 B&J benchmark shows strong closed-question scores can coexist with weak open-ended image-dependent competency                    | added clinical-competency red team, text-only ablation, and open-ended evaluation requirements                                                      |
| translation ladder was directionally correct but insufficiently operational               | 3 Sep 2026 five-phase framework separates technical, operational, interaction, clinical-evidence, and real-world stages with phase re-entry | upgraded R9 and curriculum to lifecycle validation                                                                                                  |
| privacy/distributed-learning section could imply centralization is the only serious route | 1 Sep 2026 federated-FM perspective formalizes a major emerging direction                                                                   | added federated-FM audit branch but explicitly kept it as prospective methodology, not proven solution                                              |
| dataset scale                                                                             | patients, studies, series, reconstructions, images and patches were easy to conflate                                                        | added hard numerical-unit discipline and corrected CT-RATE/MR-RATE/MIMIC examples [D12,D20,D23]                                                   |
| dataset independence                                                                      | derivative annotation datasets could be mistaken for external cohorts                                                                       | added explicit CT-RATE→RadGenome/PatchChestCT and MIMIC→MS-CXR/Chest ImaGenome genealogy maps [D17,D18,D21,D22]                                   |
| synthetic / model-assisted labels                                                         | “ground truth” could hide weak/model/LLM supervision                                                                                        | added label-provenance fields and explicit caveats for RadGenome, AbdomenAtlas 3.0, Chest ImaGenome and MR-RATE derivatives [D5,D18,D21,D23]      |
| dataset accessibility                                                                     | “public” could hide DUA/gating/mixed licences                                                                                               | added unrestricted/credentialed/gated/challenge/derivative access distinctions across the dataset ledger                                            |
| link integrity                                                                            | publisher access redirects can be mistaken for broken citations                                                                             | re-opened all 72 legacy source URLs; canonical DOI/title metadata was used to verify publisher links that the web verifier could not fetch directly |
| dataset-version citation mismatch                                                         | expanded NIH ChestX-ray14 counts were initially paired with the earlier ChestX-ray8 CVPR link                                               | corrected D15 to the canonical NIH expanded-release repository and documented the 108,948/32,717 vs 112,120/30,805 lineage                          |
| E1–E5 could be misread as a strength hierarchy                                            | source type, independence, externality, clinical stage and reproducibility are orthogonal                                                   | retained E1–E5 only as source-provenance codes and added a multidimensional evidence profile                                                        |
| availability labels collapsed artifacts, access and reuse rights                          | “weights available” could hide gated/non-commercial/no-derivatives terms                                                                    | split availability into artifact, access and reuse-rights axes                                                                                      |
| evaluation doctrine lacked formal statistical requirements                                | clean provenance does not guarantee valid inference                                                                                         | added patient/cluster-level uncertainty, paired differences, multiplicity, site heterogeneity, power and reader-study rules                         |
| “ground truth” could hide uncertain targets                                               | medical labels are often fallible reference standards                                                                                       | added reference-standard taxonomy and CLAIM/STARD-AI terminology                                                                                    |
| clinical reporting guidance was under-anchored                                            | lifecycle framing lacked study-type reporting standards                                                                                     | integrated CLAIM 2024, STARD-AI, TRIPOD+AI, DECIDE-AI, CONSORT-AI and FUTURE-AI while separating reporting quality from evidence strength           |
| R2 could not isolate the value of native 3D                                               | frontier systems differ simultaneously in data, scale, architecture and supervision                                                         | split R2 into controlled dimensionality ablation and ecological frontier comparison                                                                 |
| R3 conflated grounding, longitudinal matching and measurement                             | failures could not be attributed to the responsible stage                                                                                   | split R3 into R3A static grounding, R3B correspondence and R3C quantitative change                                                                  |
| R4 tool use could move rather than solve errors                                           | end-to-end gains do not identify perception/tool/reasoning contributions                                                                    | added oracle-localization, oracle-measurement and oracle-tool-output controls                                                                       |
| PET/SPECT remained a footnote                                                             | 2026 PET/CT-specific pretraining now provides a meaningful domain-alignment frontier                                                        | added dedicated PET/CT branch, curriculum module and R11                                                                                            |
| radiotherapy imaging semantics absent                                                     | FM workflows now interact with CT + RTSTRUCT and cross-institution ROI nomenclature                                                         | added radiotherapy branch, curriculum module and R12                                                                                                |
| RAD-DINO scientific evidence understated                                                  | dossier listed only official lineage despite a peer-reviewed Nature Machine Intelligence paper                                              | upgraded RAD-DINO to E1 scientific + E2 release evidence and corrected S53                                                                          |
| H-Optimus evidence label contained “E3?”                                                  | unresolved punctuation is inappropriate in a final evidence matrix                                                                          | replaced with explicit E3 abstract-level + E2 model-card wording                                                                                    |

| v3.4 canonical evidence schema conflicted with legacy fields | old independence/reproducibility/contamination fields remained above the new doctrine | replaced with one canonical evidence profile |
| E5A/E5B declared but E5 remained canonical | empirical replication and synthesis still shared a table code | replaced E5 with E5A/E5B in the source-provenance table |
| Claim Ledger and Section 15 diverged | 12 ledger claims but 24 conclusion statements | expanded/typed ledger and made Section 15 a direct C1–C14 projection |
| ultrasound branch omitted EchoPrime/EchoCLIP lineage | missed a major Nature 2026 multi-view echocardiography FM and shared training lineage | added EchoPrime/EchoCLIP/Echo-Vision-FM branch and split R8 |
| generic-pretraining counter-evidence missing | domain-alignment claim risked one-sided interpretation | added DINOv3 medical benchmark as counter-evidence |
| MammoScope named without direct source | watchlist item was source-laundered through a general branch | added E3 primary abstract source S85 |
| study template retained obsolete contamination risk/external-site shorthand | implementation lagged doctrine | replaced with canonical contamination status + multidimensional independence + replication fields |
| v3.5 build artifacts duplicated headings / left one escaped citation | source-generation pipeline corrupted otherwise-correct Markdown | removed malformed headings, fixed citation, and added mechanical heading/citation QA |
| EchoPrime ledger author line was wrong | URL identity was correct but structured metadata verification was incomplete | corrected S83 to Vukadinovic, Chiu, Tang et al.; separated URL verification from metadata verification |
| DINOv3 technical-report citation was version-stale | live preprint title/revision had evolved | pinned S86 to arXiv v3, revised 17 Jan 2026 |
| dermatology under-covered | PanDerm provides peer-reviewed multimodal specialty-FM evidence across four dermatology modalities | added dedicated dermatology branch and curriculum module |
| biomedical microscopy under-covered | FluoResFM provides 2026 peer-reviewed restoration-FM evidence across heterogeneous fluorescence datasets | added computational-microscopy branch and curriculum module |
| CARE missing from pathology architecture map | adaptive morphological regions + molecular guidance are a distinct WSI modelling direction | added CARE to whole-slide/molecular-guided pathology branch |
| synthesis branch too narrow | XGeM provides peer-reviewed multimodal generative-FM evidence but with CXR-centric validation | added XGeM with explicit evidence-scope limitation |
| v3.6 system taxonomy still drifted in narrative tables | descriptive strings such as FM-derived clinical system / FM-enabled method fragmented the enum | added authoritative Section 4.0 registry and exact four-value `system_class` enum |
| scope enum contradicted PanDerm label | `specialty-generalist` was used without being defined | added canonical `specialty_generalist` scope and exact enum enforcement |
| C12 citation range leaked PET/RT sources | [S70–S80] accidentally included S76–S78 | narrowed support to [S68,S70–S75,S79,S80] |
| curriculum still referenced C1–C12 | ledger had already expanded to C1–C14 | fixed and allowed explicit field-state/research-program mapping |
| Section 15.1 citation scope was too broad | PanDerm/FluoResFM citations were attached to unrelated federated/RT/reconstruction observations | split observations into separately supported bullets |
| dental/oral imaging absent | 2026 PanoFM, DentFound and DentVLM establish a substantial specialty branch | added dental anchor section, registry entries, curriculum and genealogy scaffold |
| privacy focused mainly on re-identification | membership inference has distinct patient-level risk evidence | added Nature 2026 patient-level MIA audit and explicit privacy threat taxonomy |
| security named but barely evidenced | medical VLM prompt injection/adversarial attacks are already peer-reviewed | added dedicated security branch and R14 |
| shortcut robustness framed mainly through pathology | mammography shows dataset-identity entanglement across ten heterogeneous datasets | broadened C7/C8 and added R13 |
| UniFMIR named without a source | real 2024 Nature Methods FM lineage was uncited | added S100 and integrated it into microscopy |
| federated branch was perspective-only | 2026 empirical embedding-based FL study provides bounded systems evidence | added S101 without treating federated learning as a privacy theorem |
------------------------------------------------------------------------


### v4.0 landscape-only architecture change

The dossier previously contained a Claim Ledger and a Research Blueprint (R1–R14). Those structures were useful while hardening the evidence logic but were removed from the canonical landscape edition because they mixed **field mapping** with **downstream project design**. Their evidence content is preserved in the Field-State Ledger (§1.5) and Open Frontiers (§9), where it is expressed as current state, maturity, counter-evidence and unresolved uncertainty rather than as hypotheses, null hypotheses or proposed experiments.

# 15. Current Field-State Synthesis and Maturity Map

As of **11 September 2026**, the medical-imaging AI landscape is highly uneven. Some branches now have deep competitive ecosystems and independent benchmarking; others are represented by a small number of technically important systems with limited external or prospective evidence.

## 15.1 Cross-field synthesis

1. **Task-specific supervised systems remain central.** Foundation models have not made strong task-specific methods obsolete; nnU-Net-like systems remain necessary reference comparators for segmentation and similar tightly specified tasks.
2. **Reusable representation learning is a major layer of the field in its own right.** General medical encoders, modality-specific SSL and image–text alignment frequently matter more than generative chat capability for retrieval, classification, adaptation and data curation.
3. **Native volumetric modelling is now established across CT/MRI/OCT, but the isolated causal value of “3D” remains incompletely resolved.** Direct evidence is strongest in volumetric OCT; CT provides meaningful ecological comparative evidence; MRI provides broad transfer evidence. [S40,S46,S81]
4. **Promptable/generalist 3D segmentation is technically established, while human-efficiency evidence remains less mature than benchmark coverage.** [S3–S8,S39]
5. **Grounded radiology is shifting the target from fluent prose to localizable, auditable and quantitatively verifiable claims.** Independent report-generation evidence also shows that clinically relevant quality can differ sharply between systems. [S17–S19,S102]
6. **Ultrasound is bifurcating into acquisition-interactive systems and examination-level video/multi-view representation.** Echocardiography is one of the clearest study-level modelling ecosystems. [S20,S83,S84,S87]
7. **Reconstruction, restoration and registration are genuine FM-era branches**, but physics/data consistency, anatomy/topology preservation and quantitative fidelity remain mandatory evaluation layers. [S34,S40–S44,S105]
8. **Ophthalmology is a substantial specialty FM ecosystem**, spanning volumetric OCT, longitudinal fundus modelling and workflow integration. [S45,S46,S55,S61,S62]
9. **Computational pathology is the deepest competitive FM ecosystem in this dossier.** It contains large tile encoders, WSI aggregation, multimodal slide systems, natural-language spatial parsing, adaptive regions, efficiency/distillation, independent model-selection studies, robustness benchmarks and unusually strong prospective translation evidence through CRISP. [S22–S31,S47,S82,S90,S103,S104]
10. **Spatial biology now contains two distinct FM frontiers:** morphology-to-virtual molecular inference and direct modelling of measured multiplex spatial proteomics. [S32,S60]
11. **Specialty branches matter.** Dermatology, dental/oral imaging, musculoskeletal radiography, mammography, endoscopy, lung CT and acute-abdomen systems show that workflow/modality specialization can be more clinically meaningful than universal model breadth. [S48–S51,S63,S64,S88,S92–S94]
12. **PET/CT is credible but still comparatively emerging**, and SPECT remains a clear coverage gap. [S76,S77]
13. **Radiotherapy exposes a different frontier:** spatial-object correctness and semantic interoperability across CT/RTSTRUCT/RTDOSE/RTPLAN, not merely image interpretation. [S78]
14. **Temporal/spatiotemporal learning is real but causally delicate.** Predictive gains in retina/fMRI do not establish biological disease dynamics without controlling identity, treatment, utilization and censoring. [S59,S61]
15. **Clinical translation remains the major bottleneck across most branches.** Prospective/workflow evidence exists for selected specialty systems, but the majority of FM work remains retrospective/offline. [S2,S68,S104]
16. **Privacy and security are now part of the technical landscape, not optional appendices.** Re-identification, membership inference, prompt injection and transferable multimodal attacks require separate threat models. [S65,S66,S95–S98]
17. **Label-efficient/data-centric learning remains a major active frontier** and cannot be reduced to a historical pre-FM baseline; heterogeneous-label, weak/semi-supervised and active-learning strategies still determine practical data efficiency [S108,S106,S107].
18. **Domain robustness and continual model evolution remain unsolved cross-cutting problems.** A large pretrained model is not automatically site/scanner/protocol robust or safely updateable over time [S109,S110].
19. **Synthetic data, acquisition/QC and quantitative imaging are parallel technical ecosystems**, not peripheral implementation details [S111,S115,S116].
20. **Imaging can serve as a biomarker platform beyond ordinary visual diagnosis.** CXR disease-phenomics provides a current example, but predictive associations require calibration, covariate comparison and prospective utility evidence [S112].
21. **No global “SOTA medical-imaging model” is scientifically meaningful.** The relevant unit is imaging problem × learning paradigm × clinical role × modality × adaptation × cohort × metric × deployment constraint × evidence maturity.

## 15.2 Relative maturity of major audited branches

This is a **qualitative landscape classification**, not a score or leaderboard.

| Branch | Technical ecosystem | Independent/comparative evidence | Clinical/workflow evidence | Current landscape status |
|---|---|---|---|---|
| **Computational pathology** | very deep | very strong relative to other branches | emerging-to-strong in selected workflows; CRISP prospectively notable | **deep / mature FM ecosystem** |
| **CXR representation + reporting/grounding** | deep | strong and growing | mixed; independent reporting studies expose important failures | **deep / mature technical ecosystem; clinical maturity heterogeneous** |
| **Volumetric CT** | deep and rapidly expanding | meaningful independent comparative evidence | mostly retrospective, with several task-specialized clinical systems | **established / advanced** |
| **Ophthalmology/OCT** | deep specialty branch | multiple external/comparative studies | workflow-level evidence emerging | **established / advanced specialty ecosystem** |
| **MRI / neuroimaging** | broad but heterogeneous | substantial developer/external evidence; fewer unified independent leaderboards | mostly retrospective | **established and rapidly developing** |
| **3D promptable/generalist segmentation** | broad | growing; human-efficiency evidence narrower | primarily annotation/research workflow | **established technical branch** |
| **Echocardiography / ultrasound** | strong modality-specific systems | external validation meaningful for EchoPrime; operator branch smaller | prospective utility limited | **established specialty branch** |
| **Reconstruction / restoration / registration FMs** | credible multi-paper branch | task-specific comparative evidence | clinical impact evidence varies | **established methodological frontier** |
| **Dermatology** | substantial multimodal specialty FM evidence | external/reader evidence present | mostly retrospective | **established specialty branch** |
| **Mammography / MSK / endoscopy / lung-CT specialists** | strong task/modality systems | variable | variable | **established specialty/task branches** |
| **Dental/oral imaging** | several strong 2026 systems | external/human-AI evidence present but ecosystem still smaller | early | **rapidly developing specialty branch** |
| **Label-efficient / data-centric learning** | very broad cross-cutting literature | extensive review-level and task-specific evidence | workflow benefit depends on annotation setting | **established cross-cutting paradigm** |
| **Domain generalization / adaptation / OOD** | broad established technical literature | heterogeneous comparative evidence | deployment importance high | **established robustness paradigm** |
| **Continual / incremental learning** | substantial emerging literature | benchmark landscape less standardized than static learning | direct deployment evidence limited | **developing model-evolution frontier** |
| **Synthetic / generative data** | broad and rapidly expanding | evaluation standards fragmented | clinical utility/privacy evidence uneven | **established data-engineering frontier; validation immature** |
| **Quantitative imaging / radiomics / phenomics** | mature parallel ecosystem | strong retrospective breadth; reproducibility issues persist | prospective clinical adoption limited | **established quantitative-imaging branch** |
| **Acquisition QC / harmonization** | active cross-modality engineering field | evidence fragmented by modality/QC target | potentially high workflow impact | **established enabling layer; audit partial** |
| **PET/CT** | dedicated models now credible | limited but peer-reviewed comparative evidence exists | little clinical workflow evidence | **emerging credible branch** |
| **Computational microscopy restoration** | clear FM lineage | external unseen-dataset evidence | biological-truth preservation remains task-specific | **developing specialty branch** |
| **Radiotherapy FM-enabled workflows** | early but concrete | multicentre workflow evidence | semantic/interoperability rather than universal FM evidence | **emerging workflow frontier** |
| **Tool-augmented radiology** | early | limited independent evidence | early | **emerging systems frontier** |
| **Longitudinal/spatiotemporal FMs** | credible exemplars | setting-specific | limited prospective clinical evidence | **developing frontier** |
| **Federated FMs** | methodological/system-level work | bounded empirical evidence | not a privacy solution | **emerging systems direction** |
| **Medical VLM security/privacy** | rapidly growing evidence base | multiple peer-reviewed attack/privacy studies | deployment implications substantial | **established risk domain; mitigation frontier** |


## 15.3 Audit-completeness map

Field maturity and dossier completeness must be read separately.

| Area | `audit_completeness` | Consequence |
|---|---|---|
| CT/CBCT, MRI, CXR, ophthalmology, pathology | **deep** | suitable for cross-branch synthesis inside this dossier |
| ultrasound/echo, dermatology, dental, PET/CT, microscopy/spatial biology | **substantial** | major lineages mapped; targeted project work should still run a branch-specific freshness search |
| mammography, MSK, endoscopy, radiotherapy, acquisition/QC, radiomics | **partial** | enough for orientation, not enough to assume absence of competing methods |
| label-efficient learning, domain robustness, continual learning, synthetic data | **substantial at paradigm level; modality detail variable** | cross-cutting paradigm is mapped, but project design must specialize by modality/task |
| SPECT | **gap** | no field-maturity inference permitted from this dossier |

This table is a **safety mechanism for downstream ideation**. When project design begins, any candidate arising from a `partial/scaffold/gap` area requires a dedicated literature expansion before novelty or opportunity is judged.

## 15.4 What should *not* be inferred from this map

- More parameters do not imply a stronger medical-imaging FM.
- More images/slices/patches do not imply more independent patients or studies.
- More modalities do not imply stronger evidence.
- `core_fm` does not mean clinically mature.
- External validation does not automatically mean independent transportability.
- Open weights do not mean open data, permissive reuse, or independent replication.
- Prospective evidence in one specialty does not generalize to another.
- A branch being “emerging” does not make it unimportant; it means the evidence ecosystem is less mature.

The purpose of the maturity map is to orient the reader before any downstream research or product strategy is designed.

------------------------------------------------------------------------

# 16. Source Verification Ledger

This source ledger carries forward the full 9 September 2026 evidence audit and was **refreshed through 11 September 2026 for new or materially changed claims**. Sources S102–S116 and the associated changed passages were verified against primary/version-of-record or authoritative bibliographic pages during the v3.8/v4.1 hardening passes; unchanged legacy entries retain their prior audit status rather than being falsely described as re-opened on every patch. Links are included so future audits can re-verify claims rather than trust this dossier recursively. Evidence class still depends on source type; inclusion in this ledger is not endorsement of every conclusion in the source.

## Cross-field reviews / clinical-readiness framing

1.  **[S1]** Niu C, Wu P, De Man B, Wang G. **Foundation Models for Medical Imaging: Status, Challenges, and Directions.** *IEEE Transactions on Medical Imaging*. 19 Aug 2026. DOI: `10.1109/TMI.2026.3725265`.  
    PubMed: https://pubmed.ncbi.nlm.nih.gov/42616634/

2.  **[S2]** Muneer A, Zhang K, Hamdi I, et al. **Foundation models in biomedical imaging: turning hype into reality.** *Nature Biomedical Engineering*. 11 Aug 2026. DOI: `10.1038/s41551-026-01762-z`.  
    https://www.nature.com/articles/s41551-026-01762-z

## Segmentation

3.  **[S3]** Jiang C, Ding T, Song C, et al. **Medical SAM3: A Foundation Model for Universal Prompt-Driven Medical Image Segmentation.** arXiv:2601.10880, 15 Jan 2026.  
    https://arxiv.org/abs/2601.10880

4.  **[S4]** Ma J, Yang Z, Kim S, et al. **MedSAM2: Segment Anything in 3D Medical Images and Videos.** arXiv:2504.03600.  
    https://arxiv.org/abs/2504.03600

5.  **[S5]** He Y, Guo P, Tang Y, et al. **VISTA3D: A Unified Segmentation Foundation Model For 3D Medical Imaging.** CVPR 2025.  
    https://openaccess.thecvf.com/content/CVPR2025/html/He_VISTA3D_A_Unified_Segmentation_Foundation_Model_For_3D_Medical_Imaging_CVPR_2025_paper.html

6.  **[S6]** Project MONAI. **VISTA3D / NV-Segment current repository documentation.**  
    https://github.com/Project-MONAI/VISTA

7.  **[S7]** NVIDIA-Medtech. **NV-Segment-CTMR.**  
    https://github.com/NVIDIA-Medtech/NV-Segment-CTMR

8.  **[S8]** Microsoft. **BiomedParse v2 repository.**  
    https://github.com/microsoft/BiomedParse

9.  **[S9]** Isensee F, Jaeger PF, Kohl SAA, et al. **nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation.** *Nature Methods* 18, 203–211 (2021). Current v2 implementation:  
    https://github.com/MIC-DKFZ/nnUNet

## General representations / radiology

10. **[S10]** Google Health AI Developer Foundations. **MedSigLIP model card.**  
    https://developers.google.com/health-ai-developer-foundations/medsiglip/model-card

11. **[S11]** Microsoft Research. **MedImageInsight: An Open-Source Embedding Model for General Domain Medical Imaging.**  
    https://www.microsoft.com/en-us/research/publication/medimageinsight-an-open-source-embedding-model-for-general-domain-medical-imaging/

12. **[S12]** Blankemeier L, Kumar A, Cohen JP, et al. **Merlin: a computed tomography vision-language foundation model and dataset.** *Nature* 652, 1318–1328 (2026). DOI: `10.1038/s41586-026-10181-8`.  
    https://www.nature.com/articles/s41586-026-10181-8

13. **[S13]** Hamamci IE, et al. **Generalist foundation models from a multimodal dataset for 3D computed tomography.** *Nature Biomedical Engineering* (2026). DOI: `10.1038/s41551-025-01599-y`.  
    https://www.nature.com/articles/s41551-025-01599-y

14. **[S14]** Yang Z, DSouza N, Megyeri I, et al. **Decipher-MR: a vision-language foundation model for 3D MRI representations.** *npj Digital Medicine* 9, 419 (2026). DOI: `10.1038/s41746-026-02596-4`.  
    https://www.nature.com/articles/s41746-026-02596-4

15. **[S15]** Lyu Y, Harake S, Chowdury A, et al. **Learning neuroimaging models from health system-scale data.** *Nature Biomedical Engineering* 10, 1646–1658 (2026). DOI: `10.1038/s41551-025-01608-0`.  
    https://www.nature.com/articles/s41551-025-01608-0

16. **[S16]** Google. **MedGemma 1.5 model card.**  
    https://developers.google.com/health-ai-developer-foundations/medgemma/model-card

17. **[S17]** Han T, Wu R, Tian Y, et al. **CLEAR: an auditable foundation model for radiology grounded in clinical concepts.** *Nature Biomedical Engineering*. 22 Jul 2026. DOI: `10.1038/s41551-026-01741-4`.  
    https://www.nature.com/articles/s41551-026-01741-4

18. **[S18]** Bannur S, Bouzid K, Castro DC, et al. **MAIRA-2: Grounded Radiology Report Generation.** arXiv:2406.04449.  
    https://arxiv.org/abs/2406.04449  
    Model card: https://huggingface.co/microsoft/maira-2

19. **[S19]** Ranjit MP, Porya A, Joel S, et al. **CARE-X: Towards Clinically Useful Radiology VLMs with Auxiliary Supervision, Reward-Aligned Learning, and Tool-Augmented Measurement.** arXiv:2608.03890, 4 Aug 2026.  
    https://arxiv.org/abs/2608.03890

## Ultrasound

20. **[S20]** Guo X, Alsharid M, Zhao H, et al. **A visually grounded language model for fetal ultrasound understanding.** *Nature Biomedical Engineering* 10, 1629–1645 (2026). DOI: `10.1038/s41551-025-01578-3`.  
    https://www.nature.com/articles/s41551-025-01578-3

21. **[S21]** Yu H, Li Y, Zhang N, et al. **A foundation generative model for breast ultrasound image analysis.** *Nature Biomedical Engineering*. 7 Apr 2026. DOI: `10.1038/s41551-026-01639-1`.  
    https://www.nature.com/articles/s41551-026-01639-1

## Computational pathology

22. **[S22]** Xu H, et al. **A whole-slide foundation model for digital pathology from real-world data.** *Nature* 630, 181–188 (2024). DOI: `10.1038/s41586-024-07441-w`.  
    https://www.nature.com/articles/s41586-024-07441-w

23. **[S23]** Zimmermann E, et al. **Virchow2: Scaling Self-Supervised Mixed Magnification Models in Pathology.** arXiv:2408.00738.  
    https://arxiv.org/abs/2408.00738

24. **[S24]** Chen RJ, Ding T, Lu MY, et al. **Towards a general-purpose foundation model for computational pathology.** *Nature Medicine* 30, 850–862 (2024). DOI: `10.1038/s41591-024-02857-3`.  
    https://www.nature.com/articles/s41591-024-02857-3  
    UNI2-h current card: https://huggingface.co/MahmoodLab/UNI2-h

25. **[S25]** **A multimodal whole-slide foundation model for pathology (TITAN).** *Nature Medicine* (2025). DOI: `10.1038/s41591-025-03982-3`.  
    https://www.nature.com/articles/s41591-025-03982-3

26. **[S26]** Scalbert M, Saillard C, Peeters T, et al. **H-optimus-1: A foundation model for computational histopathology.** *Cancer Research* 86(8 Suppl):LB174 (AACR 2026 abstract). DOI: `10.1158/1538-7445.AM2026-LB174`.  
    https://aacrjournals.org/cancerres/article/86/8_Supplement/LB174/783174/

27. **[S27]** Vorontsov E, Shaikovski G, Casson A, et al. **End-to-end multimodal pathology foundation model with clinical dialogue (PRISM2).** *Nature Medicine*. 31 Jul 2026. DOI: `10.1038/s41591-026-04521-4`.  
    https://www.nature.com/articles/s41591-026-04521-4

28. **[S28]** Bareja R, Carrillo-Perez F, Zheng Y, et al. **A benchmark study of vision and pathology foundation models for computational pathology.** *Nature Communications* 17, **9012** (2026). DOI: `10.1038/s41467-026-76004-6`.  
    https://www.nature.com/articles/s41467-026-76004-6

29. **[S29]** Kömen J, de Jong ED, Hense J, et al. **Towards robust foundation models for digital pathology.** *Nature Communications* 17, 5218 (2026). DOI: `10.1038/s41467-026-73923-2`.  
    https://www.nature.com/articles/s41467-026-73923-2

30. **[S30]** Luo X, Xiang J, Ji Y, et al. **nnMIL: a generalizable multiple instance learning framework for computational pathology.** *Nature Biomedical Engineering*. 25 Aug 2026. DOI: `10.1038/s41551-026-01767-8`.  
    https://www.nature.com/articles/s41551-026-01767-8

31. **[S31]** Tizhoosh HR. **Rethinking foundation models in pathology.** *Nature Biomedical Engineering* 10, 1513–1518 (2026). DOI: `10.1038/s41551-026-01696-6`.  
    https://www.nature.com/articles/s41551-026-01696-6

## Spatial biology / efficiency / synthesis / reconstruction

32. **[S32]** Valanarasu JMJ, Xu H, Usuyama N, et al. **Multimodal AI generates virtual population for tumor microenvironment modeling (GigaTIME).** *Cell* 189(2), 386–400.e19 (22 Jan 2026). DOI: `10.1016/j.cell.2025.11.016`.  
    https://doi.org/10.1016/j.cell.2025.11.016

33. **[S33]** Usuyama N, et al. **GigaPath-Flash and GigaTIME-Flash: Efficient Pathology Foundation Models for Whole-Slide and Tumor Microenvironment Analysis.** arXiv:2607.18218, 20 Jul 2026.  
    https://arxiv.org/abs/2607.18218

34. **[S34]** Ge R, Liu Y, Wu Z, et al. **Imaging foundation model for universal enhancement of non-ideal measurement CT.** *Nature Communications* 17, 7155 (2026).  
    https://www.nature.com/articles/s41467-026-73943-y

35. **[S35]** NVIDIA-Medtech. **NV-Generate-CTMR / MAISI-v2 repository.**  
    https://github.com/NVIDIA-Medtech/NV-Generate-CTMR

## Infrastructure

36. **[S36]** MONAI Consortium. **MONAI 1.6.0.** PyPI release 22 Jun 2026; Apache-2.0.  
    https://pypi.org/project/monai/

37. **[S37]** Project MONAI. **MONAI Deploy.**  
    https://github.com/Project-MONAI/monai-deploy  
    https://project-monai.github.io/deploy.html

## Second-pass additions: representation, segmentation, inverse imaging, ophthalmology, clinical specialization

38. **[S38] ConceptCLIP.** Nie J, et al. **An explainable biomedical foundation model via large-scale concept-enhanced vision-language pretraining.** *Nature Biomedical Engineering*. Published 17 Aug 2026. DOI: `10.1038/s41551-026-01764-x`. https://www.nature.com/articles/s41551-026-01764-x

39. **[S39] SAT3D.** Peiris H, Wang S, Egan G, et al. **Segment any tumour: an uncertainty-aware vision foundation model for whole-body analysis.** *Nature Communications*. Published 11 Aug 2026. DOI: `10.1038/s41467-026-76531-2`. https://www.nature.com/articles/s41467-026-76531-2

40. **[S40] Triad.** Wang S, Safari M, Li Q, et al. **Vision foundation model for 3D magnetic resonance imaging segmentation, classification, and registration.** *Medical Image Analysis* 110, 103992 (2026). DOI: `10.1016/j.media.2026.103992`. https://pubmed.ncbi.nlm.nih.gov/41702178/

41. **[S41] DeepSparse.** Lin Y, Chen J, Wang H, et al. **DeepSparse: A Foundation Model for Sparse-View CBCT Reconstruction.** *IEEE Transactions on Medical Imaging* 45(6), 3339–3351 (2026). DOI: `10.1109/TMI.2026.3674948`. https://pubmed.ncbi.nlm.nih.gov/41843524/

42. **[S42] Fast-MRI semantic FM prior.** Feng R, He X, Mercer R, Stewart Z, Liu F. **On the Utility of Foundation Models for Fast MRI: Vision-Language-Guided Image Reconstruction.** *Magnetic Resonance in Medicine* 96(2), 943–959 (2026). DOI: `10.1002/mrm.70374`. https://onlinelibrary.wiley.com/doi/10.1002/mrm.70374

43. **[S43] Foundational registration / LUMIR.** Chen J, Wei S, Honkamaa J, et al. **Beyond the LUMIR challenge: The pathway to foundational registration models.** *Medical Image Analysis* 113, 104175 (Sep 2026). DOI: `10.1016/j.media.2026.104175`. https://pubmed.ncbi.nlm.nih.gov/42361771/

44. **[S44] uniGradICON.** Tian L, Greer H, Kwitt R, et al. **uniGradICON: A Foundation Model for Medical Image Registration.** MICCAI 2024, LNCS 15002, 749–760. DOI: `10.1007/978-3-031-72069-7_70`. https://pmc.ncbi.nlm.nih.gov/articles/PMC13242914/

45. **[S45] OCTCube-M.** Liu Z, Xu H, Woicik A, et al. **A three-dimensional multi-modal foundation model for optical coherence tomography.** *Nature Biomedical Engineering*. Published 24 Apr 2026. DOI: `10.1038/s41551-026-01662-2`. https://www.nature.com/articles/s41551-026-01662-2

46. **[S46] Volumetric OCT benchmark.** Judkiewicz R, Berkowitz E, Meisel M, et al. **Shifting the retinal foundation models paradigm from slices to volumes for optical coherence tomography.** *npj Digital Medicine* 9, 314 (2026). DOI: `10.1038/s41746-026-02496-7`. https://www.nature.com/articles/s41746-026-02496-7

47. **[S47] September pathology model-selection benchmark.** Maleki D, Shaikh N, Li X, et al. **Navigating foundation model selection in digital pathology through performance evaluation and tradeoff analysis.** *Scientific Reports*. Published 2 Sep 2026. DOI: `10.1038/s41598-026-69731-9`. https://www.nature.com/articles/s41598-026-69731-9

48. **[S48] AbdomenNet.** Zhu C, Zhang R, Song X, et al. **A foundation model for acute abdomen diagnosis stratification and triage on noncontrast computed tomography.** *Nature Communications*. Published 12 Aug 2026. DOI: `10.1038/s41467-026-76634-w`. https://www.nature.com/articles/s41467-026-76634-w

49. **[S49] OMAFound.** Liang Z, Niu Q, Wang J, et al. **A foundation model for breast and lung cancer screening using non-contrast computed tomography.** *Nature Health* 1, 403–415 (2026). DOI: `10.1038/s44360-026-00055-8`. https://doi.org/10.1038/s44360-026-00055-8

50. **[S50] Mammography multiview image-report FM.** Gao Y, Zhou H-Y, Wang X, et al. **Visualizing Radiologic Connections: An Explainable Coarse-to-Fine Foundation Model with Multiview Mammograms and Associated Reports.** *Radiology: Artificial Intelligence* 8(1):e240646 (2026). DOI: `10.1148/ryai.240646`. https://pubmed.ncbi.nlm.nih.gov/41405429/

51. **[S51] GastroNet-5M.** **GastroNet-5M: A Multicenter Dataset for Developing Foundation Models in Gastrointestinal Endoscopy.** *Gastroenterology* 170(1), 174–187 (Jan 2026). https://www.sciencedirect.com/science/article/pii/S001650852505797X

52. **[S52] Brainfound (Patterns).** Zhang G, Gao Z, Duan C, et al. **A multi-modal foundation model for brain disease diagnosis and medical imaging.** *Patterns* 7(6), 101538 (2026). DOI: `10.1016/j.patter.2026.101538`. https://pubmed.ncbi.nlm.nih.gov/42328202/

53. **[S53] RAD-DINO.** Pérez-García F, Sharma H, Bond-Taylor S, et al. **Exploring scalable medical image encoders beyond text supervision.** *Nature Machine Intelligence* 7, 119–130 (2025). DOI: `10.1038/s42256-024-00965-w`. Official model card/release also verified. https://doi.org/10.1038/s42256-024-00965-w https://huggingface.co/microsoft/rad-dino

54. **[S54] RadFM.** **Towards generalist foundation model for radiology by leveraging web-scale 2D&3D medical data.** *Nature Communications* (2025). https://www.nature.com/articles/s41467-025-62385-7

55. **[S55] Ophthalmic FM review.** Gharbi K, van Wijngaarden P, Hadoux X. **Foundation models for ophthalmic imaging.** *Survey of Ophthalmology* 71(4), 1129–1147 (2026). DOI: `10.1016/j.survophthal.2026.01.004`. https://pubmed.ncbi.nlm.nih.gov/41619947/

56. **[S56] BrainFound (npj Imaging; distinct from Brainfound above).** Mazher M, Parker GJM, Alexander DC. **Towards generalisable foundation models for brain MRI.** *npj Imaging*. Published 29 May 2026. https://www.nature.com/articles/s44303-026-00176-5

### Temporal-status source

57. **[S57] MICCAI 2026 official dates.** MICCAI Society: Strasbourg, **27 Sep–1 Oct 2026**. As of this dossier snapshot (11 Sep 2026), the conference has not yet occurred. https://miccai.org/upcoming-conferences/

### Third-pass additions: health-system learning, spatiotemporal imaging, spatial proteomics, workflow specialization, and privacy

58. **[S58] NeuroVFM / health-system learning.** Kondepudi A, Rao A, Zhao C, et al. **Health system learning enables generalist neuroimaging models.** *Nature Medicine* 32, 2831–2837 (2026). DOI: `10.1038/s41591-026-04497-1`. https://pubmed.ncbi.nlm.nih.gov/42432292/

59. **[S59] NeuroSTORM.** Wang C, Jiang Y, Peng Z, et al. **Towards a general-purpose foundation model for functional MRI analysis.** *Nature Biomedical Engineering*. Published 23 Apr 2026. DOI: `10.1038/s41551-026-01666-y`. https://pubmed.ncbi.nlm.nih.gov/42026120/

60. **[S60] VirTues / Virtual Tissues.** Wenckstern J, Jain E, von Querfurth B, et al. **The Virtual Tissues foundation model resolves spatial proteomics across scales.** *Nature*. Published 5 Aug 2026. DOI: `10.1038/s41586-026-10884-y`. https://pubmed.ncbi.nlm.nih.gov/42557331/

61. **[S61] RETFound Plus.** Wang Z, Zhou Y, Wu Y, et al. **Time and person sensitive foundation model for disease prediction and risk stratification.** *npj Digital Medicine* 9, 444 (2026). DOI: `10.1038/s41746-026-02524-6`. https://www.nature.com/articles/s41746-026-02524-6

62. **[S62] FOCUS.** Zhang J, Zhong J, Lin L, et al. **Full end-to-end diagnostic workflow automation of 3D OCT via foundation model-driven AI for retinal diseases.** *npj Digital Medicine*. Published 19 Aug 2026. DOI: `10.1038/s41746-026-03151-x`. https://www.nature.com/articles/s41746-026-03151-x

63. **[S63] SKELEX.** Kim S, Lee S, Shin K, et al. **A large-scale vision foundation model for musculoskeletal radiographs.** *npj Digital Medicine* 9, 668 (2026). DOI: `10.1038/s41746-026-02826-9`. https://pubmed.ncbi.nlm.nih.gov/42230902/

64. **[S64] LCTfound.** Gao Z, Zhang G, Liang H, et al. **A lung CT vision foundation model facilitating disease diagnosis and medical imaging.** *Nature Communications* 17, 35 (2026). DOI: `10.1038/s41467-025-66620-z`. https://www.nature.com/articles/s41467-025-66620-z

65. **[S65] Patient re-identification evidence.** Nebbia G, Kumar S, McNamara SM, et al. **Re-identification of patients from imaging features extracted by foundation models.** *npj Digital Medicine* 8, 469 (2025). DOI: `10.1038/s41746-025-01801-0`. https://www.nature.com/articles/s41746-025-01801-0

66. **[S66] Re-identification methodological challenge.** Engelmann J, Wagner SK, Zhou Y, et al. **Matters Arising: Near-identical images, not foundation models, explain purported re-identification of patients from medical imaging.** *npj Digital Medicine* 9, 159 (2026). DOI: `10.1038/s41746-026-02440-9`. https://www.nature.com/articles/s41746-026-02440-9

### Final freshness sweep: clinical competence, lifecycle evaluation, and distributed learning

67. **[S67] Real-case clinical-competency benchmark.** Wang D, Yuan Z, Liu J, et al. **The illusion of clinical reasoning: a benchmark reveals the pervasive gap in vision-language models for clinical competency.** *npj Digital Medicine*. Published 3 Sep 2026. DOI: `10.1038/s41746-026-03191-3`. https://www.nature.com/articles/s41746-026-03191-3

68. **[S68] Five-phase medical-AI evaluation framework.** Ye Z, Chen Y, Huang X, et al. **A five-phase evaluation framework for diagnostic and predictive medical artificial intelligence.** *npj Digital Medicine* 9, 678 (2026). Published 3 Sep 2026; version of record 4 Sep 2026. DOI: `10.1038/s41746-026-03155-7`. https://www.nature.com/articles/s41746-026-03155-7

69. **[S69] Federated foundation-model perspective.** Chakrabarty K, Hosseinalipour S, Ying L. **A perspective on federated foundation models in biomedical sensing and imaging.** *npj Digital Medicine*. Published 1 Sep 2026. DOI: `10.1038/s41746-026-03164-6`. https://www.nature.com/articles/s41746-026-03164-6

### Evidence-methodology and newly promoted modality/workflow sources

70. **[S70] CLAIM 2024.** Tejani AS, Klontzas ME, Gatti AA, et al. **Checklist for Artificial Intelligence in Medical Imaging (CLAIM): 2024 Update.** *Radiology: Artificial Intelligence* 6(4):e240300 (2024). DOI: `10.1148/ryai.240300`. https://doi.org/10.1148/ryai.240300

71. **[S71] STARD-AI.** Sounderajah V, Guni A, Liu X, et al. **The STARD-AI reporting guideline for diagnostic accuracy studies using artificial intelligence.** *Nature Medicine* 31, 3283–3289 (2025). DOI: `10.1038/s41591-025-03953-8`. https://www.nature.com/articles/s41591-025-03953-8

72. **[S72] DECIDE-AI.** Vasey B, Nagendran M, Campbell B, et al. **Reporting guideline for the early-stage clinical evaluation of decision support systems driven by artificial intelligence: DECIDE-AI.** *Nature Medicine* 28, 924–933 (2022). DOI: `10.1038/s41591-022-01772-9`. https://www.nature.com/articles/s41591-022-01772-9

73. **[S73] FUTURE-AI.** Lekadir K, Frangi AF, Porras AR, et al. **FUTURE-AI: international consensus guideline for trustworthy and deployable artificial intelligence in healthcare.** *BMJ* 388:e081554 (2025). DOI: `10.1136/bmj-2024-081554`. https://www.bmj.com/content/388/bmj-2024-081554

74. **[S74] TRIPOD+AI.** Collins GS, Moons KGM, Dhiman P, et al. **TRIPOD+AI statement: updated guidance for reporting clinical prediction models that use regression or machine learning methods.** *BMJ* 385:e078378 (2024). DOI: `10.1136/bmj-2023-078378`. https://www.bmj.com/content/385/bmj-2023-078378

75. **[S75] CONSORT-AI.** Liu X, Cruz Rivera S, Moher D, et al. **Reporting guidelines for clinical trial reports for interventions involving artificial intelligence: the CONSORT-AI extension.** *Nature Medicine* 26, 1364–1374 (2020). DOI: `10.1038/s41591-020-1034-x`. https://www.nature.com/articles/s41591-020-1034-x

76. **[S76] Open multicentre FDG PET/CT FM.** Liu X, Zhang Q, Marin T, et al. **An Open Multi-Center Whole-Body FDG PET/CT Foundation Model for Tumor Segmentation.** arXiv:2605.21835, 20 May 2026. **Preprint; not peer reviewed at snapshot date.** https://arxiv.org/abs/2605.21835

77. **[S77] Dual-Stream DINOv3 PET/CT.** Lin Y-N Scarlett, Wang S, Safari M, Yang X. **Beyond Natural Images: A Dual-Stream DINOv3 Framework for PET/CT Segmentation.** *Proceedings of the 9th International Conference on Medical Imaging with Deep Learning (MIDL 2026)*, PMLR 315:2780–2794 (2026). Peer-reviewed conference paper. https://proceedings.mlr.press/v315/lin26b.html

78. **[S78] Radiotherapy ROI interoperability.** Yang D, Lei M, Yang Q, et al. **Foundation Model-Driven Regions of Interest Classification and Renaming in Cancer Radiotherapy: A Customizable, Retraining-Free Workflow Across Institutions.** *JCO Clinical Cancer Informatics* 10:e2500380 (2026). DOI: `10.1200/CCI-25-00380`. https://ascopubs.org/doi/10.1200/CCI-25-00380

79. **[S79] PROBAST+AI.** Moons KGM, Damen JAA, Kaul T, et al. **PROBAST+AI: an updated quality, risk of bias, and applicability assessment tool for prediction models using regression or artificial intelligence methods.** *BMJ* 388:e082505 (2025). DOI: `10.1136/bmj-2024-082505`. https://www.bmj.com/content/388/bmj-2024-082505

80. **[S80] SPIRIT-AI.** Cruz Rivera S, Liu X, Chan A-W, et al. **Guidelines for clinical trial protocols for interventions involving artificial intelligence: the SPIRIT-AI extension.** *Nature Medicine* 26, 1351–1363 (2020). DOI: `10.1038/s41591-020-1037-7`. https://www.nature.com/articles/s41591-020-1037-7

81. **[S81] Independent CT FM comparison.** Tagscherer J, de Boer S, van der Graaf F, et al. **Systematic evaluation of foundation models for organ-level classification on CT scans.** *International Journal of Computer Assisted Radiology and Surgery*. Published 27 Aug 2026. DOI: `10.1007/s11548-026-03786-x`. https://link.springer.com/article/10.1007/s11548-026-03786-x

82. **[S82] Independent pathology FM benchmark.** Neidlinger P, et al. **Benchmarking foundation models as feature extractors for weakly supervised computational pathology.** *Nature Biomedical Engineering*. Published 1 Oct 2025. DOI: `10.1038/s41551-025-01516-3`. https://www.nature.com/articles/s41551-025-01516-3

83. **[S83] EchoPrime.** Vukadinovic M, Chiu I-M, Tang X, et al. **Comprehensive echocardiogram evaluation with view primed vision language AI.** *Nature* 650, 970–977 (2026). Published online 11 Nov 2025; version of record 23 Dec 2025; issue date 26 Feb 2026. DOI: `10.1038/s41586-025-09850-x`. Training: 12,124,168 videos from 275,442 studies / 108,913 patients at Cedars-Sinai; internal plus four external health-system evaluations; code/weights/demo released. https://www.nature.com/articles/s41586-025-09850-x

84. **[S84] EchoCLIP.** Christensen M, Vukadinovic M, Yuan N, et al. **Vision–language foundation model for echocardiogram interpretation.** *Nature Medicine* 30, 1481–1488 (2024). DOI: `10.1038/s41591-024-02959-y`. Training: 1,032,975 video-text pairs from 224,685 studies / 99,870 patients. https://www.nature.com/articles/s41591-024-02959-y

85. **[S85] MammoScope.** Sadée C, Lin C, Raymond D, et al. **Mammoscope: a clinically-informed foundation model for high-resolution mammography interpretation.** *Clinical Cancer Research* 32(4 Suppl):PS3-06-30 (2026), SABCS 2025 abstract. DOI: `10.1158/1557-3265.SABCS25-PS3-06-30`. Reports pretraining on >650,000 mammograms from 14 public datasets. **Abstract-level evidence.** https://aacrjournals.org/clincancerres/article/32/4_Supplement/PS3-06-30/773514

86. **[S86] Generic DINOv3 medical benchmark.** Liu C, Chen Y, Shi H, et al. **Does DINOv3 Set a New Medical Vision Standard? Benchmarking 2D and 3D Classification, Segmentation, and Registration.** arXiv:2509.06467 **v3**, submitted 8 Sep 2025; last revised **17 Jan 2026**. Technical report; reports strong generic transfer on multiple medical tasks but degradation in highly specialized WSI, electron-microscopy and PET settings. **Preprint/technical-report evidence.** https://arxiv.org/abs/2509.06467v3

87. **[S87] Echo-Vision-FM.** Zhang Z, Wu Q, Ding S, et al. **Echo-Vision-FM: a pre-training and fine-tuning framework for echocardiogram video vision foundation model.** *Nature Communications* 17, 19 (2026); published online 11 Dec 2025; publisher correction 20 Jan 2026. DOI: `10.1038/s41467-025-66340-4`. https://www.nature.com/articles/s41467-025-66340-4

88. **[S88] PanDerm.** Yan S, Yu Z, Primiero C, et al. **A multimodal vision foundation model for clinical dermatology.** *Nature Medicine* 31, 2691–2702 (2025). Published online 6 Jun 2025. DOI: `10.1038/s41591-025-03747-y`. Pretraining: 2,149,706 skin images from 11 sources across total-body photography, dermatopathology, clinical photography and dermoscopy; evaluated on 28 benchmarks; encoder code/weights released. https://www.nature.com/articles/s41591-025-03747-y

89. **[S89] FluoResFM.** Lu Q, Liu X, Feng Q, Zeng S, Cheng S. **A foundation model for multi-task cross-distribution restoration of fluorescence microscopy images.** *Nature Communications* 17, 3729 (2026). Published 10 Mar 2026. DOI: `10.1038/s41467-026-70307-4`. Trained on 4,303,086 paired patches across three restoration tasks and >20 biological structures; evaluated on 302 internal and 51 unseen external datasets. https://www.nature.com/articles/s41467-026-70307-4

90. **[S90] CARE.** Zhang D, Gong Z, Pang X, et al. **CARE: A Molecular-Guided Foundation Model with Adaptive Region Modeling for Whole Slide Image Analysis.** *CVPR 2026*, pp. 21078–21088. Peer-reviewed conference paper. Pretraining uses 34,277 WSIs with self-supervised morphology learning followed by RNA/protein-guided alignment; evaluated across 33 downstream benchmarks. https://openaccess.thecvf.com/content/CVPR2026/html/Zhang_CARE_A_Molecular-Guided_Foundation_Model_with_Adaptive_Region_Modeling_for_CVPR_2026_paper.html

91. **[S91] XGeM.** Molino D, Di Feola F, Faiella E, et al. **XGeM: A multi-prompt foundation model for multimodal medical data generation.** *Computerized Medical Imaging and Graphics* 128, 102718 (2026). Epub 30 Jan 2026. DOI: `10.1016/j.compmedimag.2026.102718`. 6.77B multimodal generative model; published validation is heavily MIMIC-CXR/report centered despite broad any-to-any architecture. https://pubmed.ncbi.nlm.nih.gov/41637851/


### v3.7 precision/completeness additions: dental imaging, privacy/security, shortcut robustness, microscopy lineage and federated systems

92. **[S92] PanoFM.** Han X, Wu Z, Wang Z, Dong S, Zhang R, Liu J, Zhang L. **PanoFM: An LLM-empowered panoramic foundation model with clinical semantic integration for comprehensive dental disease diagnosis.** *Pattern Recognition* 180, 114411 (2026). DOI: `10.1016/j.patcog.2026.114411`. Trained/validated on 127,878 panoramic radiographs from six geographically diverse clinical centres in China; paper explicitly distinguishes the pretrained panoramic foundation encoder from the downstream LLM/contextual diagnostic system. https://www.sciencedirect.com/science/article/pii/S0031320326013762

93. **[S93] DentFound.** Zhu Q, Lin Y, Fu W, et al. **Towards clinical-level interpretation of dental panoramic radiography using an instance-guided vision-language model.** *Nature Biomedical Engineering* (2026). Published 25 Jun 2026. DOI: `10.1038/s41551-026-01713-8`. Dataset: >101,000 patients, ages 2–98, 98 diseases and 11 post-treatment categories; multicentre evaluation and expert review by 12 dentists/radiologists. **Author Correction published 8 Jul 2026**, DOI `10.1038/s41551-026-01759-8`; use the corrected current article. https://www.nature.com/articles/s41551-026-01713-8

94. **[S94] DentVLM.** Meng Z, Hao J, Dai X, et al. **A multimodal vision-language model for comprehensive dental diagnosis and enhanced clinical practice.** *Nature Communications* 17, 8933 (2026). Published 22 Jul 2026; version of record 25 Aug 2026. DOI: `10.1038/s41467-026-75718-x`. Training: 20,741 patients, 110,447 images and 2,458,084 bilingual VQA pairs across seven dental imaging modalities; 36 tasks; 32-participant human–AI evaluation. Based on Qwen2-VL-7B. https://www.nature.com/articles/s41467-026-75718-x

95. **[S95] Patient-level membership-inference privacy audit.** Knolle MA, Menten MJ, Jungmann F, et al. **Disparate privacy risks from medical AI.** *Nature* 656, 192–198 (2026). Published 24 Jun 2026. DOI: `10.1038/s41586-026-10688-0`. Patient-level membership-inference audit across seven real-world medical datasets; demonstrates extreme individual vulnerability can be hidden by aggregate attack metrics and that risk can rise with model capacity and be unequally distributed across subgroups. https://www.nature.com/articles/s41586-026-10688-0

96. **[S96] Medical VLM prompt injection — oncology.** Clusmann J, Ferber D, Wiest IC, et al. **Prompt injection attacks on vision language models in oncology.** *Nature Communications* 16, 1239 (2025). Published 1 Feb 2025. DOI: `10.1038/s41467-024-55631-x`. Quantitative study of 594 prompt-injection attacks across four VLMs using sub-visual prompts embedded in medical images. https://www.nature.com/articles/s41467-024-55631-x

97. **[S97] Surgical-video prompt injection.** Zhang Z, Qadir MI, Carstens M, et al. **Prompt injection attacks on vision-language models for surgical decision support.** *npj Digital Surgery* 1, 15 (2026). Published 27 Jul 2026. DOI: `10.1038/s44484-026-00014-6`. Evaluated textual and temporally varying visual attacks across 100 surgical video clips and eight decision-support tasks. https://www.nature.com/articles/s44484-026-00014-6

98. **[S98] Transferable medical-VLM adversarial attack.** Ghosh A, Baidya S, Saha S, Chen X. **When Background Matters: Breaking Medical Vision Language Models by Transferable Attack.** *Proceedings of ACL 2026*, pp. 38143–38170. DOI: `10.18653/v1/2026.acl-long.1768`. MedFocusLeak evaluates transferable black-box imperceptible attacks concentrated in non-diagnostic background regions across six medical imaging modalities. https://aclanthology.org/2026.acl-long.1768/

99. **[S99] Cross-dataset mammography shortcut study.** Germani E, Zeineddine F, Mourad C, Albarqouni S. **When multi-institutional dataset aggregation masks shortcut-like behavior in Foundation Models for medical imaging.** *Medical Image Analysis* 114, 104258 (2026). Published online 22 Aug 2026. DOI: `10.1016/j.media.2026.104258`. Studies FM feature extractors across ten heterogeneous mammography datasets; dataset identity is readily recoverable and aggregated classifiers retain dataset-dependent error/confidence/calibration failures. https://www.sciencedirect.com/science/article/pii/S1361841526003270

100. **[S100] UniFMIR.** Ma C, Tan W, He R, Yan B. **Pretraining a foundation model for generalizable fluorescence microscopy-based image restoration.** *Nature Methods* 21, 1558–1567 (2024). Published 12 Apr 2024. DOI: `10.1038/s41592-024-02244-3`. Demonstrated across five fluorescence-restoration tasks and 14 datasets; code/models released. https://www.nature.com/articles/s41592-024-02244-3

101. **[S101] Empirical federated-FM embedding study.** Lohmann JJG, Witte A, Maier A, Saak CC, Sauter G, Zimmermann M, Bonn S, Baumbach J. **On the power and limits of foundation model image embeddings for privacy-preserving federated learning.** *Array* 29, 100725 (2026). DOI: `10.1016/j.array.2026.100725`. Empirical medical-imaging study reporting >90% reduction in trainable federated model size when using FM embeddings, with estimated communication-overhead reductions and secure multiparty computation; does not establish complete privacy protection. https://www.sciencedirect.com/science/article/pii/S2590005626000482

102. **[S102] Independent CXR report-generation comparison / MAIRA-2 counter-evidence.** Lim WH, et al. **Comparative evaluation of generative AI models for chest radiograph report generation in the emergency department.** *European Radiology* (2026). DOI: `10.1007/s00330-026-12648-8`. Retrospective 478-patient tertiary-centre ED cohort with blinded randomized review by three thoracic radiologists and same-day CT reference for finding-level analyses; MAIRA-2 showed higher RADPEER-3b disagreement, lower clinical acceptability and substantially more hallucinations than radiologist-written reports in this cohort. https://link.springer.com/article/10.1007/s00330-026-12648-8

103. **[S103] PathSegmentor.** Chen Z, Hou J, Lin L, et al. **Segment anything in pathology images with natural language.** *Nature Computational Science*. Published 10 Sep 2026. DOI: `10.1038/s43588-026-01042-5`. Introduces PathSeg (21 public datasets; 275,200 image–mask–label triples) and a natural-language-guided pathology segmentation foundation model spanning 160 pathological categories with internal and external public/clinical evaluation. https://www.nature.com/articles/s43588-026-01042-5

104. **[S104] CRISP.** Zhao Z, Zhou F, Li R, et al. **A clinically-oriented foundation model for intraoperative pathology.** *Nature Medicine*. Published 10 Sep 2026. DOI: `10.1038/s41591-026-04703-0`. Frozen-section pathology FM developed on >100,000 frozen sections from ten medical centres; evaluated retrospectively across >15,000 intraoperative slides/nearly 100 tasks and prospectively in >3,000 patients with reported human–AI workflow effects. https://www.nature.com/articles/s41591-026-04703-0

105. **[S105] HorusEye.** Chu Y, Zhou L, Luo G, et al. **HorusEye: a self-supervised foundation model for generalizable X-ray tomography restoration.** *Nature Computational Science* 6, 372–387 (2026). Published 27 Mar 2026. DOI: `10.1038/s43588-026-00973-3`. Self-supervised X-ray tomography restoration FM trained on >100 million images and evaluated across multiple restoration tasks/modalities, including unseen modalities and clinical/downstream analyses; official source code is released. https://www.nature.com/articles/s43588-026-00973-3

### v4.1 ground-layer additions: recall, non-FM paradigms, CXR supervision/phenomics, QC and quantitative imaging

106. **[S106] Ark.** Ma D, Pang J, Gotway MB, Liang J. **A fully open AI foundation model applied to chest radiography.** *Nature* 643, 488–498 (2025). Published 11 Jun 2025. DOI: `10.1038/s41586-025-09079-8`. Open CXR FM using knowledge accrual across heterogeneous expert-labelled datasets; code and pretrained models released. https://www.nature.com/articles/s41586-025-09079-8

107. **[S107] Ark+.** Ma D, Pang J, Senthil Velan S, Gotway MB, Liang J. **Ark+: Supervised training a single high-performance AI foundation model from many differently labeled datasets—no label consolidation required.** *Medical Image Analysis* 108, 103828 (2026). DOI: `10.1016/j.media.2025.103828`. Supervised heterogeneous-label aggregation across multiple public CXR datasets; includes classification, segmentation/localization, bias/long-tail and federated-learning analyses. https://www.sciencedirect.com/science/article/pii/S1361841525003743

108. **[S108] Label-efficient medical image analysis survey.** Jin C, Guo Z, Lin Y, Luo L, Chen H. **Learning with less supervision: A survey of label-efficient learning for medical image analysis.** *Medical Image Analysis* 111, 104062 (2026). DOI: `10.1016/j.media.2026.104062`. Reviews 350+ studies across major low/weak-label learning regimes and situates health FMs inside the broader supervision landscape. https://www.sciencedirect.com/science/article/pii/S1361841526001301

109. **[S109] Domain-generalization review.** Yoon JS, Oh K, Shin Y, Mazurowski MA, Suk HI. **Domain Generalization for Medical Image Analysis: A Review.** *Proceedings of the IEEE* 112(10), 1583–1609 (2024). DOI: `10.1109/JPROC.2024.3507831`. Organizes medical-imaging DG into data-, feature-, model- and analysis-level approaches and frames domain shift across the imaging workflow. https://ieeexplore.ieee.org/document/10780969/

110. **[S110] Continual-learning review.** Kumari P, Chauhan J, Bozorgpour A, Huang B, Azad R, Merhof D. **Continual learning in medical image analysis: A comprehensive review of recent advancements and future prospects.** *Medical Image Analysis* 106, 103730 (2025). DOI: `10.1016/j.media.2025.103730`. Reviews task-, class- and domain-incremental medical-imaging learning, catastrophic forgetting, drift, evaluation and deployment-relevant constraints. https://www.sciencedirect.com/science/article/pii/S1361841525002774

111. **[S111] Synthetic medical-image data systematic review.** Fu X, Li X, Delamare E, Dunn AG, Bi L, Kim J. **A systematic review of generative artificial intelligence techniques for synthetic medical image datasets: Quality, models, public availability and applications.** *Computer Methods and Programs in Biomedicine* 280, 109331 (2026). DOI: `10.1016/j.cmpb.2026.109331`. Maps public synthetic datasets/checkpoints across medical-imaging domains and highlights lack of standardized evaluation. https://pubmed.ncbi.nlm.nih.gov/41895216/

112. **[S112] CXR disease phenomics.** Lin C, Chen K-C, Huang J-W, et al. **Transparent chest radiograph foundation model enables explainable human disease profiling.** *npj Digital Medicine* 9, 674 (2026). Published 16 Jul 2026; version of record 2 Sep 2026. DOI: `10.1038/s41746-026-02990-y`. Evaluates 1,074 EHR-derived phecodes with frozen CXR-FM embeddings across three independent validation cohorts and relates disease predictions to radiologist-curated imaging features. https://www.nature.com/articles/s41746-026-02990-y

113. **[S113] Biomedical-FM benchmarking perspective.** Saez-Rodriguez J, Schäfer PSL, Kalavros N, Stolovitzky G. **Benchmarking biomedical foundation models.** *Nature Methods* 23, 1724–1733 (2026). Published 4 Sep 2026. DOI: `10.1038/s41592-026-03182-y`. Perspective on reproducibility, generalization, replicability, falsifiability/utility and community benchmark design for biomedical FMs. https://www.nature.com/articles/s41592-026-03182-y

114. **[S114] Medical-imaging FM systematic review / recall backstop.** Rajendran P, Safari M, He W, Hu M, Shah K, Wang S, Zhou J, Yang X. **Foundation models in medical image analysis: A systematic review and quantitative analysis.** *Medical Image Analysis* 115, 104293 (issue metadata Jan 2027; article available in 2026). DOI: `10.1016/j.media.2026.104293`. Structured synthesis of **237 studies** across vision-only and vision-language FMs; used here as a coverage-recall backstop, not as a substitute for primary-source verification. https://www.sciencedirect.com/science/article/pii/S1361841526003622

115. **[S115] Medical image quality assessment review.** Herath HMSS, Herath HMKKMB, Madusanka N, Lee B-I. **A Systematic Review of Medical Image Quality Assessment.** *Journal of Imaging* 11(4), 100 (2025). DOI: `10.3390/jimaging11040100`. Reviews 42 MIQA studies spanning subjective/objective quality assessment and AI/ML automation; useful as a field-level anchor for QC, artifact and standardization issues. https://pubmed.ncbi.nlm.nih.gov/40278016/

116. **[S116] Quantitative imaging / radiomics review.** Linton-Reid K, Chen M, Martell MB, Posma JM, Aboagye EO. **Radiomics in clinical radiology: advances, challenges, and future directions.** *Clinical Radiology* 92, 107165 (2026). DOI: `10.1016/j.crad.2025.107165`. Reviews radiomics methodology, validation, standardization, multicentre issues and emerging AI/LLM/agentic integration, while emphasizing reproducibility and clinical-translation barriers. https://doi.org/10.1016/j.crad.2025.107165


------------------------------------------------------------------------

# 17. Dataset Verification Ledger and Link Audit

The dataset claims introduced in Section 3A were independently re-checked on 9 September 2026. **Dataset cards are cited for live access/version facts; peer-reviewed papers/data descriptors are preferred for scientific claims.** A live card can legitimately differ from the version of record; both must be labelled rather than silently reconciled.

## 17.1 Dataset sources

1.  **[D1]** Ma J et al. / challenge organizers. **CVPR 2025–2026: Foundation Models for Text-guided 3D Biomedical Image Segmentation.** Official Codabench challenge page; states >200,000 3D image-mask pairs and modality coverage.  
    https://www.codabench.org/competitions/5651/

2.  **[D2]** Official **CVPR-BiomedSegFM** Hugging Face dataset card. License metadata: CC-BY-NC-SA-4.0.  
    https://huggingface.co/datasets/junma/CVPR-BiomedSegFM

3.  **[D3]** FLARE-MedFM. **PanCancerCTSeg — MICCAI 2026 Challenge dataset.** Official Hugging Face dataset card; >17,000 labelled cancer CT scans with per-source licences.  
    https://huggingface.co/datasets/FLARE-MedFM/PancancerCTSeg

4.  **[D4]** Li W, Qu C, Chen X, et al. **AbdomenAtlas: A large-scale, detailed-annotated, & multi-center dataset for efficient transfer learning and open algorithmic benchmarking.** *Medical Image Analysis* 97:103285 (2024). DOI: `10.1016/j.media.2024.103285`.  
    https://pubmed.ncbi.nlm.nih.gov/39116766/

5.  **[D5]** Bassi PRAS, Yavuz MC, Hamamci IE, et al. **RadGPT: Constructing 3D Image-Text Tumor Datasets.** ICCV 2025. Introduces AbdomenAtlas 3.0 and its mask-assisted/radiologist-reviewed report creation.  
    https://openaccess.thecvf.com/content/ICCV2025/html/Bassi_RadGPT_Constructing_3D_Image-Text_Tumor_Datasets_ICCV_2025_paper.html

6.  **[D6]** Wasserthal J, Breit HC, Meyer MT, et al. **TotalSegmentator: Robust Segmentation of 104 Anatomic Structures in CT Images.** *Radiology: Artificial Intelligence* 5:e230024 (2023).  
    https://pubmed.ncbi.nlm.nih.gov/37795137/

7.  **[D7]** Antonelli M, Reinke A, Bakas S, et al. **The Medical Segmentation Decathlon.** *Nature Communications* 13:4128 (2022).  
    https://www.nature.com/articles/s41467-022-30695-9

8.  **[D8]** **LUNA16 — Data.** Official Grand Challenge page.  
    https://luna16.grand-challenge.org/Data/

9.  **[D9]** **KiTS23 — The 2023 Kidney Tumor Segmentation Challenge.** Official challenge page.  
    https://kits-challenge.org/kits23/

10. **[D10]** **AMOS22 — Multi-Modality Abdominal Multi-Organ Segmentation Challenge.** Official Grand Challenge page.  
    https://amos22.grand-challenge.org/Home/

11. **[D11A]** **autoPET — Dataset.** Official Grand Challenge page.  
    https://autopet.grand-challenge.org/Dataset/

12. **[D12]** Johnson AEW, Pollard TJ, Berkowitz SJ, et al. **MIMIC-CXR Database v2.1.0.** PhysioNet.  
    https://physionet.org/content/mimic-cxr/2.1.0/

13. **[D13]** Stanford AIMI. **CheXpert: Chest X-rays.** Canonical dataset page.  
    https://aimi.stanford.edu/datasets/chexpert-chest-x-rays

14. **[D14]** Bustos A, Pertusa A, Salinas JM, de la Iglesia-Vayá M. **PadChest: A large chest x-ray image dataset with multi-label annotated reports.** *Medical Image Analysis* 66:101797 (2020).  
    https://pubmed.ncbi.nlm.nih.gov/32877839/

15. **[D15]** NIH Clinical Center. **NIH ChestX-ray14 expanded release.** The canonical NIH repository hosts the expanded 14-label release used in later work (112,120 frontal images / 30,805 patients). The original CVPR 2017 **ChestX-ray8** paper reported the earlier 108,948-image / 32,717-patient release and must not be used as the source for the expanded-release counts.  
    https://nihcc.app.box.com/v/ChestXray-NIHCC

16. **[D16]** Nguyen HQ, Lam K, Le LT, et al. **VinDr-CXR v1.0.0.** PhysioNet; 18,000 released CXRs with radiologist annotations.  
    https://physionet.org/content/vindr-cxr/1.0.0/

17. **[D17]** Boecking B, Usuyama N, Bannur S, et al. **MS-CXR v1.1.0.** PhysioNet; MIMIC-CXR-derived phrase-grounding benchmark.  
    https://physionet.org/content/ms-cxr/1.1.0/

18. **[D18]** Wu J, Agu N, Lourentzou I, et al. **Chest ImaGenome v1.0.0.** PhysioNet; MIMIC-CXR-derived scene graphs with a manually validated gold subset.  
    https://physionet.org/content/chest-imagenome/1.0.0/

19. **[D19A]** Reis EP, et al. **BRAX v1.1.0.** PhysioNet.  
    https://physionet.org/content/brax/1.1.0/

20. **[D20]** Hamamci IE et al. **CT-RATE official dataset card.**  
    https://huggingface.co/datasets/ibrahimhamamci/CT-RATE

21. **[D21]** Zhang S et al. **Development of a large-scale grounded vision language dataset for chest CT analysis (RadGenome-ChestCT).** *Scientific Data* (2025). DOI: `10.1038/s41597-025-05922-9`.  
    https://www.nature.com/articles/s41597-025-05922-9

22. **[D22]** Li Y, Zhang H, Xu M, et al. **PatchChestCT: A patch-level spatial annotation dataset for nine abnormalities in chest CT.** *Scientific Data* 13:1180 (2026). DOI: `10.1038/s41597-026-07793-0`.  
    https://www.nature.com/articles/s41597-026-07793-0

23. **[D23]** Forithmus. **MR-RATE official dataset card.** Public release 18 Mar 2026; paper/model marked “Coming Soon” at audit date.  
    https://huggingface.co/datasets/Forithmus/MR-RATE

24. **[D24]** NYU / Meta AI. **fastMRI dataset and project.**  
    https://fastmri.med.nyu.edu/

25. **[D25]** National Cancer Institute. **TCGA — Data Types Collected.** Diagnostic and tissue whole-slide images are listed as open-access via GDC.  
    https://www.cancer.gov/ccg/research/genome-sequencing/tcga/using-tcga-data/types

26. **[D26]** National Cancer Institute. **Clinical Proteomic Tumor Analysis Consortium (CPTAC).** Data access spans GDC/PDC and includes both open and controlled data types.  
    https://gdc.cancer.gov/about-gdc/contributed-genomic-data-cancer-research/clinical-proteomic-tumor-analysis-consortium-cptac

27. **[D27]** **CAMELYON17 — Data.** Official Grand Challenge page; 1,000 WSIs from five medical centres, CC0.  
    https://camelyon17.grand-challenge.org/Data/

28. **[D28]** **PANDA — Prostate cANcer graDe Assessment Challenge.** Official Kaggle data/rules pages.  
    https://www.kaggle.com/c/prostate-cancer-grade-assessment/data

29. **[D29]** Providence/Microsoft. **GigaTIME official repository.** Public sample includes 50 paired H&E/mIF test patches.  
    https://github.com/prov-gigatime/GigaTIME

30. **[D30]** Blankemeier L, Kumar A, et al. **Merlin: a computed tomography vision–language foundation model and dataset.** *Nature* 652, 1318–1328 (2026), data-availability record for the released Merlin abdominal CT dataset.  
    https://www.nature.com/articles/s41586-026-10181-8

31. **[D31]** Draelos RL, et al. **RAD-ChestCT Dataset.** Zenodo DOI `10.5281/zenodo.6406114`; full cohort metadata and initial 3,630-scan public release.  
    https://zenodo.org/records/6406114

## 17.2 Link-integrity audit

### Legacy scientific-source ledger

- The **72 legacy URLs** inherited from v3.2.1 were syntactically re-extracted and re-opened on 9 Sep 2026 during the prior audit.
- No malformed URL or citation pointing to an obviously different paper/model was found.
- The browser verifier directly retrieved **47/72** targets. **25/72** produced publisher/login/cookie/anti-bot/redirect-layer fetch failures rather than a demonstrated 404/wrong target.
- For those access-layer failures, the audit used exact title + DOI/PMID searches or authoritative model/repository metadata to verify bibliographic identity. Examples include Merlin, CT-RATE, Decipher-MR, Prima, Sonomate, Prov-GigaPath, PathoROB, nnMIL, TAMP, OCTCube-M and the 3 Sep 2026 clinical-reasoning benchmark.
- Therefore **“verifier could not fetch” is not labelled “broken link.”** Future maintenance should prefer DOI, PubMed, official repository, or canonical dataset-card URLs when publisher redirects become unstable.
- The **v3.3 additions [S70–S78]** were separately checked on 9 Sep 2026 against journal/DOI, PubMed/PMC, arXiv/OpenReview or official publication pages. Their addition does not retroactively change the old 72-link denominator.
- The **v3.4 additions [S79–S82]** were separately re-verified on 9 Sep 2026.
- The **v3.5 set [S83–S87]** was re-audited at structured-metadata level in v3.6: this caught and corrected the EchoPrime author-line error in S83 and version-pinned the current DINOv3 arXiv record (S86) to v3, revised 17 Jan 2026.
- The **v3.6 additions [S88–S91]** were verified against Nature Medicine/PubMed, Nature Communications/PubMed, the CVF CVPR proceedings, and PubMed/Elsevier metadata.
- The **v3.7 additions [S92–S101]** were verified on 9 Sep 2026 against Nature/Nature Communications/Nature Biomedical Engineering, ACL Anthology, Nature Methods, PubMed and Elsevier/ScienceDirect records. DentFound’s 8 Jul 2026 Author Correction is explicitly carried forward; PanoFM and the mammography/federated studies are versioned by DOI/article identifier.
- The **v3.8 freshness/counter-evidence additions [S102–S105]** were verified on 11 Sep 2026 against the version-of-record pages in *European Radiology*, *Nature Computational Science* and *Nature Medicine*. S103/S104 were published on 10 Sep 2026, after the v3.7 snapshot; S105 closes a previously underrepresented X-ray-tomography restoration branch.

### Structured bibliographic QA rule

A URL resolving to the intended article is **necessary but not sufficient**. For every new or changed source, verify independently:

`URL / DOI -> exact title -> author line -> publication status -> publication/version date -> volume/pages/article number -> numerical claims used in the dossier`

For live preprints/repositories, pin the exact revision/version when scientific wording or author lists have changed.

For system taxonomy, mechanically reject any `system_class` outside `core_fm / fm_derived / fm_enabled / companion` and any `scope` outside `generalist / modality_generalist / specialty_generalist / organ_specialist / workflow_specialist`. Section 4.0 is the source of truth.

### Dataset-source ledger

- The 29 initial dataset URLs introduced in this pass were individually opened; **21 were directly retrievable** in the verifier and **8 required exact-title/DOI/dataset-name fallback search** because of publisher/challenge/Kaggle access layers. None of those eight fallback checks indicated a wrong target.
- Two additional sources (**Merlin dataset [D30]** and **RAD-ChestCT [D31]**) were then added from the peer-reviewed CT literature and canonical data-availability pages, bringing the dataset ledger to **31 verified source entries**.
- One actual citation mismatch was found and fixed: the first candidate linked **ChestX-ray8** while quoting **ChestX-ray14** expanded-release counts. This is exactly the kind of version-lineage error that a URL-only audit would miss.

### Corrections discovered in earlier rounds and re-confirmed here

- The 2026 32-model pathology benchmark is **Nature Communications 17, Article 9012**, DOI `10.1038/s41467-026-76004-6`; the old dossier’s Article 5821 was wrong. [S28]
- `uniGradICON`’s PMC URL (`PMC13242914`) is valid and corresponds to the MICCAI 2024 paper, DOI `10.1007/978-3-031-72069-7_70`. [S44]
- The final peer-reviewed Prima paper is the authority for final scientific numbers; preprint-era values should not silently override the version of record. [S15]
- RadGenome-ChestCT illustrates live-version drift: the peer-reviewed paper reports **1.2M** grounded VQA pairs, while later live dataset materials may report **1.3M**. This dossier preserves the paper value in scientific claims and labels live-card changes separately. [D21]
- TotalSegmentator illustrates release drift: the original paper reports **1,204 CT examinations / 104 structures**; later toolkit/data releases may differ. [D6]

## 17.3 Claims from the supplied dataset note that were rejected or narrowed

The supplied dataset note was treated as an untrusted candidate-information list, not a source. The audit specifically rejected or narrowed the following formulations:

- **“Public”** was replaced by exact access status (e.g. MIMIC-CXR credentialed DUA; MR-RATE gated; FLARE challenge aggregate; PatchChestCT annotations separate from CT-RATE images).
- **“NIH ChestX-ray14 = the CVPR 2017 ChestX-ray8 paper”** was rejected as a bibliographic shortcut: the original paper reports the earlier ChestX-ray8 release, whereas the widely used expanded ChestX-ray14 release has different counts. [D15]
- **“RadGenome has 1.3M VQA pairs”** was version-qualified: the peer-reviewed descriptor reports 1.2M; a later live card may report 1.3M.
- **“AbdomenAtlas 3.0 has radiology reports”** was qualified: the reports are RadGPT/mask-assisted and radiologist-reviewed, not merely routine native clinical reports.
- **“CT-RATE has 50,188 CT scans”** was corrected: 25,692 scans/studies are expanded to 50,188 reconstructed volumes.
- **“MR-RATE is a frontier model”** is not currently supported: the dataset is live, while the official card still marks the paper/model as coming soon.
- **“Derived grounding sets provide external validation”** was rejected for CT-RATE→RadGenome/PatchChestCT and MIMIC-CXR→MS-CXR/Chest ImaGenome.
- **“TCGA/CPTAC are clean OOD tests”** was rejected as a default; the 2026 pathology benchmark explicitly warns that pretraining overlap cannot be excluded, especially for TCGA. [S28]

------------------------------------------------------------------------

# 18. Maintenance Protocol

This dossier should never again be marked “frozen.”

## Monthly audit

For every anchor system **and every high-level field-state conclusion**:

- cross-check modality × task × learning-paradigm coverage against at least one recent systematic/scoping review and record missing **categories**, not merely missing model names;
- refresh `audit_completeness` independently from field maturity;
- verify that any `partial/scaffold/gap` branch is still explicitly marked so absence-of-evidence is not converted into evidence-of-absence;
- publication status changed?
- model/checkpoint updated?
- licence changed?
- new independent benchmark?
- new external/prospective validation?
- retraction/correction?
- evidence of contamination?
- replacement model from same group?
- benchmark metric superseded?
- stronger counter-evidence or negative replication?
- reference-standard concern?
- statistical conclusion changed after re-analysis?
- access/reuse terms changed independently of artifact availability?
- does every model row still carry a system class consistent with Section 4.0?
- does every Section 15 synthesis statement remain traceable to the Field-State Ledger, modality sections, and cited evidence?
- has any deprecated evidence-profile field reappeared?
- for borderline/specialist systems, is scope explicitly stated rather than inferred from the word “foundation”; is adaptation/distillation/foundation lineage recorded wherever relevant, including adapted/distilled `core_fm` systems?
- does structured bibliography QA match URL/DOI/title/authors/status/date/volume-pages and the numbers quoted in text?
- do Markdown QA tests show zero malformed duplicate headings, zero escaped closing citation brackets, zero duplicate numbered subsections, and exact C/S/D reference resolution?

## Triggered audit

Immediately re-audit when:

- a new independent benchmark, replication or strong counter-example materially changes the field-state or maturity map;
- an anchor model receives a new peer-reviewed version or major checkpoint/data revision;
- a clinically prospective, human–AI, workflow or deployment study changes evidentiary strength;
- a major dataset, licence, access condition or model artifact changes;
- a correction, retraction, contamination finding or reproducibility failure appears;
- a modality-specific result exposes a missing branch or invalidates a cross-modality assumption.

**Venue is not a trigger criterion.** MIDL, domain journals, challenges and specialist clinical venues can produce evidence as important as Nature-family/CVPR/MICCAI papers.

## Versioning

Use semantic dossier versions:

- `vMAJOR`: taxonomy, landscape architecture, or maturity framework substantially changes;
- `vMINOR`: new anchor models/evidence branches;
- `vPATCH`: factual/citation corrections.

**Current recommended version:** `4.1.0-ground-layer-2026-09-11`.

Version 4.1.0 is the **ground-layer hardening release**. It preserves the landscape-only separation while broadening the organizing frame from foundation models to medical-imaging AI as a whole: imaging problems × learning paradigms × clinical roles, with explicit audit-completeness and recall controls.

Major v4.1.0 changes:

- changed the top-level ontology from an FM-centred atlas to a **medical-imaging AI ground layer** in which FMs are one major paradigm rather than the default organizing assumption;
- added a formal landscape-construction/coverage-recall method and explicit `audit_completeness` axis;
- added three orthogonal landscape axes: imaging problem stack, learning/data paradigm, and clinical task/use role;
- promoted label-efficient/data-centric learning, domain generalization/adaptation/OOD, continual learning, synthetic data, acquisition/QC/harmonization and quantitative imaging/radiomics/phenomics into the active landscape;
- added Ark/Ark+ as the heterogeneous-label supervised CXR FM lineage [S106,S107];
- added CXR disease phenomics/opportunistic prediction [S112];
- added *Nature Methods* benchmarking guidance [S113] and the 237-study FM systematic review as a recall backstop [S114];
- added MIQA and radiomics evidence anchors [S115,S116];
- separated field maturity from dossier audit completeness so under-audited branches cannot be mistaken for scientifically immature ones;
- expanded mastery criteria so future project generation must begin from problem × paradigm × clinical role rather than model-first ideation.

Major v4.0.0 changes:

- removed the canonical Claim Ledger and Research Blueprint as organizing structures;
- replaced them with a Field-State/Evidence-Maturity/Uncertainty Ledger and an Open Frontiers section;
- converted dataset-selection language from project-oriented to landscape-role-oriented;
- replaced the Model Study Template with a Landscape Entry Template;
- replaced claim-level conclusions with a cross-field synthesis + relative branch-maturity map;
- changed maintenance and mastery criteria so they preserve field mapping rather than silently generating projects;
- retained the source ledger, evidence policy, dataset genealogy, system registry, model-family coverage, benchmarking doctrine, reproduction policy and historical audit trail.

Historical v3.8.0 changes:

- clarified that E1–E4 source-form codes and E5A/E5B evidence-role modifiers are composable (`E1+E5A` is valid), removing the false impression that publication form and evaluator role are one mutually exclusive axis;
- hardened the `core_fm` rule: class reflects reusability/breadth of the **medical release**, while adaptation/distillation is captured separately in `foundation_lineage`;
- fixed registry propagation for Medical SAM3, MAIRA-2 and GigaPath-Flash;
- added explicit branch-maturity vocabulary so `core_fm` is not misread as “mature evidence” and specialty branches are not over-described;
- expanded C1–C14 into a fuller operational Claim–Evidence Graph with strongest alternative explanation and abandonment/demotion criteria; C1 now separates direct volumetric OCT evidence from ecological CT and contextual MRI evidence;
- added independent MAIRA-2 clinical counter-evidence [S102] to the grounded-reporting branch;
- added **PathSegmentor** [S103] and **CRISP** [S104], both published 10 Sep 2026, including the new promptable pathology-segmentation branch and unusually strong prospective intraoperative pathology evidence;
- added **HorusEye** [S105] to close an underrepresented self-supervised X-ray-tomography restoration branch;
- updated the living snapshot to 11 Sep 2026 and removed numeric “eight-branch” version drift from the historical audit text.

Historical v3.7.0 changes:

- made `system_class` a strict four-value enum and added a canonical Section 4.0 system registry with separate `scope` and `foundation_lineage`;
- added `specialty_generalist` to the canonical scope enum and eliminated descriptive class-value drift;
- split clinical-stage evidence into **study temporality** and **workflow exposure**, and replaced reference-standard “strength” with task-appropriate type + quality fields;
- fixed the C12 citation-range leakage, the stale C1–C12 curriculum rule and Section 15.1 citation scoping;
- added **PanoFM, DentFound and DentVLM** as a dedicated dental/oral imaging branch with patient/tooth/view hierarchy;
- expanded privacy from re-identification to patient-level membership inference using the 2026 *Nature* privacy audit;
- added a dedicated medical-VLM security branch covering oncology prompt injection, surgical-video injection and ACL 2026 transferable background attacks;
- broadened technical-shortcut evidence beyond pathology using the ten-dataset 2026 mammography study and added R13;
- added **UniFMIR** as the properly sourced earlier microscopy-restoration FM lineage;
- added bounded empirical federated-learning evidence while retaining the rule that federated learning is not a privacy theorem;
- added Section 3A.11 genealogy scaffolds for echocardiography, ophthalmology, dermatology, microscopy, mammography, endoscopy, dental/oral imaging and PET/CT;
- added R14 patient-level privacy + multimodal-security research program and corresponding curriculum coverage;
- expanded the scientific source ledger to S101 and strengthened self-QA to reject taxonomy enum drift.

Historical v3.6.0 changes:

- repaired malformed duplicate headings and the residual escaped citation introduced by the v3.5 build pipeline;
- corrected EchoPrime S83 author metadata and separated article-identity verification from structured bibliographic verification;
- pinned DINOv3 medical benchmark S86 to the current arXiv **v3 (17 Jan 2026)** title/revision;
- added scope as a second taxonomy axis and foundation-lineage tracking for FM-derived/FM-enabled systems;
- reclassified AbdomenNet as an FM-derived clinical system and Sonomate as an FM-derived workflow-specialist system under the operational taxonomy;
- added **PanDerm** as the dedicated multimodal dermatology anchor;
- added **FluoResFM** as the dedicated biomedical-microscopy restoration anchor;
- added **CARE** to the pathology adaptive-region/molecular-guidance frontier;
- added **XGeM** to generative synthesis with explicit separation between architectural breadth and CXR-centric evidence breadth;
- added empirical confidence promotion/demotion criteria and study-design-specific risk-of-bias guidance;
- tightened C3 evidence-maturity asymmetry and C13 internal-ablation-versus-external-full-system interpretation;
- strengthened future monthly QA with structured bibliography and raw-Markdown integrity checks.

Historical v3.5.0 changes:

- replaced the old E5/evidence-profile definitions with one canonical E1–E4/E5A/E5B schema;
- removed obsolete single-field independence/reproducibility/contamination definitions;
- typed and tightened C1–C14 claims;
- made Section 15 a direct C1–C14 projection and moved non-ledger statements to field-state observations;
- propagated Core FM / FM-derived / FM-enabled / companion classification into anchor tables;
- added EchoPrime, EchoCLIP and Echo-Vision-FM as a dedicated echocardiography branch and split R8 into acquisition-interaction versus examination-level reasoning;
- added generic DINOv3 medical counter-evidence to the domain-alignment claim;
- reframed R3B/R3C as benchmark/data construction before longitudinal model claims;
- added a severity-weighted clinical failure budget to R4 and explicit inferential-unit hierarchy to R7;
- propagated SPIRIT-AI/PROBAST+AI into R9/M03C;
- replaced remaining `contamination risk` template language with canonical contamination status;
- added direct MammoScope sourcing, removed unsupported EyeFound/EyeFM model-specific naming, fixed pathology section numbering and cleaned escaped internal citations;
- updated link-audit documentation for S79–S87.

Major v3.3.0 changes:

- E1–E5 retained only as source-provenance codes; multidimensional evidence profiles added;
- availability split into artifact, access and reuse-rights axes;
- claim–evidence graph added with counter-evidence, alternative explanations and abandonment criteria;
- formal statistical-inference doctrine and reference-standard taxonomy added;
- CLAIM 2024, STARD-AI, TRIPOD+AI, DECIDE-AI, CONSORT-AI and FUTURE-AI integrated without conflating reporting compliance with evidence strength;
- R2 gains a controlled 2D/2.5D/3D causal-ablation track;
- R3 split into static grounding, longitudinal correspondence and quantitative-change stages;
- R4 gains oracle decomposition controls;
- PET/CT/nuclear medicine promoted from a coverage gap to an emerging dedicated branch;
- radiotherapy imaging/treatment-planning semantics added as a workflow-specialized branch;
- RAD-DINO peer-reviewed evidence corrected and H-Optimus evidence notation cleaned;
- modality-specific dataset-genealogy incompleteness is now explicit rather than hidden.

------------------------------------------------------------------------

# 19. Final Standard for Expert-Level Ground-Layer Mastery

You should not consider the medical-imaging AI ground layer mastered because you can summarize flagship foundation-model papers.

For every major modality, clinical task and learning paradigm, you should be able to:

1. explain the **clinical/data problem without naming a model**;
2. place it on all three landscape axes: **imaging problem stack × learning/data paradigm × clinical role**;
3. identify the strongest task-specific/specialist baseline **and** the relevant FM/generalist alternatives;
4. explain whether the real bottleneck is model capacity, labels, data quality, acquisition, geometry, domain shift, temporal drift, uncertainty, workflow or integration;
5. identify the major model/method lineages and what genuinely differentiates them;
6. state what data they saw, the relevant counting unit, and the likely dataset genealogy;
7. distinguish native human labels/reference standards from weak, NLP-mined, model-generated, LLM-generated or revised supervision;
8. state exactly what the strongest published results demonstrate — and what they do not;
9. identify the strongest independent evaluation or counter-evidence, or explicitly state that none has been found/audited;
10. distinguish **field maturity** from **dossier audit completeness** and refuse to infer scientific immaturity from an audit gap;
11. distinguish technical maturity from independent-evidence maturity and clinical-translation maturity;
12. explain the dominant scanner/site/protocol/population shifts, shortcuts, geometry/acquisition constraints and failure modes;
13. understand domain generalization/adaptation, OOD/selective prediction and continual-learning options when static pretraining is unlikely to be sufficient;
14. understand label-efficient/data-centric alternatives when annotation—not representation capacity—is the limiting resource;
15. understand synthetic-data options and separately evaluate fidelity, task utility, privacy and clinical/biological validity;
16. understand quantitative-imaging/radiomics/phenomics alternatives when the endpoint is biomarker, prognosis, response or opportunistic screening rather than ordinary diagnosis;
17. identify artifact/access/reuse restrictions and whether reproduction is realistically possible;
18. understand the relevant statistical unit, calibration/uncertainty requirements and reference-standard limitations;
19. place the branch on the relative maturity map without using publication prestige, parameter count or “foundation model” status as a proxy;
20. state the major unresolved frontier areas **without prematurely converting them into a preferred project**;
21. identify what new evidence would materially change the current field interpretation;
22. know when the dossier itself is insufficient and a branch-specific literature expansion is mandatory before judging novelty or opportunity.

When you can do this across the major branches, the dossier is serving its intended role: **a bias-resistant evidence map from which later medical-imaging AI projects can be designed, rather than a document that silently predetermines those projects.**
