# MLOps Docker Labs: Wildfire Data Pipeline

## 🚀 Project Overview
This project is a containerized MLOps data pipeline designed to ingest, process, and validate wildfire and weather data. By leveraging **Apache Airflow** for workflow orchestration and **Docker** for environment consistency, the pipeline ensures reproducible and scalable data engineering processes.

As part of the Data Pipeline Team, my specific focus was on **Data Validation, Anomaly Detection, and Infrastructure Troubleshooting**.

## 🛠️ Tech Stack
* **Orchestration:** Apache Airflow
* **Containerization:** Docker, Docker Compose
* **Database:** PostgreSQL (Airflow Metadata)
* **Data Processing & Geospatial:** Python 3.10, Pandas, NumPy, GeoPandas, H3, GDAL
* **Data Quality:** Great Expectations

## 🔧 Key Modifications & Implementations
During the development and deployment phase, I was responsible for resolving critical environment build failures and implementing data quality gates:

1. **Dockerfile Refactoring & Virtual Environment Integration:**
   * **Issue:** The original multi-stage Docker build caused persistent `ModuleNotFoundError` issues (specifically with geospatial libraries like `h3` and `geopandas`) because the system Python could not locate packages installed via `--prefix`.
   * **Solution:** Redesigned the `Dockerfile` to implement a dedicated Python Virtual Environment (`venv`) inside the container. This completely isolated the dependencies and ensured all modules were correctly linked and accessible to Airflow.
2. **Dependency Management & Resolution:**
   * Resolved package version conflicts in `requirements.txt` (e.g., duplicate `h3` requests).
   * Added missing PostgreSQL adapters (`psycopg2-binary`) to successfully initialize the Airflow metadata database.
3. **Data Validation Logic (Bohan's Role):**
   * Designed and implemented the `validate_schema.py` and `detect_anomalies.py` modules.
   * Utilized `great-expectations` to enforce data quality rules based on `schema_config.yaml` (e.g., verifying temperature ranges and fire intensity metrics) before downstream model training.

## 🏃‍♂️ How to Run the Pipeline Locally

### 1. Prerequisites
Ensure you have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running on your machine.

### 2. Setup Dummy GCP Key
To avoid volume mounting errors during initialization, create a dummy Google Cloud service account key in the root directory:
```bash
echo "{}" > gcp-key.json
