FROM --platform=linux/amd64 ubuntu:24.04

# Set up repositories
RUN echo "deb http://deb.debian.org/debian bookworm main contrib non-free" > /etc/apt/sources.list
RUN echo "deb http://security.debian.org/debian-security bookworm-security main contrib non-free" >> /etc/apt/sources.list

# Add multiarch support
RUN dpkg --add-architecture amd64

# Install QEMU emulation
RUN apt-get update && apt-get install -y qemu-user-static
RUN update-binfmts --enable

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
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
    ca-certificates \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Download prebuilt ARM64 chip-tool
RUN curl -L -o chip-tool-linux.zip "https://github.com/SathanaV-Software-Engineer/chip-tool-api/releases/download/v3.3.0/chip-tool-linux.zip" && \
    mkdir -p ./connected_home_ip/out/ && \
    unzip chip-tool-linux.zip -d ./connected_home_ip/out/ && \
    chmod +x ./connected_home_ip/out/chip-tool-linux/chip-tool && \
    rm chip-tool-linux.zip


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
