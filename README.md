# AI-Driven Drug Discovery, 3D Molecular Docking & Clinical Trial Optimization Platform

An end-to-end, **100% local-first** scientific workbench unifying 5-endpoint ADMET machine learning, AutoDock Vina 3D biophysical docking, de novo retrosynthetic design, Lipid Nanoparticle (LNP) profiling, and live ClinicalTrials.gov analytics.

---

## 1. Core Scientific Modules

| Module | Underlying Engines & Algorithms | Primary Deliverable |
| :--- | :--- | :--- |
| **Molecular Inspector & 3D Physics** | RDKit (2D Descriptors, ETKDGv3 + MMFF94), `py3Dmol` | Lipinski/Veber rules, QED, Synthetic Accessibility (SA), interactive 3D conformers |
| **5-Endpoint ADMET Predictor** | 2048-bit Morgan Fingerprints + Calibrated Scikit-Learn Models | Aqueous Solubility (ESOL), BBB Penetration, ClinTox, hERG Cardiotoxicity & CYP3A4 Risk |
| **Target Inspector & 3D Docking** | UniProt REST API, AlphaFold DB (v6), Meeko & AutoDock Vina v1.2.5 | Live 3D protein retrieval, binding free energy (Δ G kcal/mol), \(K_d\) (nM) & ranked poses |
| **De Novo Designer & Retrosynthesis** | BRICS Recombination, SELFIES Evolution & Reaction SMARTS | Multi-objective analog generation + wet-lab reaction routes (Amide, Ester, Suzuki, etc.) |
| **LNP & Ionizable Lipid Profiler** | Custom RDKit SMARTS Linker & Apparent \(pK_a\) Heuristics | Tertiary amine \(pK_a\) window (6.2–6.6) & biodegradable linker audit (ester, silyl, disulfide, acetal) |
| **Clinical Trial Optimizer & Reports** | ClinicalTrials.gov API v2 & Automated `FPDF2` Engine | Phase split, median enrollment/duration, failure themes & 1-click PDF candidate dossiers |

---

## 2. Validated End-to-End Benchmarks

* **Small-Molecule CNS Case Study (Donepezil + ACHE `P22303`):** Confirmed zero Lipinski violations, high Blood-Brain Barrier penetration, active-site pocket binding (Δ G = -6.04 kcal/mol, RMSD \(1.52\text{ \AA}\)), and synthesized 40 live Alzheimer's trials revealing a median enrollment of **186 patients**, median duration of **29 months**, and **Efficacy/Futility** as the leading termination driver.
* **Multi-Modality LNP Benchmark (Ionizable Lipid + ApoE `P02649`):** Evaluated 742–918 Da ionizable lipid architecture (cLogP > 13), verified low hERG (`0.013`) and CYP3A4 (`0.117`) liability, mapped Reductive Amination & Esterification synthesis steps, and quantified monomeric ApoE surface affinity (Δ G = -3.98 kcal/mol, \(K_d \approx 1.21\text{ mM}\)).

---

## 3. Strict Engineering Principles

* **Zero-Cloud IP Privacy:** All SMILES structures, 3D conformers, and docking simulations execute locally on your hardware with zero external LLM data leakage.
* **No Mock Scientific Predictions:** Every chemical structure is sanitized with RDKit; all ML models are trained with 5-fold cross-validation on public MoleculeNet / Therapeutics Data Commons datasets.
* **Live External APIs:** Real-time structural and clinical intelligence retrieved directly via UniProt, AlphaFold DB (v6), and ClinicalTrials.gov API v2.

---

## 4. Quick Start Guide

### Step 1: Environment Setup
```bash
# Option A: Conda (recommended)
conda env create -f environment.yml
conda activate ai-drug-discovery

# Option B: Docker
docker-compose build
docker-compose up
```

### Step 2: Train ADMET Models (Required Once)
```bash
python setup_and_train.py
# or
make train
```
Downloads real datasets (Delaney/ESOL, BBBP, ClinTox, hERG, CYP3A4), computes 2048-bit Morgan fingerprints + RDKit descriptors, trains calibrated models with 5-fold CV, and writes artifacts to `models/`.

### Step 3: Launch Application
```bash
streamlit run app.py
# or
make run
```
Open `http://localhost:8501` in your browser.

### Step 4: Run Tests
```bash
pytest tests/ -v
# Network integration tests (UniProt, AlphaFold, ClinicalTrials.gov):
pytest tests/ -v -m network
```

---

## 5. Project Layout

```text
ai-drug-discovery-platform/
├── app.py                     # Streamlit multi-page scientific workbench
├── setup_and_train.py         # Dataset download & 5-fold CV model training
├── environment.yml            # Conda environment specification
├── Dockerfile / docker-compose.yml
├── Makefile
├── src/
│   ├── chemistry_utils.py     # RDKit descriptors, rules, ETKDGv3 + MMFF94 3D
│   ├── admet_predictor.py     # 5-endpoint ADMET inference
│   ├── target_docking.py      # UniProt, AlphaFold DB & AutoDock Vina pipeline
│   ├── molecule_generator.py  # BRICS/SELFIES design & Reaction SMARTS retrosynthesis
│   ├── lnp_profiler.py        # Ionizable lipid pKa & biodegradable linker audit
│   └── clinical_optimizer.py  # ClinicalTrials.gov API v2 & FPDF2 report generator
├── docs/                      # Executive brief, pitch deck & README PDFs
├── models/                    # Trained .joblib artifacts + model_metrics.json
├── data/                      # Auto-populated MoleculeNet datasets
└── tests/                     # Unit & live API integration tests
```
