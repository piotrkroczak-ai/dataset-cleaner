# User Guide

> A functional overview of AI Data Cleaner for end users and evaluators.

---

## Overview

AI Data Cleaner is a Streamlit web application that guides you through a structured, auditable data cleaning workflow. It is designed for data professionals who need to clean tabular datasets safely, with full visibility into what was changed and why.

---

## Getting Started

1. Log in with your credentials
2. Upload a CSV file using the upload area
3. Follow the 6-step guided workflow (see below)

Your original file is protected from the moment it is uploaded — all operations work on a separate copy.

---

## The 6-Step Guided Workflow

The application provides six sequential workflow buttons that walk you through the process:

### Step 1 — Calibration *(optional)*
Load a ground-truth specification file if you want to measure the cleaning quality against known expected results. When enabled, the application will compare its detection and cleaning output against your spec and produce a calibration report.

### Step 2 — D.S.E. (Dataset Safety & Exploration)
View side-by-side information about your original dataset and your working copy. Confirms that the safety boundary is active. Shows schema, row count, and basic statistics.

### Step 3 — S.A.R. (Structured Analysis Report)
Run the AI-powered analysis. The application examines your dataset, classifies detected issues by type (Type 1 / 2 / 3), and produces a structured analysis report you can export as `.txt`.

This is read-only — nothing is modified at this step.

### Step 4 — Heatmap
Visualize the confidence state of your dataset before cleaning. Each cell is colored by its confidence score:

| Confidence | Meaning |
|---|---|
| > 95% | Confident — no action needed |
| 85–95% | Low risk — acceptable, monitor |
| 55–85% | Review required — flag for attention |
| ≤ 55% | Suspicious — likely error |

### Step 5 — Clean Interface
Configure and run the cleaning. Choose which error types to address (Type 1, Type 2, Type 3 independently), set any required parameters (date formats, thresholds), and execute. The heatmap refreshes after cleaning to show the confidence improvement.

Two modes are available:
- **Default mode** — safe presets, recommended for most use cases
- **Advanced mode** — full control over each method and parameter

### Step 6 — Clean Summary
Review the cleaning results: KPIs showing before/after quality scores, a summary of what was changed, and export options for the cleaned dataset (`.csv`) and the cleaning report (`.txt`).

---

## AI Features

### AI Analysis
Triggered as part of the S.A.R. step — uses Azure OpenAI to analyze the dataset structure, detect issues, and produce structured recommendations. This is what populates the analysis report.

### AI Data Assistant
A conversational assistant accessible throughout the session. You can ask questions about your dataset, the detected issues, the cleaning results, or the reports. The assistant can also be guided with slash commands:

| Command | Purpose |
|---|---|
| `/help` | List all available commands |
| `/taxonomy` | Explain the Type 1/2/3 error classification |
| `/reports` | Explain the available reports and how to use them |
| `/calibration` | Explain the calibration mode and how to interpret results |
| `/quality` | Explain the confidence score and quality KPIs |
| `/pipeline` | Explain the full application flow |
| `/diagnose` | Non-destructive diagnosis of current application state |
| `/nextsteps` | Suggest 3–5 next actions based on current state |
| `/glossary` | Definitions of product-specific terms |
| `/documentation` | Summary of available documentation |

---

## Output Files

| File | When produced | Contents |
|---|---|---|
| `analysis_report.txt` | After S.A.R. step | Detected errors by type, counts, schema issues |
| `cleaning_report.txt` | After cleaning | What was changed, per-column summary, KPIs |
| `cleaned_dataset.csv` | After cleaning | The cleaned working copy of your dataset |
| `calibration_report.txt` | When calibration enabled | Detection coverage, cleaning efficiency vs. ground truth |

---

## Data Safety

- **Your original file is never modified.** It is protected at the moment of upload.
- You can always re-export the original at any time.
- All cleaning operations target the working copy only.
- If something looks wrong after cleaning, the original is your rollback point.

---

## Tips

- Run calibration before analysis if you have a ground-truth spec — it will be used in the reports.
- Use the heatmap before and after cleaning to visually confirm the confidence improvement.
- Export the analysis report before cleaning — it gives you the pre-cleaning baseline.
- If the UI appears stale after a long session, refresh the page (your session state resets on upload).
