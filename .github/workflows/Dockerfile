FROM ubuntu:22.04

# Install required packages
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    git \
    curl \
    wget \
    unzip \
    xvfb \
    libgl1-mesa-glx \
    libgl1-mesa-dri \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
RUN pip3 install requests pyyaml

# Create necessary directories
RUN mkdir -p /app/workflow

# Copy workflow files
COPY render-replay.yml /app/workflow/
COPY scripts/ /app/workflow/scripts/

# Set working directory
WORKDIR /app/workflow

# Set entrypoint to run the workflow
ENTRYPOINT ["python3", "/app/workflow/scripts/run_workflow.py"] 