FROM python:slim AS base

FROM base AS pip-installs

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt /tmp/pip-tmp/
RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM base
COPY --from=pip-installs --chmod=777 /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
