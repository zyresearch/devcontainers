FROM python:slim-bullseye AS base

FROM base AS jax-installs
RUN python -m venv /opt/jaxenv
ENV PATH="/opt/jaxenv/bin:$PATH"

RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir \
    -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html \
    absl-py \
    clu \
    dm-haiku \
    flax \
    jax[cuda12_pip] \
    optax \
    orbax

FROM base AS torch-installs
RUN python -m venv /opt/torchenv
ENV PATH="/opt/torchenv/bin:$PATH"

RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir \
    -f https://download.pytorch.org/whl/cu118 \
    torch \
    torchaudio \
    torchvision

FROM base
COPY --from=jax-installs /opt/jaxenv /opt/jaxenv
COPY --from=torch-installs /opt/torchenv /opt/torchenv
