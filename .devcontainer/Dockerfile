ARG IMAGE=clickhouse/binary-builder:latest
FROM ${IMAGE}

# 添加开发工具
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git-extras \
        lldb \
        gdb \
        vim && \
    rm -rf /var/lib/apt/lists/*

# 配置开发环境
RUN mkdir -p /workdir/.cache/{ccache,go-build} && \
    chmod -R 777 /workdir
