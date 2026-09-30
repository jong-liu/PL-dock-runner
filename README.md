# PL-Dock Runner

Public web interface for the PyRx molecular docking pipeline.
Submit a PDB ID and ligand residue name → AutoDock Vina runs on a private server → download results.

## Architecture

```
Browser (GitHub Pages)
  └─ Flask API (140.114.98.97:8443)
       └─ GitHub Actions workflow_dispatch
            └─ Self-hosted Runner (Ubuntu server)
                 └─ Private pyrx repo (core pipeline)
                      └─ Artifacts uploaded back to this repo
```

## Setup (server admin)

### 1. Register self-hosted runner on the Ubuntu server

```bash
# On 140.114.98.97 — get exact commands from:
# GitHub → PL-dock-runner → Settings → Actions → Runners → New self-hosted runner
mkdir ~/actions-runner && cd ~/actions-runner
# paste the ./config.sh and ./run.sh commands from GitHub
```

### 2. Run as a systemd service (persistent)

```bash
sudo ./svc.sh install
sudo ./svc.sh start
sudo systemctl status actions.runner.*
```

### 3. Verify the private repo is cloned on the server

```bash
ls /home/jong/pyrx/pyrx/scripts/run_pipeline.py  # must exist
conda activate pyrx && python --version           # must work
```

### 4. Start the backend API server

```bash
sudo systemctl start pyrx-api
sudo systemctl status pyrx-api
```

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
