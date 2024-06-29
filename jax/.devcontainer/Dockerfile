FROM python:3.12-slim AS base

FROM base AS pip-installs

RUN apt-get update && apt-get install -y --no-install-recommends \
    git build-essential cmake \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir jax[cuda12]

RUN python -m pip install --no-cache-dir \
    absl-py \
    clu@git+https://github.com/google/CommonLoopUtils.git \
    dill \
    dm-haiku \
    flax@git+https://github.com/google/flax.git \
    jaxline \
    ml-collections \
    numpy \
    orbax-checkpoint \
    orbax-export \
    tensorflow-cpu \
    tensorflow-datasets

FROM base

COPY --from=pip-installs /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=pip-installs /usr/local/bin /usr/local/bin