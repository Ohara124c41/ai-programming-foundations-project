# AIOps Data Workflow (P1)

## Project Description
This project builds a complete, reproducible data workflow using OpenRCA Telecom telemetry data. The notebook covers ingestion, cleaning, exploratory analysis, visualization, and a lightweight fairness-risk screening. The goal is to produce a reusable foundation for later ML/DL/agentic RCA work.

## Project Repository
- https://github.com/Ohara124c41/ai-programming-foundations-project

## What I Built
- `data_workflow.ipynb`: end-to-end data workflow notebook
- `requirements.txt`: Python dependencies for reproducible execution
- `module_summary.pdf`: written report with citations 
=======
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
Poor cleaning can introduce bias in this dataset because structural nulls are source-driven, not random. For example, columns such as `servicename`, `avg_time`, `num`, and `succee_rate` are about 99.94% null in the 50,000-row subset because those fields come from a narrow portion of the merged schemas; imputing them globally would fabricate values for roughly 49,971 rows. Source imbalance is also large (`metric_node` = 25,703 rows vs `metric_app` = 29 rows), so a model can easily learn source identity artifacts instead of failure behavior. The notebook mitigates this by preserving `metric_source`, limiting imputation to lower-missingness columns, and documenting proxy-label limitations in the AIF360 screening section.

### Future Integration: ML Workflow Changes
For this telecom telemetry, future ML workflow changes should include source-aware feature selection and strict time-based splits. Because the data is timestamped within a single operational window, random splitting can leak temporal patterns and inflate validation performance. The workflow should also enforce group-aware evaluation by `metric_source` so model quality is not dominated by high-volume sources like `metric_node`. Finally, leakage checks are needed around derived fields and any future RCA labels to avoid target contamination.

### Future Integration: Neural Network Preparation
Neural-network preparation should explicitly handle distribution shape and schema heterogeneity discovered in EDA. The `value` column is strongly heavy-tailed (skewness about 10.82; kurtosis about 130.77), so robust transforms such as `log1p` plus robust scaling are more appropriate than naive normalization. Extremely sparse source-specific fields should be dropped or modeled through source-conditional architectures rather than dense global imputation. Training artifacts should be packaged into stable, versioned train/validation/test datasets with reproducible preprocessing configs.

### Future Integration: Agentic Automation Potential
Agentic automation can operationalize this workflow by continuously validating data contracts, refreshing feature statistics, and detecting drift by source group and time window. For this dataset, agents should flag structural-null shifts, source-volume shifts (for example sudden `metric_app` increase), and changes in tail behavior of `value`. Agents can also automate RCA triage prompts using the cleaned telemetry context, but every action should remain auditable through versioned configs, reproducible environments, and Git-tracked outputs. This keeps automation useful without sacrificing traceability.
=======
Poor cleaning can introduce bias by treating source-specific structural missingness as random missingness and by over-imputing sparse fields. This workflow keeps source context (`metric_source`) and explicitly discusses proxy-label bias risks.

### Future Integration: ML Workflow Changes
Before predictive modeling, this workflow should add source-aware feature selection, leakage checks, and strict time-based train/validation splits.

### Future Integration: Neural Network Preparation
For neural models, features should be normalized with source-aware schemas, heavy tails should be transformed robustly, and data should be packaged into stable train/validation/test artifacts.

### Future Integration: Agentic Automation Potential
Agentic pipelines can automate data checks, feature refreshes, drift detection, and RCA triage, but should preserve auditability with versioned configs, commits, and reproducible environment files.
