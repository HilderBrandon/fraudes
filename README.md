# Modelos de clasificación para detección de fraude bancario

Actividad de **Métodos Supervisados (S2)** — Maestría, Universidad Santo Tomás.
Competencia Kaggle: [Modelos de Fraude 2026](https://www.kaggle.com/competitions/modelos-de-fraude-2026) · **Métrica:** AUC-ROC.

El objetivo es ordenar a los clientes por su probabilidad de fraude, siguiendo la
metodología de los códigos de clase: ingeniería de variables (cuadrados, interacciones,
razones, interacciones categóricas con Kruskal-Wallis), selección de variables con XGBoost,
búsqueda de hiperparámetros con `GridSearchCV` (k = 5), análisis de estabilidad y
predicción sobre `test` para la entrega en Kaggle.

## Estructura

```
.
├── ds-env/                 # Entorno Docker (Python 3.12 + JupyterLab + librerías de ML)
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── requirements.txt
│   └── versiones.txt       # versiones exactas instaladas
├── data/                   # aquí van train.csv, test.csv y sample_submission.csv (no se versionan)
└── fraude/
    ├── 01_EDA_ingenieria_variables.ipynb   # exploración, limpieza y construcción de la base de modelamiento
    ├── 02_Regresion_Logistica.ipynb
    ├── 03_Random_Forest.ipynb
    ├── 04_XGBoost.ipynb
    ├── 05_Ensamble.ipynb                    # voto suave entre los mejores modelos
    └── 06_Comparacion_modelos.ipynb         # comparación, estabilidad y selección final
```

Al ejecutarse, los notebooks crean `fraude/artefactos/` (bases `.parquet`, modelos `.pkl`
y resultados `.json`) y `fraude/submissions/` (CSV para Kaggle). Esas carpetas no se
suben al repositorio porque pesan cientos de MB y se regeneran corriendo los notebooks.

## Cómo reproducirlo

1. **Clonar el repositorio**
   ```bash
   git clone <url-del-repo>
   cd modelos-fraude-2026
   ```
2. **Descargar los datos** desde la pestaña *Data* de la competencia (hay que aceptar sus
   reglas) y guardar `train.csv`, `test.csv` y `sample_submission.csv` en `data/`.
   Con la CLI de Kaggle:
   ```bash
   kaggle competitions download -c modelos-de-fraude-2026 -p data && unzip data/*.zip -d data
   ```
3. **Levantar el entorno**
   ```bash
   cd ds-env
   docker compose up -d --build
   ```
   Abrir <http://localhost:8888>. La raíz del repositorio aparece en Jupyter como `/work`.
4. **Ejecutar los notebooks en orden** (`01` → `06`). Cada uno lee los artefactos del anterior.

Los notebooks encuentran solos la carpeta `data/` y también funcionan en **Google Colab**
(`/content/data/`) o **Databricks** (ajustar la ruta del Volume en la primera celda).
Más detalles del entorno en [`ds-env/README.md`](ds-env/README.md).

## Resultados (validación cruzada, k = 5)

| Modelo | AUC CV (media) | Desv. |
|---|---|---|
| XGBoost | 0.933 | 0.002 |
| Ensamble árboles (RF + XGB) | 0.931 | 0.002 |
| Random Forest | 0.930 | 0.004 |
| Regresión Logística | 0.924 | 0.002 |

Un hallazgo clave: enviar `predict()` (clases 0/1) en lugar de `predict_proba()` baja el
AUC a ~0.68, porque el AUC necesita un *ranking* de probabilidades.

## Requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (o Python 3.12 con `pip install -r ds-env/requirements.txt`)
- Cuenta de Kaggle para descargar los datos
