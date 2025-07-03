# RP1_CT_analysis
This repository contains codes of the research project thesis by Muriel Hammond. The study is using Circuit Topology to study folding properties of ordered and disordered proteins. 

This project uses circuit topology (CT) to analyze protein folding properties in both ordered and disordered proteins. This README provides an overview of the notebooks and scripts used throughout the project, organized by folder.

The folder CT_code/PDB_files_preparation contains all scripts for preprocessing PDB files from various structural databases. These scripts download, filter, and clean structural data. Only single-chain proteins with fewer than 200 frames are retained. Hydrogen atoms and non-ATOM lines are removed, and residues are renumbered. Cleaned PDB files are saved to CT_code/PDB_files_preparation/output/pdbs_traj/single_chain/. These can be copied to protein_CT/input/ using the final block of code. 

- The PED_preprocess notebook downloads and preprocesses data from the PED database (https://proteinensemble.org/). 
- The SCOPe_preprocess notebook handles SCOPe data (https://scop.berkeley.edu/). 
- The KPRO_preprocess notebook processes manually downloaded .gz files from the KPRO database (https://folding.biofold.org/k-pro/api/docs/#/), and creates KPRO_df with folding and unfolding rate data. 
- The ACPRO_preprocess notebook processes data from the ACPRO database (https://www.ats.amherst.edu/protein/) and creates ACPRO_df with rate data as well. 
- The cleanregions_preprocess notebook extracts the longest folded and disordered regions from PED and SCOPe proteins. To run this code: Input trajectories must be located in CT_code/CT_analysis/trajectory/5/. Regions are classified as folded if they contain secondary structures ‘H’ (helix) or ‘E’ (extended), and as disordered if they contain ‘C’ (coil). Regions must be at least five residues long and can have a maximum of three interruptions. 

The folder CT_code/Protein_CT contains scripts developed by Duane Moes to compute CT contact types. 
- The MAIN script calculates the number of parallel, series, and cross contacts for each protein using PDB files placed in input_files/pdb/. Results are stored in results/statistics/. 
- The MAIN_cleanregions script computes CT contact matrices for clean folded and disordered regions, which are used later for slicing PDBs.

The folder CT_code/CT_analysis contains all analysis notebooks using CT data for modeling and visualization. 
- The CT_gamma_predict notebook predicts the Flory exponent (γ) using sigmoid regression based on CT data. The model is trained using proteins with fewer than 200 residues and visualized in a 3D plot, then tested on longer proteins. 
- The CT_folding_predict notebook uses logistic regression to classify folded versus disordered regions, trained on clean data from PED and SCOPe and validated on the full merged database, with a 3D plot of the classification result. 
- The CT_energy_pred notebook uses CT and contact order (CO) to predict free energy of folding, folding rate (kf), and unfolding rate (ku) using linear regression. Models are validated on ACPRO data, and predictions are applied to the merged database. 
- The CT_sequence_topology notebook explores the relationship between protein sequence and CT by counting residue pairs per contact type and applying chi-square independence tests. After, we analyse the if some residue pairs have a higher probability to form a CT arrangement in parallel, series, or cross. For this, the same data is used. 
- The MERGED_normalization notebook normalizes CT contacts (parallel, series, and cross) in PED and SCOPe proteins by protein length and by percentage contact type. These normalized values are used in thermodynamic and kinetic prediction models.
