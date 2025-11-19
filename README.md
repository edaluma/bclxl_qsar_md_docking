# Bcl-xL Inhibitor Design: QSAR → MD → Docking

This repository contains a complete in silico workflow to identify potential inhibitors of the anti-apoptotic protein **Bcl-xL**. The project combines **QSAR modeling**, **molecular dynamics simulations**, and **structure-based docking** to propose candidate molecules that can bind to the BH3 binding groove of Bcl-xL.

## Project Overview

The pipeline is organized in three main stages:

1. **QSAR Modeling (Classification & Regression)**  
   A dataset of 1000 small molecules (balanced: 500 actives, 500 inactives) was used to build classification and regression models.  
   - Molecular descriptors were calculated using the **Mordred** descriptor engine (2D + 3D descriptors).  
   - For **classification**, machine learning models such as **Logistic Regression**, **Random Forest**, and **Support Vector Machine (SVM)** were used.  
   - For **regression**, **Multiple Linear Regression (MLR)** and **MLR with 10-fold Cross-Validation** were applied.  
   - Model evaluation included ROC-AUC, accuracy, sensitivity, specificity, PR-AUC, enrichment metrics (EF, NEF) for classification, and R², RMSE, and MAE for regression.
  
2. **Molecular Dynamics Simulations (Apo & Holo Bcl-xL)**  
   MD simulations were performed on the **apo form (2LPC)** and the **holo form (1G5J)** of Bcl-xL using **GROMACS with the OPLS-AA/L force field**. The goal was to generate equilibrated receptor conformations and explore the conformational landscape of the protein.

   - **Apo system (2LPC):**  
     The representative NMR conformer was cleaned (removal of His-tag), solvated, minimized, equilibrated (NVT/NPT), and simulated for **100 ns**.  
     Structural analysis included RMSD, RMSF, radius of gyration, hydrogen-bond profiles, and DSSP-based secondary structure evaluation.

   - **Holo system (1G5J, Bcl-xL–BAD complex):**  
     The soluble domain of Bcl-xL (chain A) and the 25-residue BAD BH3 peptide (chain B) were prepared by removing artifact residues.  
     The system was minimized, equilibrated, and simulated for **50 ns**, followed by the same structural analyses performed for the apo form.

   - **Conformational analysis:**  
     Additional analyses included **RMSD-based clustering**, **PCA and free-energy surface projection**, and **DBSCAN clustering** to identify dominant conformational states of the holo complex.

   These processed MD trajectories were used to select representative structures for the subsequent **structure-based docking** stage.

3. **Structure-Based Docking**  
   Candidate molecules selected from QSAR are docked into Bcl-xL using a structure-based virtual screening protocol.  
   - Receptor preparation from MD snapshots  
   - Docking of selected ligands  
   - Ranking by docking scores and, if applicable, rescoring  
   - Visual inspection of binding modes and proposal of two final inhibitor candidates

The goal of the project is to propose **two molecules** with the best inhibitory potential against Bcl-xL, supported by QSAR predictions, MD-derived structures and docking poses.

## Repository Structure

Planned structure of this repository:

bclxl_qsar_md_docking/
│
├── README.md                  # Project overview and workflow description
├── .gitignore                 # Python + R ignore rules
│
├── qsar/                      # QSAR modeling workflow
│   ├── notebooks/             # Jupyter notebooks (classification, regression)
│   ├── src/                   # Python/R scripts for descriptor calculation, modeling
│   ├── results/               # Figures, plots, tables
│   ├── report/                # QSAR report PDF
│   └── data/                  # Dataset folder (not included in repository)
│       └── README.md          # Instructions for dataset placement
│
├── md/                        # Molecular Dynamics simulations
│   ├── input_files/           # Edited PDBs, topology files, MD-ready structures
│   ├── gromacs_scripts/       # .mdp files for minimization, NVT, NPT, production
│   ├── analysis/              # RMSD, RMSF, Rg, DSSP, clustering, PCA notebooks/scripts
│   ├── results/               # Plots from the MD analysis
│   └── report/                # MD analysis PDF
│
└── docking/                   # Structure-based docking workflow
    ├── protocol/              # Receptor preparation, ligand preparation, workflow steps
    ├── poses/                 # Docking poses, best-ranked structures
    ├── scoring/               # Vina scores, rescoring scripts/notebooks
    ├── results/               # Docking plots, binding mode visualizations
    └── report/                # Docking report
