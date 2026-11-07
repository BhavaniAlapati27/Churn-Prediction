# Customer Churn Prediction — MLOps Labs 1–11

This is the reconstructed, runnable-layout version of the supplied flat project package, scoped to **Labs 1 through 11**. Lab 12 monitoring/drift/retraining scripts and Lab 13 automation are intentionally not included.

## What is included

- Data loading, schema validation, preprocessing, and processed-output checks
- Model training, MLflow tracking/registry, evaluation, and quality gates
- Candidate-vs-production lifecycle decisions and inference
- Failure injection, troubleshooting, and recovery exercises
- FastAPI prediction service and API tests
- Docker packaging and Kubernetes deployment manifests for Lab 11

## Repository layout

```text
churn-prediction/
├── api/                 # FastAPI app, schemas, model loader
├── artifacts/           # Validation/evaluation artifacts and reports
├── data/raw/            # Source churn dataset and optional incoming batch
├── data/processed/      # Train/test splits and preprocessing outputs
├── deployment/          # Production model bundle and deployment script
├── docs/                # Architecture, labs 7–11 reports, runbook
├── k8s/                 # Kubernetes Deployment and Service
├── models/               # Training/model artifacts
├── pipelines/            # Labs 3–6 helper scripts
├── src/mlops_pipeline/   # Main numbered pipeline stages through Lab 11
├── tests/                # API, performance, local/container consistency tests
├── Dockerfile
├── requirements.txt
└── requirements-api.txt
```

## Prerequisites

- Python 3.11 recommended
- Git
- Docker Desktop with Linux containers enabled for container stages
- Kubernetes enabled in Docker Desktop and `kubectl` for Kubernetes stages
- MLflow dependencies and model artifacts required by the pipeline stages

## Setup on Windows PowerShell

From the project root:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -r requirements-api.txt
```

If PowerShell blocks activation, use the Python executable directly at `.venv\Scripts\python.exe` or follow your institution's execution-policy guidance. Do not copy an existing virtual environment into this project.

## Labs 1–10: data and model lifecycle

The main pipeline entry point is:

```powershell
python .\src\mlops_pipeline\churn_prediction_pipeline.py
```

The numbered stages in `src/mlops_pipeline/` cover loading data, validation, preprocessing, model training/registration, evaluation, quality gating, lifecycle decisions, inference, failure simulation, and deployment preparation. Run individual stages only after their prerequisite data/artifacts exist. MLflow registry behavior depends on the local tracking/registry configuration and may require setup in a fresh environment.

## Lab 10 API tests

```powershell
pytest tests/ -v
```

The supplied Lab 11 notes report a prior baseline of 29 passing tests. Re-run tests in your environment; results can vary with dependency versions and artifact availability.

Start the API locally:

```powershell
uvicorn api.main:app --host 127.0.0.1 --port 8000
```

Open `http://127.0.0.1:8000/docs` for the interactive API documentation. The service expects the deployment model and preprocessor artifacts to be present.

## Lab 11: Docker and Kubernetes

Build the API image from the project root:

```powershell
docker build -t churn-prediction-api:lab11 .
```

Run it:

```powershell
docker run --rm -p 8000:8000 churn-prediction-api:lab11
```

Then validate `http://127.0.0.1:8000/health`, `http://127.0.0.1:8000/metadata`, and `http://127.0.0.1:8000/docs` (endpoint availability depends on the API implementation).

Apply Kubernetes resources:

```powershell
kubectl apply -f .\k8s\deployment.yaml
kubectl apply -f .\k8s\service.yaml
kubectl get deployments,pods,services
```

Follow `lab11_complete_flow.md`, `docs/lab11_containerization_report.md`, `docs/lab11_deployment_debugging_report.md`, and the reference files in `lab11_commands/` for the ordered Lab 11 stages, troubleshooting, scaling, interruption, and recovery validation.

**Verification note:** this package reconstruction and Python syntax/test checks do not themselves prove that a Docker image builds or a Kubernetes cluster deploys successfully. Execute those stages on a machine with Docker and Kubernetes available and record the actual outputs.

## Reports and documentation

- `docs/ARCHITECTURE.md`
- `docs/LAB7_PIPELINE.md`
- `docs/LAB8_CI_REPORT.md`
- `docs/LAB9_FAILURE_HANDLING_REPORT.md`
- `docs/RUNBOOK.md`
- `docs/lab11_containerization_report.md`
- `docs/lab11_deployment_debugging_report.md`
- `lab11_complete_flow.md`

## Security and repository hygiene

Do not commit credentials, `.env` files, virtual environments, MLflow databases/run stores, or private data. Check `.gitignore` before committing model artifacts or data. Confirm that you have permission to redistribute the supplied dataset and model files.
