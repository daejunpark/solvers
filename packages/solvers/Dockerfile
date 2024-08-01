FROM ubuntu:24.04 as builder

# Install necessary packages
RUN apt-get update && apt-get install --no-install-recommends -y \
    curl \
    git \
    build-essential \
    wget \
    unzip \
    autoconf \
    libffi-dev \
    cmake \
    python3 \
    python3-pip \
    python3-venv \
    ninja-build \
    software-properties-common \
    g++ zlib1g-dev libboost-all-dev flex bison

# Install Bitwuzla
WORKDIR /bitwuzla
RUN git clone --depth 1 https://github.com/bitwuzla/bitwuzla . && \
    python3 -m venv .venv && \
    . .venv/bin/activate && \
    python3 -m pip install meson && \
    ./configure.py && \
    cd build && \
    ninja install && \
    rm -rf /bitwuzla

# Create the final image
FROM ubuntu:24.04

# Copy installed files from builder

COPY --from=builder \
    /usr/local/bin/bitwuzla \
    /usr/local/bin/

# Set the default command for the container
WORKDIR /workspace
CMD ["/bin/bash"]
