FROM python:3.11-slim-bookworm AS base

FROM base AS pip-installs

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*
COPY requirements.txt /tmp/pip-tmp/
RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM base
COPY --from=pip-installs /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=pip-installs /usr/local/bin /usr/local/bin