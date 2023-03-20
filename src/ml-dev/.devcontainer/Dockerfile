ARG PYTHON_VERSION=3.11
ARG CUDA_MAJOR_VERSION=11
ARG CUDA_MINOR_VERSION=8
ARG CUDA_VERSION=${CUDA_MAJOR_VERSION}.${CUDA_MINOR_VERSION}
ARG CUDNN_MAJOR_VERSION=8

# Official CUDA image for CUDA libraries
FROM nvidia/cuda:${CUDA_VERSION}.0-cudnn${CUDNN_MAJOR_VERSION}-devel-ubuntu22.04 AS lib

# Use micromamba as package manager
FROM mambaorg/micromamba:latest
ARG CUDA_MAJOR_VERSION
ARG CUDA_MINOR_VERSION
ARG CUDA_VERSION=${CUDA_MAJOR_VERSION}.${CUDA_MINOR_VERSION}
ARG CUDNN_MAJOR_VERSION

# CUDA libraries
COPY --from=lib /usr/local/cuda/compat /usr/local/cuda/compat
COPY --from=lib /usr/local/cuda/lib64/stubs/libcuda.so /usr/local/cuda/lib64/stubs/libcuda.so.1
COPY --from=lib \
    /usr/local/cuda/lib64/libcublas.so.$CUDA_MAJOR_VERSION \
    /usr/local/cuda/lib64/libcublasLt.so.$CUDA_MAJOR_VERSION \
    /usr/local/cuda/lib64/libcudart.so.${CUDA_MAJOR_VERSION}.0 \
    /usr/local/cuda/lib64/libcufft.so.10 \
    /usr/local/cuda/lib64/libcufftw.so.10 \
    /usr/local/cuda/lib64/libcuinj64.so.$CUDA_VERSION \
    /usr/local/cuda/lib64/libcupti.so.$CUDA_VERSION \
    /usr/local/cuda/lib64/libcurand.so.10 \
    /usr/local/cuda/lib64/libcusolver.so.$CUDA_MAJOR_VERSION \
    /usr/local/cuda/lib64/libcusolverMg.so.$CUDA_MAJOR_VERSION \
    /usr/local/cuda/lib64/libcusparse.so.$CUDA_MAJOR_VERSION \
    /usr/local/cuda/lib64/libnvrtc-builtins.so.$CUDA_VERSION \
    /usr/local/cuda/lib64/libnvrtc.so.$CUDA_MAJOR_VERSION.2 \
    /usr/local/cuda/lib64/
COPY --from=lib \
    /usr/lib/x86_64-linux-gnu/libcudnn.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_adv_infer.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_adv_train.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_cnn_infer.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_cnn_train.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_ops_infer.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/libcudnn_ops_train.so.$CUDNN_MAJOR_VERSION \
    /usr/lib/x86_64-linux-gnu/
COPY --from=lib /usr/local/cuda/bin/ptxas /usr/local/cuda/bin/ptxas

ENV PATH=/usr/local/cuda/bin:$PATH
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility

USER root

# Tensorflow 2
RUN micromamba create --yes --name tf2 --channel conda-forge \
    python=3.11 \
    && micromamba clean --all --yes \
    && /opt/conda/envs/tf2/bin/pip install --no-cache-dir tensorflow==2.12.0rc1

# Pytorch
RUN micromamba create --yes --name pytorch --channel conda-forge \
    python=3.11 \
    && micromamba clean --all --yes \
    && /opt/conda/envs/pytorch/bin/pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu118 

# Jax
RUN micromamba create --yes --name jax --channel conda-forge \
    python=3.11 \
    && micromamba clean --all --yes \
    && /opt/conda/envs/jax/bin/pip install --no-cache-dir "jax[cuda]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html 
