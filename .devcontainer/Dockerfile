FROM fedora:42

RUN dnf update -y && \
    dnf install -y \
        git curl gcc gcc-c++ make cmake \
        snappy-devel protobuf-compiler protobuf-devel \
        sudo && \
    dnf clean all

# Install Bazelisk to /usr/local/bin (standard for user-installed binaries)
RUN curl -fSL https://github.com/bazelbuild/bazelisk/releases/latest/download/bazelisk-linux-amd64 \
        -o /usr/local/bin/bazel && \
    chmod +x /usr/local/bin/bazel && \
    /usr/local/bin/bazel --version

ENV PATH="/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:${PATH}"
COPY --from=ghcr.io/astral-sh/uv:0.10.9 /uv /uvx /usr/local/bin/

RUN useradd -m dev-pycharm && \
    echo "dev-pycharm ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/dev-pycharm && \
    chmod 0440 /etc/sudoers.d/dev-pycharm

USER dev-pycharm
ENV UV_PYTHON_INSTALL_DIR=/home/dev-pycharm/.local/share/uv/python

RUN uv python install 3.12.4

WORKDIR /workspace