FROM nvidia/cuda:13.3.1-devel-ubuntu24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy
ENV PATH="/opt/venv/bin:$PATH"

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    build-essential \
    python3.12 \
    python3.12-dev \
    && rm -rf /var/lib/apt/lists/*

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Create isolated Python environment
RUN uv venv /opt/venv --python /usr/bin/python3.12

# Install PyTorch CUDA 13.0
RUN UV_HTTP_TIMEOUT=600 \
    UV_HTTP_RETRIES=10 \
    uv pip install \
    --python /opt/venv/bin/python \
    torch==2.13.0 \
    torchvision==0.28.0 \
    --index-url https://download.pytorch.org/whl/cu130 \
    --system-certs

# Install remaining AI dependencies
RUN uv pip install \
    --python /opt/venv/bin/python \
    numpy \
    pandas \
    scikit-learn \
    matplotlib \
    jupyter \
    transformers \
    datasets \
    accelerate \
    safetensors \
    qdrant-client \
    flask

WORKDIR /workspace

VOLUME ["/workspace", "/models", "/datasets"]

EXPOSE 8888

CMD ["python"]
