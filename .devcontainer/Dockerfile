FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# 安装基本工具和依赖
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    wget \
    git \
    gnupg2 \
    lsb-release \
    software-properties-common \
    cmake \
    ninja-build \
    pkg-config \
    libssl-dev \
    zlib1g-dev \
    liblz4-dev \
    libreadline-dev \
    libicu-dev \
    python3 \
    python3-pip \
    gdb \
    lldb \
    openssh-client \
    sudo \
    unzip \
    vim \
    tzdata \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 安装 LLVM 和 Clang
RUN curl -fsSL https://apt.llvm.org/llvm.sh -o /tmp/llvm.sh \
    && chmod +x /tmp/llvm.sh \
    && /tmp/llvm.sh 15 all \
    && rm /tmp/llvm.sh

# 创建非root用户
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# 配置 clangd
RUN ln -s /usr/bin/clangd-15 /usr/bin/clangd

# 配置 clang 和 clang++ 作为默认编译器
RUN update-alternatives --install /usr/bin/cc cc /usr/bin/clang-15 100 \
    && update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++-15 100

# 设置工作目录
WORKDIR /workspaces/ClickHouse

USER $USERNAME
