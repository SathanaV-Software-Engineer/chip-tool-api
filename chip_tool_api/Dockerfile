FROM ubuntu:24.04

# Install dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    wget \
    unzip \
    git \
    libssl-dev \
    libavahi-compat-libdnssd-dev \
    curl \
    build-essential \
    iproute2 \
    avahi-daemon \
    libglib2.0-dev \
    sudo \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Download prebuilt ARM64 chip-tool
RUN wget -O chip-tool_arm64 https://github.com/nrfconnect/sdk-connectedhomeip/releases/download/v3.0.0/chip-tool_arm64 && \
    chmod +x chip-tool_arm64 && \
    mkdir -p ./connected_home_ip/out/chip-tool-linux && \
    mv chip-tool_arm64 ./connected_home_ip/out/chip-tool-linux/chip-tool


# Copy application code
COPY connected_home_ip/ ./connected_home_ip/
COPY run.sh /run.sh

# Make run script executable
RUN chmod +x /run.sh

# Set up virtual environment and install Flask dependencies
RUN python3 -m venv /opt/venv && \
    . /opt/venv/bin/activate && \
    pip install --no-cache-dir flask flask-cors

# Set virtualenv path in ENV
ENV PATH="/opt/venv/bin:$PATH"

# Expose Flask port
EXPOSE 6000

# Set the default command
CMD ["/run.sh"]
