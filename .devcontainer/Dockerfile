FROM ubuntu:noble

# 避免安装过程中的交互提示
ENV DEBIAN_FRONTEND=noninteractive

# 安装依赖
RUN apt-get update && apt-get install -y \
    git \
    cmake \
    ccache \
    python3 \
    ninja-build \
    nasm \
    yasm \
    gawk \
    lsb-release \
    wget \
    software-properties-common \
    gnupg \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# 安装 LLVM
RUN bash -c "$(wget -O - https://apt.llvm.org/llvm.sh)"

# 创建非 root 用户
RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid 1000 -s /bin/bash -m vscode \
    && echo vscode ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/vscode \
    && chmod 0440 /etc/sudoers.d/vscode

# 设置默认用户
USER vscode
