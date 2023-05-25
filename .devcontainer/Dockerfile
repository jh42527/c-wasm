FROM mcr.microsoft.com/devcontainers/cpp:0-debian-11

ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="none"

# Optionally install the cmake for vcpkg
COPY ./reinstall-cmake.sh /tmp/

RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
        chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
    fi \
    && rm -f /tmp/reinstall-cmake.sh

RUN apt update
RUN apt install -y build-essential cmake lsb-release

# [Optional] Uncomment this section to install additional vcpkg ports.
# RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# ORTOOLS
WORKDIR /opt/ortools

RUN wget https://github.com/google/or-tools/releases/download/v9.6/or-tools_amd64_debian-11_cpp_v9.6.2534.tar.gz && \
    tar -xvzf or-tools_amd64_debian-11_cpp_v9.6.2534.tar.gz

ENV PATH "$PATH:/opt/ortools/;" ls

# WASMEDGE
WORKDIR /opt/wasmedge

RUN curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash

RUN echo 'source "$HOME/.wasmedge/env"' >> $HOME/.bash_profile

# WASMTIME
WORKDIR /opt/wasmtime

RUN curl https://wasmtime.dev/install.sh -sSf | bash

ENV PATH "$PATH:/root/.wasmtime/bin/;" ls

# EMSDK
WORKDIR /opt

RUN git clone https://github.com/emscripten-core/emsdk.git && \
    cd emsdk && \
    ./emsdk install latest && \
    ./emsdk activate latest

RUN echo 'source "/opt/emsdk/emsdk_env.sh"' >> $HOME/.bash_profile

