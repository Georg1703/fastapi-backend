# Start from the official Ubuntu image
FROM ubuntu:22.04

# Set environment variables to non-interactive mode to avoid prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install required system packages
RUN apt-get update && apt-get install -y \
    software-properties-common \
    curl \
    git \
    build-essential \
    libssl-dev \
    libffi-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    zlib1g-dev \
    libncurses5-dev \
    libncursesw5-dev \
    liblzma-dev \
    tk-dev \
    lzma \
    wget \
    gzip \
    && rm -rf /var/lib/apt/lists/*

# Add the deadsnakes PPA to get Python 3.12
RUN add-apt-repository ppa:deadsnakes/ppa && apt-get update && apt-get install -y \
    python3.12 \
    python3.12-venv \
    python3.12-dev \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

RUN pip install poetry==1.8.0

RUN poetry config virtualenvs.in-project true

RUN wget https://github.com/evilmartians/lefthook/releases/download/v1.7.14/lefthook_1.7.14_Linux_x86_64.gz \ 
    && gunzip lefthook_1.7.14_Linux_x86_64.gz \
    && mv lefthook_1.7.14_Linux_x86_64 /usr/local/bin/lefthook \
    && chmod +x /usr/local/bin/lefthook

ENV PATH="/root/.local/bin:$PATH"

ENV POETRY_NO_INTERACTION=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

WORKDIR /app

COPY pyproject.toml poetry.lock ./
RUN touch README.md
RUN poetry install && rm -rf $POETRY_CACHE_DIR

COPY . .
