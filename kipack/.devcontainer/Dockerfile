FROM python:3.11-slim AS base

FROM base AS pip-installs

RUN apt-get update && apt-get install -y --no-install-recommends \
    git build-essential cmake \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir jax[cuda12_pip] -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

RUN python -m pip install --no-cache-dir \
    absl-py \
    cupy-cuda12x \
    dill \
    ml-collections \
    numpy \
    pyfftw \
    tensorflow-cpu \
    tensorflow-datasets

FROM base

COPY --from=pip-installs /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=pip-installs /usr/local/bin /usr/local/bin