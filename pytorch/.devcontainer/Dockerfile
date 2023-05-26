FROM python:slim AS base

FROM base AS pip-installs

RUN python -m venv /opt/torchenv
ENV PATH="/opt/torchenv/bin:$PATH"

COPY requirements.txt /tmp/pip-tmp/
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM base
COPY --from=pip-installs /opt/torchenv /opt/torchenv
ENV PATH="/opt/torchenv/bin:$PATH"
