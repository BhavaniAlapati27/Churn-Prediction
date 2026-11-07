# Customer Churn Prediction MLOps — Labs 1–11

This package reconstructs the supplied flat-file handout into its original folder layout, scoped through Lab 11. Lab 12 monitoring/drift/retraining scripts and Lab 13 workflows/docs are intentionally omitted. Suspicious mapping entries are not copied as filenames.

## Setup (Windows PowerShell)

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -r requirements-api.txt
```

Use the project README and `lab11_complete_flow.md` for the staged workflow. Docker and Kubernetes stages require Docker Desktop and a working Kubernetes cluster; they cannot be verified merely by creating the files. Do not commit `.env`, credentials, virtual environments, MLflow run stores, or local databases.
