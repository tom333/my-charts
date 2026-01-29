ARG MLFLOW_VERSION=v3.9.0

FROM ghcr.io/mlflow/mlflow:${MLFLOW_VERSION}

USER root

# Installer les dépendances système nécessaires pour psycopg2
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Installer les dépendances Python pour PostgreSQL et S3
RUN pip install --no-cache-dir \
    psycopg2-binary \
    boto3

USER 1000

# Port par défaut
EXPOSE 5000

# Commande par défaut (sera overridée par le deployment)
CMD ["mlflow", "server"]
