FROM fedora:42

# Install dependencies
RUN dnf update -y && dnf install -y \
    curl ca-certificates gcc gcc-c++ make cmake git \
    snappy-devel protobuf-compiler protobuf-devel \
    && dnf clean all

# Install uv binary
COPY --from=ghcr.io/astral-sh/uv:0.10.9 /uv /uvx /usr/local/bin/

# Setup Python via uv
ENV UV_PYTHON_INSTALL_DIR=/python \
    UV_PYTHON_PREFERENCE=only-managed \
    PATH="/workspace/.venv/bin:$PATH"

# Pre-install Python version from your project file
COPY .python-version /tmp/.python-version
RUN cd /tmp && uv python install

# Setup the Dev User
RUN useradd -m dev-pycharm && \
    chown -R dev-pycharm:dev-pycharm /python

USER dev-pycharm
WORKDIR /workspace