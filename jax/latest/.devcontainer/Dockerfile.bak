FROM python:3.11-slim-bookworm AS py

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*
COPY requirements.txt /tmp/pip-tmp/
RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

FROM nvidia/cuda:12.1.1-cudnn8-devel-ubuntu22.04 AS lib

FROM python:3.11-slim-bookworm

COPY --from=py /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=py /usr/local/bin /usr/local/bin

COPY --from=lib /usr/local/cuda/compat /usr/local/cuda/compat
COPY --from=lib \
    /usr/local/cuda/lib64/libcudart.so.12 \
    /usr/local/cuda/lib64/libcublas.so.12 \
    /usr/local/cuda/lib64/libcublasLt.so.12 \
    /usr/local/cuda/lib64/libcufft.so.11 \
    /usr/local/cuda/lib64/libcufftw.so.11 \
    /usr/local/cuda/lib64/libcupti.so.12 \
    /usr/local/cuda/lib64/libnvperf_*.so \
    /usr/local/cuda/lib64/libcusolver.so.11 \
    /usr/local/cuda/lib64/libcusolverMg.so.11 \
    /usr/local/cuda/lib64/libcusparse.so.12 \
    /usr/local/cuda/lib64/libnvJitLink.so.12 \
    /usr/local/cuda/lib64/
COPY --from=lib \
    /usr/lib/x86_64-linux-gnu/libcudnn.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_adv_infer.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_adv_train.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_cnn_infer.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_cnn_train.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_ops_infer.so.8 \
    /usr/lib/x86_64-linux-gnu/libcudnn_ops_train.so.8 \
    /usr/local/cuda/lib64/
COPY --from=lib /usr/local/cuda/bin/ptxas /usr/local/cuda/bin/ptxas

RUN echo "/usr/local/cuda/lib64" >> /etc/ld.so.conf.d/cuda.conf \
    && ldconfig

ENV PATH=/usr/local/cuda/bin:$PATH

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
