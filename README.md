# PL-Dock Runner

Public web interface for the PyRx molecular docking pipeline.
Submit a PDB ID and ligand residue name → AutoDock Vina runs on a private server → download results.

## Usage (end user)

1. Open `https://jong-liu.github.io/PL-dock-runner`
2. Log in with your username and password
3. Enter a PDB ID and ligand resname → click **Run Docking**
4. When complete, download the results ZIP directly from the page

## Output files (inside ZIP artifact)

| File | Description |
|------|-------------|
| `complex_best.pdb` | Receptor + best-scored pose (open in PyMOL) |
| `scores.csv` | Affinity for all 9 poses (kcal/mol) |
| `score_chart.png` | Bar chart of pose scores |
| `vina_log.txt` | Full AutoDock Vina output log |
| `summary.json` | Machine-readable result summary |

## License

Web interface: MIT. Core docking engine: see [AutoDock Vina](https://github.com/ccsb-scripps/AutoDock-Vina).
