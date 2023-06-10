ARG TORCH_ENV_DIR="/opt/torch"

FROM mambaorg/micromamba:jammy AS builder
ARG TORCH_ENV_DIR

USER root
RUN apt-get update apt-get install -y --no-install-recommends
    git \
    && rm -rf /var/lib/apt/lists/*

RUN micromamba create -p ${TORCH_ENV_DIR} -c conda-forge \
    python pip \
    && micromamba clean -ya

COPY requirements.txt /tmp/pip-tmp/
RUN ${TORCH_ENV_DIR}/bin/python -m pip install --no-cache-dir --upgrade pip \
    && ${TORCH_ENV_DIR}/bin/python -m pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM ubunt:latest
ARG TORCH_ENV_DIR

COPY --from=builder --chmod=777 ${TORCH_ENV_DIR} ${TORCH_ENV_DIR}
ENV PATH=${TORCH_ENV_DIR}/bin:$PATH

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility