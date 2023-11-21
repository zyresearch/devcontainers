FROM python:3.11-slim-bookworm AS base

FROM base as pip-installs
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*
RUN python -m pip install --no-cache-dir --upgrade pip \
    absl-py \
    numpy \
    matplotlib \
    ml-collections \
    scipy \
    torch \
    torchvision

FROM base
COPY --from=pip-installs /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=pip-installs /usr/local/bin /usr/local/bin