# ds-env — Entorno de ciencia de datos (Docker)

Python 3.12 (misma versión que Google Colab) + JupyterLab, pandas, scikit-learn,
XGBoost, LightGBM, imbalanced-learn, SHAP, statsmodels, Optuna, Kaggle CLI y python-pptx.
Las versiones exactas instaladas están en `versiones.txt`.

## Uso diario
```bash
cd ds-env
docker compose up -d      # iniciar  -> abrir http://localhost:8888
docker compose stop       # detener
```
La carpeta de arriba de `ds-env` (la raíz del repo) aparece dentro de Jupyter como `/work`, así que los
notebooks y datos se guardan directamente en tu Mac.

## Desplegar en otro computador
1. Copia `ds-env/` donde quieras (por ejemplo dentro de la carpeta del curso).
2. Por defecto Jupyter muestra la carpeta **de arriba** de `ds-env`. Si quieres otra,
   crea un archivo `.env` junto a `docker-compose.yml` con, por ejemplo:
   `PROYECTO=/ruta/a/Master`
3. `docker compose up -d --build`

Los notebooks buscan solos la carpeta `data/` (con `train.csv`) en su carpeta o en
cualquiera de las de arriba, y crean `artefactos/` y `submissions/` junto al notebook
si no existen. Basta con que `data/` esté en algún nivel por encima del notebook.

## Agregar una librería
1. Añádela a `requirements.txt`
2. `docker compose up -d --build`

## Kaggle (opcional)
Descarga tu `kaggle.json` desde kaggle.com → Settings → API → *Create New Token*
y guárdalo en `~/.kaggle/kaggle.json`. Dentro del contenedor quedará disponible el comando `kaggle`.

## Llevar notebooks a Colab / Databricks
Los notebooks usan una variable `DATA_DIR` al inicio para la ruta de los datos;
solo hay que cambiarla según el entorno (local, `/content/` en Colab o un Volume en Databricks).
