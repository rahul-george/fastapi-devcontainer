# Base image
FROM python:3.11-slim

# Install system dependencies including git, curl, Docker CLI prerequisites
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    git \
    apt-transport-https \
    ca-certificates \
    gnupg \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Install Docker CLI (latest stable)
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && apt-get install -y docker-ce-cli

# Install Docker Compose CLI plugin (v2)
RUN curl -L "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Install pip dependencies
# COPY requirements.txt /workspace/requirements.txt
# RUN pip install --upgrade pip && pip install -r /workspace/requirements.txt

# Install Poetry (official method)
RUN curl -sSL https://install.python-poetry.org | python3 - && \
    ln -s /root/.local/bin/poetry /usr/local/bin/poetry

# Configure Poetry to not create virtual environments inside containers
RUN poetry config virtualenvs.create false

# Set working directory
WORKDIR /workspace

# Copy project files and install dependencies
COPY pyproject.toml poetry.lock* /workspace/

RUN poetry install --no-root

# (Optional) copy the rest of the app code later to benefit from Docker layer caching
COPY . /workspace
