# Entorno general de ciencia de datos (Python 3.12, igual que Google Colab)
FROM python:3.12-slim

# libgomp1: paralelismo de XGBoost/LightGBM | build-essential: paquetes que compilan | git: control de versiones
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential libgomp1 git curl \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /work
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r /tmp/requirements.txt

EXPOSE 8888
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", \
     "--ServerApp.token=", "--ServerApp.password=", "--ServerApp.root_dir=/work"]
