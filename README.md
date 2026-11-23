# AI-Driven Drug Discovery, 3D Molecular Docking & Clinical Trial Optimization Platform

An end-to-end, **100% local-first** scientific workbench unifying 5-endpoint ADMET machine learning, AutoDock Vina 3D biophysical docking, de novo retrosynthetic design, Lipid Nanoparticle (LNP) profiling, and live ClinicalTrials.gov analytics.

> **Technical Validation Report:** [Download Full PDF Report (`AI-Drug-Discovery-Technical-Report.pdf`)](./AI-Drug-Discovery-Technical-Report.pdf)

---

## 1. Core Scientific Modules

| Module | Underlying Engines & Algorithms | Primary Deliverable |
| :--- | :--- | :--- |
| **Molecular Inspector & 3D Physics** | RDKit (2D Descriptors, ETKDGv3 + MMFF94), `py3Dmol` | Lipinski/Veber rules, QED, Synthetic Accessibility (SA), interactive 3D conformers |
| **5-Endpoint ADMET Predictor** | 2048-bit Morgan Fingerprints + Calibrated Scikit-Learn Models + Tanimoto AD Gate | Aqueous Solubility (ESOL), BBB Penetration, ClinTox, hERG Cardiotoxicity & CYP3A4 Risk with Applicability Domain alerts |
| **Target Inspector & 3D Docking** | UniProt REST API, AlphaFold DB (v6), Meeko & AutoDock Vina v1.2.5 | Live 3D protein retrieval, physiological pH 7.4 microspecies enumeration, binding free energy ($\Delta G$, kcal/mol), $K_d$ (nM) & ranked poses |
| **De Novo Designer & Retrosynthesis** | BRICS Recombination, SELFIES Evolution & Reaction SMARTS | Multi-objective analog generation + wet-lab reaction routes (Amide, Ester, Suzuki, etc.) |
| **LNP & Ionizable Lipid Profiler** | Random Forest $\text{p}K_a$ Regressor (`FCFP4` + RDKit Descriptors) & SMARTS Linker Audit | ML apparent $\text{p}K_a$ prediction (target 6.2–6.6 window), pH 7.4 $\rightarrow$ 5.5 protonation shift & biodegradable linker audit |
| **Clinical Trial Optimizer & Reports** | ClinicalTrials.gov API v2 & Automated `FPDF2` Engine | Phase split, median enrollment/duration, failure themes & 1-click PDF candidate dossiers |

---

## 2. Peer-Reviewed Cheminformatics & Biophysical Protocols

To ensure rigorous scientific validity beyond black-box machine learning, this platform implements validated cheminformatics and structural biology workflows derived from peer-reviewed literature by Paul Czodrowski and collaborators:

| Literature Reference | Target Module(s) | Implemented Scientific Upgrade |
| :--- | :--- | :--- |
| **Czodrowski, *J. Chem. Inf. Model.* (2013)** — *hERG Me Out* | `src/admet_predictor.py`, `src/expanded_admet.py`, `app.py` | Tanimoto Applicability Domain gating, isotonic probability calibration, and multi-endpoint safety profiling |
| **Baltruschat & Czodrowski, *F1000Research* (2020)** — *Machine learning meets pKa* | `src/lnp_analyzer.py`, `src/admet_predictor.py` | RDKit salt/tautomer canonicalization and Random Forest macroscopic $\text{p}K_a$ regression (`FCFP` + physicochemical descriptors) |
| **Czodrowski et al., *ChemMedChem* (2025)** — *Protonation Effects in Protein-Ligand Complexes* | `src/vina_docker.py`, `app.py` | Physiological pH 7.4 microspecies enumeration (`+1` cations, `-1` anions, and neutral states) with competitive AutoDock Vina scoring |

### Key Scientific Enhancements
* **Chemical Standardization & Applicability Domain Gating:** Raw SMILES undergo RDKit salt stripping (`LargestFragmentChooser`) and tautomer canonicalization (`TautomerEnumerator`). Every query computes maximum Morgan fingerprint Tanimoto similarity ($T_{\max}$) against the reference training space and flags out-of-domain scaffolds ($T_{\max} < 0.40$).
* **ML Macroscopic $\text{p}K_a$ & Endosomal Protonation Modeling:** The LNP Profiler predicts apparent $\text{p}K_a$ using a Random Forest regressor trained on 1024-bit Feature Morgan Fingerprints (`FCFP4`, `useFeatures=True`) + 12 RDKit descriptors, paired with Henderson–Hasselbalch ionization profiling at blood pH (`7.4`) vs. endosomal pH (`5.5`).
* **pH-Aware 3D Molecular Docking:** AutoDock Vina evaluates competing physiological microspecies at pH $7.4 \pm 1.0$ (`+1` cations, `-1` anions, and neutral states) and renders a side-by-side $\Delta G$ (`kcal/mol`) comparison table.

---

## 3. References

1. Czodrowski, P. *hERG Me Out*. **Journal of Chemical Information and Modeling**, 2013, 53 (9), 2240–2251.
2. Baltruschat, M.; Czodrowski, P. *Machine learning meets pKa*. **F1000Research**, 2020, 9, 113.
3. Czodrowski, P. et al. *Protonation Effects in Protein-Ligand Complexes*. **ChemMedChem**, 2025, e202400953.
