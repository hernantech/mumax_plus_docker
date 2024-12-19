ARG CUDA_VERSION=12.1.1
FROM nvidia/cuda:${CUDA_VERSION}-devel-ubuntu22.04

# Avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    python3.8 \
    python3.8-dev \
    python3-pip \
    python3.8-venv \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy the mumax+ source code from submodule
COPY src/mumaxplus ./mumaxplus

# Initialize and update submodules within mumaxplus
RUN cd mumaxplus && \
    git submodule init && \
    git submodule update

WORKDIR /app/mumaxplus

# Create Python virtual environment
RUN python3.8 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Install Python dependencies
RUN pip install --upgrade pip && \
    pip install conda-pack

# Create and activate conda environment
RUN conda env create -f environment.yml && \
    conda init bash

# Add CUDA to PATH
ENV PATH="/usr/local/cuda/bin:${PATH}"
ENV LD_LIBRARY_PATH="/usr/local/cuda/lib64:${LD_LIBRARY_PATH}"

COPY docker/scripts/entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

# Build the project
RUN conda run -n mumaxplus pip install -ve .

# Verify CUDA installation
RUN nvcc --version

ENTRYPOINT ["entrypoint.sh"]
CMD ["conda", "run", "-n", "mumaxplus", "python3"]