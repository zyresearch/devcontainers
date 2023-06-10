ARG JAX_ENV_DIR="/opt/jax"

FROM mambaorg/micromamba:jammy AS builder
ARG JAX_ENV_DIR

USER root
RUN apt-get update && apt-get install -y --no-install-recommends
    git \
    && rm -rf /var/lib/apt/lists/*

RUN micromamba create -p ${JAX_ENV_DIR} -c conda-forge \
    python pip \
    && micromamba clean -ya

COPY requirements.txt /tmp/pip-tmp/
RUN ${JAX_ENV_DIR}/bin/python -m pip install --no-cache-dir --upgrade pip \
    && ${JAX_ENV_DIR}/bin/python -m pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM ubuntu:latest
ARG JAX_ENV_DIR

COPY --from=builder --chmod=777 ${JAX_ENV_DIR} ${JAX_ENV_DIR}
ENV PATH=${JAX_ENV_DIR}/bin:$PATH

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility