# End-to-end-MLOps-System-Churn-Prediction
This repository features an end-to-end MLOps project designed to reduce customer churn for SaaS-Vibe, a cloud-based operations management platform for SMEs. The goal is to bring the churn rate below 2% and prevent acquisition costs that are 5x higher than retention costs.

# Predicción de Churn y Sistema de Alerta Temprana (SaaS-Vibe)

Este proyecto implementa un sistema predictivo de extremo a extremo (End-to-End) para identificar clientes B2B con alto riesgo de abandono (Churn). El objetivo es pasar de datos crudos a una herramienta de toma de decisiones accionable para los equipos de Customer Success.

## Caso de Negocio: SaaS-Vibe
SaaS-Vibe es una plataforma de gestión operativa B2B que enfrenta una tasa de abandono mensual del 4.5%, lo que representa una pérdida mensual de aproximadamente **$45,000 USD en MRR** (Ingreso Mensual Recurrente). 

**El Objetivo:** Construir un sistema de alerta temprana con una ventana de 30 días de anticipación. Debido a restricciones operativas, el equipo de retención solo puede intervenir al 15% de la base de clientes, por lo que el modelo maximiza la precisión (Precision/Recall) en el percentil de mayor riesgo.

## Arquitectura del Sistema y Roadmap

El proyecto está diseñado para evolucionar iterativamente.

**Fase 1: Minimum Viable Product (MVP) - [Estado Actual]**
* **Análisis (EDA):** Limpieza y correlación de más de 20,000 registros transaccionales (uso y tickets de soporte).
* **Modelo Predictivo:** Entrenamiento de un clasificador (XGBoost/Random Forest) para predecir la probabilidad de churn.
* **Servicio (API):** Endpoint construido con FastAPI para inferencia en tiempo real.
* **Interfaz (Frontend):** Dashboard interactivo en Streamlit para el usuario final.

[Raw Data CSVs] ➔ [EDA & Scikit-Learn/XGBoost] ➔ [Model Artifact (.pkl)]
                                                           │
                                                           ▼
[Streamlit UI] ◄━━(REST API JSON)━━ [FastAPI Docker Container]

**Fase 2: MLOps y Cloud (Próximos Pasos)**
* Orquestación de ingesta diaria con Apache Airflow.
* Migración de procesamiento y Feature Store a Databricks.
* Registro y versionado de modelos con MLflow.
* Monitoreo de Data Drift en producción usando Evidently AI.

## Stack Tecnológico
* **Lenguaje:** Python 3.10+
* **Datos y Modelado:** Pandas, NumPy, Scikit-Learn, Jupyter Notebooks.
* **Despliegue Backend:** FastAPI, Uvicorn, Docker.
* **Interfaz de Usuario:** Streamlit.

## Estructura del Directorio

```text
saas-vibe-churn/
├── data/
│   ├── raw/                 # Archivos CSV originales generados
│   └── processed/           # Datos limpios y listos para modelar
├── notebooks/
│   ├── 01_data_generation.ipynb
│   ├── 02_eda_and_cleaning.ipynb
│   └── 03_model_training.ipynb
├── src/
│   ├── api/                 # Código de FastAPI
│   │   ├── main.py
│   │   └── model_loader.py
│   ├── app/                 # Código de Streamlit (Frontend)
│   │   └── ui.py
│   └── models/              # Modelos serializados (.pkl)
├── Dockerfile               # Configuración del contenedor principal
├── docker-compose.yml       # Orquestación local de API + UI
├── requirements.txt         # Dependencias del proyecto
└── README.md

## System Arquitecture

+-----------------------------------------------------------------------------------+
|                                   GITHUB (CI/CD)                                  |
| [ Python Code / Dockerfile / Airflow DAGs ] --(Automated Deployment)-->           |
+-------------------------------+-----------------------+---------------------------+
                                |                       |
                                v                       v
+-------------------------------+----+   +--------------+---------------------------+
|            DATABRICKS              |   |          AMAZON WEB SERVICES (AWS)       |
|                                    |   |                                          |
| +--------------------------------+ |   | +--------------------------------------+ |
| |      Orchestrator (Airflow)    | |   | |          Amazon S3 (Data Lake)       | |
| |   (Triggers daily data sync)   |-+---+->  - /raw_data (New daily CSVs)        | |
| +---------------+----------------+ |   | |  - /model_artifacts (.pkl/.onnx)     | |
|                 |                  |   | +------------------+-------------------+ |
|                 v                  |   |                    |                     |
| +--------------------------------+ |   |                    v                     |
| |          Feature Store         | |   | +------------------+-------------------+ |
| | (Spark - Business Aggregations)|<----|        Amazon EC2 (Production)         | |
| +---------------+----------------+ |   | |                                      | |
|                 |                  |   | | +--------------------------------+   | |
|                 v                  |   | | |    Docker Container (FastAPI)  |   | |
| +--------------------------------+ |   | | | - Fetches latest Model         |   | |
| |             MLflow             | |   | | | - Real-time Churn Inference    |   | |
| | (Model Registry & Experiments) |<======+ +----------------+---------------+   | |
| +--------------------------------+ |   | |                  |                   | |
|                 ^                  |   | |                  v                   | |
|                 |                  |   | | +----------------+---------------+   | |
|                 | (Triggers        |   | | |      Frontend (Streamlit UI)   |   | |
|                 |  Retraining)     |   | | +--------------------------------+   | |
|                 |                  |   | +------------------+-------------------+ |
|                 |                  |   |                    |                     |
|                 |                  |   |                    v                     |
|                 |                  |   | +------------------+-------------------+ |
|                 +------------------+---+--    Monitoring (Evidently AI)         | |
|                                    |   | |   (Evaluates daily Data Drift)       | |
|                                    |   | +--------------------------------------+ |
+------------------------------------+   +------------------------------------------+
