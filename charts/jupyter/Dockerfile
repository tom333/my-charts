ARG BASE_IMAGE=quay.io/jupyter/scipy-notebook:latest
FROM $BASE_IMAGE

RUN pip install --no-cache-dir marimo jupyter-marimo-proxy
RUN fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
