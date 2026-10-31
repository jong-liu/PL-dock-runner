# Changelog

Version history of the **PL-Dock Runner** public frontend (`jong-liu/PL-dock-runner`,
served via GitHub Pages). The docking pipeline and backend API live in separate
repositories. Entries are grouped by session date, newest first; the short hashes
reference commits on `main`.

_Last recorded: 2026-10-31._

## 2026-10-31 — Stuck "Running" jobs self-heal (backend)

Backend only — `pyrx-web-backend`, a separate service that is **not under git**;
recorded here so the project history stays in one place.

- **My Jobs no longer stalls on "Running".** The jobs-list endpoint now lazily
  reconciles any job that has a workflow run but no stored conclusion: it queries
  GitHub once and writes the result back. A run that waits in the queue longer than
  the 60-minute monitor window (e.g. behind a disconnected self-hosted runner) now
  still resolves to Success/Failure on the next refresh. Completion recording was
  factored into a shared helper (`_record_completion`) so the notification email
  still fires exactly once.
- One-off repair: reconciled the 5J5T virtual-screening job that was stuck
  (run `41056960587` → success, best Vina −10.48, top hit 173476815).

## 2026-10-30 — Results panel: per-pose scores + collapsible, pose-tagged interactions

Screening dispatch and wording:

- `40e2f21` Use English **Expand ▾ / Collapse ▲** hints on the interaction sections.
- `8f4c019` Make the workflow's `ligand_resname` optional so **virtual screening can
  dispatch** — an empty value was previously rejected with HTTP 422.

Explainer, dividers, and visual polish:

- `8fba81d` Make the CNN-vs-Vina caret yellow-orange and move its divider below the header.
- `7768897` Add a collapsible **"CNN vs Vina"** explainer (5 points) under the score table.
- `5017a76` Point the H-bond credit link to the live GitHub Pages site.
- `6166491` Use a single divider between interaction sub-sections (no double line).
- `bc65ff7` Drop the divider between the interactions header and its summary line.
- `107f7d5` Add top+bottom separators to the sub-section headers _(revised in `6166491`)_.
- `54216f5` Make the collapsible carets ocean-blue and larger for visibility.
- `0febfa5` Add this CHANGELOG recording the version history.

Feature work:

- `9726376` Show and make clickable **every docked pose**, not just the first 9
  (the pipeline already writes `complex_pose{i}.pdb` for all `num_modes` poses).
- `9fc6102` Collapse the interaction sections (H-bonds, mutation hotspots, contact
  residues) **by default**, each with a prominent "點我展開" expand hint.
- `09601c1` Tag the **Contact residues** heading with the current pose.
- `876a44a` Tag the **Protein–ligand interactions** and **Mutation-site suggestions**
  headings with a "Showing Pose N" badge, mirroring the 3D viewer.
- `d2a8cda` Cap the pose-score table at the first 9 poses _(later lifted in `9726376`)_.
- `4ef818e` Collapse the **pose-score table** by default (click header to expand).
- `a94bf8b` Add a **per-pose score table** (CNN pose score · CNN affinity · Vina
  affinity) with ★ / ◆ best-of markers and a one-click **CSV download**.

## 2026-10-29 — Interaction fingerprint, legible failures, screening, polish

- `04aecb4` Update license section with gnina reference.
- `c87762f` Footer: credit gnina + CNN scoring source (McNutt et al. 2021).
- `fc1e6dc` Accept `.mol` ligand uploads.
- `f42e465` Job Status: show the **failure reason**, not just "Failure — …".
- `1ce1b4c` Protein–ligand **interaction fingerprint** (H-bonds + mutation hotspots),
  integrated from `jong-liu/protein-ligand-HB-interaction`, client-side.
- `e7e62f0` **Virtual-screening** mode (ligand library → ranked hits).
- `89be4d6` Workflow: add screen mode (ligand library) branch.
- `a842ec0` Let users delete their own jobs from My Jobs.

## 2026-10-28 — Dual scoring display and 3D viewer interaction

- `b6f3803` Show ligand carbons in white (`whiteCarbon` colorscheme).
- `2692260` Add Rotate button to the 3D viewer for auto-spin toggle.
- `fadc3de` Click a pose tile to show that pose in the 3D viewer.
- `3028118` Display dual scoring (CNN + Vina) with a methodology note.

## 2026-10-20 — 3D viewer fixes and job routing

- `bb3dbc6` Fix 3D viewer: valid CDN URL, error visibility, robust init.
- `0340eab` Update 3D viewer to use chain L for docked-ligand selection.
- `b347fa3` Fix score display and 3D viewer black screen on the results page.
- `403fb17` Add per-job URL routing (`?job=<id>`) and a 5-item progress log.
- `46e09af` Show Cancelled badge (grey) instead of Failure (red) on cancel.
- `3741b46` Add cancel button and a dedicated progress log to Job Status.

## 2026-10-19 — Accounts, job history, and submission options

- `1c3306a` Add job history, progress timeline, and email-notification UI.
- `f1681d0` Add user account management with admin approval.
- `169971a` Add `num_modes` selector (9/11/13/15) to Advanced Settings.
- `1c7de37` Add file-upload support for receptor and ligand.

## 2026-10-17

- `d44c99b` Fix login overlay not hiding after successful login.

## 2026-10-15 — Tunnel and login fixes

- `066e764` Cleanup README.
- `76cf737` Fix missing base variable in `doLogin`.
- `bc27a0d` Switch API to the ngrok static tunnel URL.
- `7ccd185` Switch `API_BASE` to the Cloudflare Tunnel URL.

## 2026-10-01

- `8e722d7` Switch `API_BASE` to port 443 via an nginx reverse proxy.

## 2026-09-30 — Project rename and hardening

- `51a125a` Rename project to **PL-Dock Runner**.
- `ea23c75` Hardcode API URL; remove the settings panel.
- `b433e94` Hide demo-account credentials from the login page.

## 2026-09-29 — Initial release

- `8bfbe16` Overhaul frontend: local auth, direct download, 3Dmol 3D viewer.
- `b1a4cf5` Initial PyRx Web: GitHub Pages frontend + self-hosted runner workflow.
