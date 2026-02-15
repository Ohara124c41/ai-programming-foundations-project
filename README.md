# AIOps Data Workflow (P1)

## Project Description
This project builds a complete, reproducible data workflow using OpenRCA Telecom telemetry data. The notebook covers ingestion, cleaning, exploratory analysis, visualization, and a lightweight fairness-risk screening. The goal is to produce a reusable foundation for later ML/DL/agentic RCA work.

## What I Built
- `data_workflow.ipynb`: end-to-end data workflow notebook
- `requirements.txt`: Python dependencies for reproducible execution
- `module_summary.pdf`: written report with citations

## Dataset
- Name: OpenRCA (Telecom telemetry)
- Link: https://github.com/microsoft/OpenRCA

## How To Run
1. Create and activate a virtual environment (WSL/Ubuntu example):
```bash
python3 -m venv .venv
source .venv/bin/activate
```
2. Install dependencies:
```bash
pip install -r requirements.txt
```
3. Launch Jupyter:
```bash
jupyter notebook
```
4. Open and run:
`data_workflow.ipynb`

## Reproducibility Notes
- The notebook supports local OpenRCA files under `data/openrca/...`.
- To refresh dependency locking before submission:
```bash
pip freeze > requirements.txt
```

## Reflection (Rubric Items)
### Bias Awareness
Poor cleaning can introduce bias by treating source-specific structural missingness as random missingness and by over-imputing sparse fields. This workflow keeps source context (`metric_source`) and explicitly discusses proxy-label bias risks.

### Future Integration: ML Workflow Changes
Before predictive modeling, this workflow should add source-aware feature selection, leakage checks, and strict time-based train/validation splits.

### Future Integration: Neural Network Preparation
For neural models, features should be normalized with source-aware schemas, heavy tails should be transformed robustly, and data should be packaged into stable train/validation/test artifacts.

### Future Integration: Agentic Automation Potential
Agentic pipelines can automate data checks, feature refreshes, drift detection, and RCA triage, but should preserve auditability with versioned configs, commits, and reproducible environment files.
