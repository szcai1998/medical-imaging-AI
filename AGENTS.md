# AGENTS.md - Medical Imaging AI Operational Constitution & Engineering Guidelines

> Universal operational constitution, behavioral constraints, and Git workflow rules for autonomous AI coding agents working in the `medical-imaging-AI` repository.

---

## 1. Core Behavioral Controls (The Karpathy Rules)

Coding agents suffer from a systemic "defaults problem": a bias to barrel ahead, hallucinate requirements, make unqualified SOTA claims, over-engineer abstractions, and perform unsolicited renovations. In this repository, you MUST strictly obey these five core principles:

### Rule 1: Think Before Coding (Never Barrel Ahead)
* **Surface Assumptions Explicitly**: State your technical assumptions, evidence classifications, and architectural design before touching files.
* **Halt on Ambiguity**: When clinical targets, dataset versions, or benchmark metrics are ambiguous or contradictory, **STOP and ask the user**. Never silently guess intent.
* **Present Trade-offs**: When multiple approaches exist (e.g., benchmark evaluation on blind test vs. cross-validation, foundation model vs. task-specific baseline), present the trade-offs clearly.
* **Expose Confusion**: If citations, counting units, or previous documentation appear inconsistent, state your confusion explicitly. Do not paper over latent errors with fabricated data.

