ARG BASE_IMAGE=quay.io/jupyter/scipy-notebook:latest
ARG version=20

FROM $BASE_IMAGE

RUN pip install --no-cache-dir "marimo[recommended,sql,lsp,mcp]" uv \
        jupysql duckdb-engine notebook-intelligence jupyterlab-lsp \
        jupyter-marimo-proxy duckdb
RUN fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER root

# RUN apt-get update && apt install nodejs --yes
RUN apt-get update && apt install curl --yes
RUN curl -sL https://deb.nodesource.com/setup_24.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt install nodejs --yes
USER $NB_UID

# RUN mamba install --yes nodejs "marimo[recommended,sql,lsp,mcp]" jupysql duckdb-engine jupyterlab-lsp && \
#     mamba clean --all -f -y
# RUN pip install --no-cache-dir notebook-intelligence duckdb
# RUN fix-permissions "${CONDA_DIR}" && \
#     fix-permissions "/home/${NB_USER}"
