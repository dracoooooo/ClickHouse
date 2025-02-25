FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# 添加 LLVM 仓库（手动方式而不是使用脚本）
RUN apt-get update && apt-get install -y \
    gnupg wget ca-certificates apt-transport-https \
    && wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - \
    && echo "deb http://apt.llvm.org/jammy/ llvm-toolchain-jammy-18 main" > /etc/apt/sources.list.d/llvm.list \
    && apt-get update

# 安装基本工具和依赖
RUN apt-get install -y \
    build-essential \
    curl \
    git \
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
    openssh-client \
    sudo \
    unzip \
    vim \
    tzdata \
    # 安装 Clang-18
    clang-18 \
    clangd-18 \
    lld-18 \
    # 安装中文语言支持
    fonts-noto-cjk \
    language-pack-zh-hans \
    && rm -rf /var/lib/apt/lists/*

# 设置中文环境
ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN:en
ENV LC_ALL=zh_CN.UTF-8

# 设置 clang 为默认编译器
RUN update-alternatives --install /usr/bin/clang clang /usr/bin/clang-18 100 \
    && update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-18 100 \
    && update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-18 100 \
    && update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100 \
    && update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++ 100

# 创建非root用户
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# 使用 ccache 加速构建（可选但推荐）
RUN apt-get update && apt-get install -y ccache \
    && rm -rf /var/lib/apt/lists/*

# 复制常用脚本助手到容器中
RUN echo '#!/bin/bash\ncd /workspaces/ClickHouse\nmkdir -p build\ncd build\ncmake -D CMAKE_BUILD_TYPE=Debug -D ENABLE_CCACHE=ON ..\nninja -j $(nproc) clickhouse-server clickhouse-client' > /usr/local/bin/build-clickhouse \
    && chmod +x /usr/local/bin/build-clickhouse

# 设置工作目录
WORKDIR /workspaces/ClickHouse

# 切换到非root用户
USER $USERNAME