### Rule 2: Simplicity First (Write 50 Lines, Not 500)
* **Minimum Viable Documentation & Code**: Implement strictly what was requested. No speculative abstractions, premature benchmark wrappers, or unrequested framework scaffolding.
* **YAGNI (You Aren't Gonna Need It)**: Lean, precise markdown cards and focused Python scripts protect context windows and maintain repository hygiene.

### Rule 3: Surgical Edits & Living Documentation
* **Strict Minimal Blast Radius**: Touch ONLY lines strictly necessary to satisfy the request. Every edit must be directly traceable to the user's instruction.
* **Zero Unsolicited Renovations**: NEVER reformat untouched lines, reorganize folder structures, clean up whitespace in adjacent sections, or refactor unrelated files.
* **Synchronous Documentation Sync**: Whenever you add, modify, or rename datasets, models, or scripts, update the corresponding `README.md` index files in the exact same commit. Prevent documentation drift.
* **Preserve Documentation Integrity**: Retain all existing analytical notes, evidence caveats, and bibliographies. Clean up only transient scratch files introduced during your session.

### Rule 4: Evidence Rigor, Anti-Flailing & Scientific Integrity
* **No Unqualified "SOTA"**: Every competitive claim MUST specify: task, cohort, unit of analysis, evaluation split (held-out blind test vs. CV), metric with uncertainty, and baseline comparator set.
* **Adherence to Canonical Evidence Codes**: Use formal evidence tags (`E1` Peer-Reviewed Version-of-Record, `E2` Official Challenge/Model Card, `E5A` Independent Benchmark).
* **3-Strike Anti-Flailing Circuit Breaker**: If an automated script, web research query, or test fails after **2 consecutive attempts**, STOP immediately. Re-evaluate your approach, report the obstacle, and request human guidance rather than looping blindly.

### Rule 5: Zero-Trust Security & Data Governance
* **Zero Credential Exposure**: NEVER print, log, or commit API keys, Hugging Face tokens, PhysioNet credentials, or `.env` files into commits or PR descriptions.
* **Protected Health Information (PHI) & Data Isolation**: NEVER commit raw patient DICOM/NIfTI scans, private patient identifiers, or unanonymized clinical reports. Ensure all large binaries (`*.dcm`, `*.nii.gz`, `*.pt`, `*.safetensors`) remain strictly ignored by `.gitignore`.

---

## 2. Medical AI Cartography & Documentation Standards

All dataset and model cards created in `docs/` must strictly follow the repository's standardized schemas:

### 2.1 Dataset Documentation Schema (`D-A-T-A-S + Leaderboard`)
Every file in `docs/01_datasets/` must incorporate:
1. **Quick Metadata**: Canonical Dossier ID (`[D1]`–`[D31]`), Modality, Challenge/Publication Year, Access Level, and formal **Evidence Code (`E1`/`E2`/`E5A`)**.
2. **[D] Domain & Clinical Target**: Concrete clinical failure modes addressed and target anatomy/lesions.
3. **[A] Acquisition Physics & Scale**: Verified counting-unit discipline:
   $$\text{patient} \neq \text{examination/study} \neq \text{series} \neq \text{reconstructed volume} \neq \text{2D slice} \neq \text{voxel mask}$$
   Record slice thickness, contrast phases, and multi-center scanner distribution.
4. **[T] Truth & Annotation Provenance**: Explicitly differentiate native human expert consensus, semi-automatic radiologist revision, model-assisted labels (e.g., RadGPT), and NLP report-mined tags.
5. **[A] Access, Terms & Artifacts**: Exact repository/download URL, licensing terms (Apache-2.0, CC BY, CC BY-NC-SA, PhysioNet DUA), and artifact availability.
6. **[S] Systemic Lineage & Genealogy**: Parent-to-derivative genealogy (e.g., CT-RATE $\rightarrow$ RadGenome) and circular pretraining contamination warnings.
7. **Verified SOTA Leaderboard (Top 5 Rank)**: Real test standings with team names, architectures, official metrics, and an explicit **Evaluation Split & Setting** column.
8. **Ground-Layer Practical Guidance**: Compute footprint (VRAM, disk), minimal runnable Python verification snippet with dependency header, and dominant clinical failure modes.

### 2.2 Model Documentation Schema (`P-A-B-H-A`)
Every file in `docs/02_models/` must follow the 5-point snapshot:
* **[P] Clinical Problem**: Failure mode, clinical bottleneck, or data limitation addressed (1 sentence).
* **[A] Architectural Core**: Novel mechanism, attention variant, loss function, or scaffold (1–2 sentences).
* **[B] Benchmark & Delta**: Authoritative metric and delta proving superiority against baselines on standard test sets.
* **[H] Hardware Footprint**: Minimum VRAM for inference (FP16/quantized), training GPU profile, and workstation feasibility.
* **[A] Access & Artifacts**: Direct paper link/DOI, GitHub URL, Hugging Face checkpoint, and license.

---

## 3. Python & Scientific Computing Discipline

* **Environment Isolation**: Always work within a dedicated virtual environment (`.venv`). Never install packages globally.
* **Deterministic Dependencies**:
  * Use modern packaging tools (`uv` or `poetry`).
  * Verify package existence and version compatibility before proposing installation.
* **Standard Medical Imaging Tooling**:
  * For 3D volumetric images (`.nii.gz`): Prefer `nibabel` or `SimpleITK`.
  * For DICOM headers & coordinate geometry: Use `pydicom` or `SimpleITK` (respecting LPS/RAS physical coordinate conventions).
  * For deep learning pipelines: Build on `MONAI` and `PyTorch`.
* **Script Header Contract**: Every minimal verification snippet in documentation must state its dependency requirements in the top line (e.g., `# Requirements: pip install nibabel`).

---

## 4. GitHub Flow & Version Control (`gh` CLI)

All modifications are managed via Trunk-Based Development with clean linear history.

### 4.1 Branching Rules
* **No Speculative Commits on `main`**: Non-trivial features and major documentation modules should be developed on short-lived feature branches cut from `origin/main`.
* **Branch Naming Standard**: strictly lowercase `kebab-case` with category prefixes:
  * `feat/<slug>`: New dataset module, model cards, or benchmark integration
  * `fix/<slug>`: Correcting bibliographies, metrics, or factual errors
  * `docs/<slug>`: Documentation refactoring, index updates, or dossier maintenance
  * `chore/<slug>`: Tooling, linter, or repository configurations

### 4.2 GitHub CLI (`gh`) Workflow
1. **Branch & Commit**:
   ```bash
   git checkout -b feat/dataset-module-2-ct-mri
   git add docs/01_datasets/02_radiology_ct_mri/
   git commit -m "feat(datasets): add volumetric CT/MRI benchmark cards"
   git push -u origin feat/dataset-module-2-ct-mri
   ```
2. **Pull Request via `gh`**:
   ```bash
   gh pr create --title "feat: add volumetric CT/MRI benchmark cards" --body "Closes #..."
   ```
3. **Squash and Merge**:
   ```bash
   gh pr merge --squash --delete-branch
   ```

### 4.3 Commit Conventions
* Use conventional commit headers: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`.
* Explicit staging only: `git add path/to/file` (NEVER bare `git add .` without review).

---

## 5. The Verification Ladder

Before declaring any documentation card or script complete, verify through this escalating ladder:

```
[Rung 3] Code Execution Check (Execute Python snippets via python -c to verify imports & syntax)
   ▲
[Rung 2] Metric & Split Hygiene (Verify blind test vs. CV split explicitly labeled)
   ▲
[Rung 1] Link & Identifier Audit (Check that DOIs, arXiv IDs, Grand Challenge URLs are live and accurate)
   ▲
[Rung 0] Markdown & Syntax Validation (Verify table alignments, heading hierarchies, no broken internal links)
```

---

## 6. Definition of Done (DoD) & Pre-Flight Checklist

Before presenting completed work to the user, verify every item on this checklist:

- [ ] **Evidence Coded**: Are all claims and dataset cards tagged with their canonical evidence codes (`E1`/`E2`/`E5A`)?
- [ ] **Split Transparency**: Are leaderboard metrics explicitly attributed to their evaluation split (blind test vs. CV)?
- [ ] **Counting Units Verified**: Are patients, studies, scans, and masks clearly distinguished without conflation?
- [ ] **Contamination Audited**: Are pretraining overlaps and parent-derivative lineages documented?
- [ ] **Code Snippets Annotated**: Do verification code blocks include dependency installation instructions?
- [ ] **Living Docs Synced**: Are parent `README.md` index tables and root navigation updated in the same commit?
- [ ] **Zero PHI & Secrets**: Are all patient data and credentials excluded from version control?
- [ ] **Clean Git Tree**: Is the working directory clean with all changes committed under Conventional Commits?
