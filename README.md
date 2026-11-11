# dhp_binding
For model validation and MD analysis of dihydropyridine drug binding to CaV1.2

For results and findings, please read: 

This consolidated library of scripts is organized by what stage of the pipeline it belongs to and what it is used for. Each stage/subfolder has its own README with per-script usage details.

These are the canonical, template copies of each script developed with the help of GenAI. Therefore, please exercise caution and discretion when using these scripts for the user's personal systems as they were initially produced to work on a MacOS system and specifically for AlphaFold2 CaV1.2 models.
Pipeline order

The project's overall workflow runs roughly in this order:

| # | Folder | Stage | Project location |
|---|---|---|---|
| 1 | [`01_ModelValidation_Pipeline`](01_ModelValidation_Pipeline) | Orchestrator for stages 2–5 below | `1_MVP-PreDock/` |
| 2 | [`02_GlobalRMSD_Matrix`](02_GlobalRMSD_Matrix) | Whole-structure pairwise RMSD across all models | `1_MVP-PreDock/*/2_GlobalMatrix/` |
| 3 | [`03_ResidueRMSD_Matrix`](03_ResidueRMSD_Matrix) | Pairwise RMSD over a curated residue selection | `1_MVP-PreDock/*/3_ResidueMatrix/` |
| 4 | [`04_LigandSphere_RMSD`](04_LigandSphere_RMSD) | RMSD local to the ligand-binding pocket, + stats | `1_MVP-PreDock/*/1_LigandSphere/<Ligand>/` |
| 5 | [`05_HOLE2_PoreAnalysis`](05_HOLE2_PoreAnalysis) | Pore-radius profiling via HOLE2 (Linux only) | `1_MVP-PreDock/*/4_Hole2/` |
| 6 | [`06_Apo_PocketGeometry`](06_Apo_PocketGeometry) | Apo (no-ligand) pocket-geometry comparison, relaxed vs. nonrelaxed | `2.5_Analysis/Apo-PocketGeometry/` |
| 7 | [`07_MD_LigandAnalysis`](07_MD_LigandAnalysis) | Full ligand-bound MD trajectory analysis (RMSD/RMSF, contacts, H-bonds, etc.) | `4.5_Analysis/MD-LigandAnalysis/` |
| 8 | [`08_Docking`](08_Docking) | AutoDock Vina docking of benidipine into validated receptor models (cation + neutral states), plus score plotting/stats | `3_Docking/` |

Stages 1–5 validate AlphaFold-derived models before docking/MD (pre-dock). Stage 8 docks the ligand into those validated models. Stage 6 looks at apo MD starting structures. Stage 7 analyzes the production ligand-bound MD trajectories.

**Shared utilities**
label_cleaner.py (in folders 02, 03, 04) — normalizes AlphaFold-style filenames into short run labels. Identical across all three; kept per-folder so each is self-contained and copy-pasteable to a new run directory, matching how the project already duplicates it.

apo_utils.py (folder 06) and md_utils.py (folder 07) define the same curated focus-residue set independently — keep them in sync if that residue selection changes; see the note in each README.

**Finding a script's active, in-place working copy**
Since these are templates, if you need the actual copy tied to a specific run's results (not just the template), look in the project's native layout:
- 1_MVP-PreDock/{Rank1,Rank1-3,All_Rank1-3}/1_LigandSphere/<Ligand>/{Nonrelaxed,Relaxed,Combined}/
- 1_MVP-PreDock/{Rank1,Rank1-3,All_Rank1-3}/2_GlobalMatrix/{Nonrelaxed,Relaxed,Combined}/
- 1_MVP-PreDock/{Rank1,Rank1-3}/3_ResidueMatrix/{Nonrelaxed,Relaxed}/
- 1_MVP-PreDock/{Rank1,Rank1-3,All_Rank1-3}/4_Hole2/
- 2.5_Analysis/Apo-PocketGeometry/
- 4.5_Analysis/MD-LigandAnalysis/
- 3_Docking/
