FROM python:slim AS base

FROM base AS pip-installs
RUN apt-get update && apt-get install -y --no-install-recommends git \
    && rm -rf /var/lib/apt/lists/*

RUN python -m venv /opt/jaxenv
ENV PATH="/opt/jaxenv/bin:$PATH"

COPY requirements.txt /tmp/pip-tmp/
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM base
COPY --from=pip-installs /opt/jaxenv /opt/jaxenv
ENV PATH="/opt/jaxenv/bin:$PATH"