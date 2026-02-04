# Ubuntu 24.04 LTS
FROM ubuntu:24.04

# Avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Basic tools
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    git \
    build-essential \
    bash \
    sudo \
  && rm -rf /var/lib/apt/lists/*

# Create non-root user
RUN useradd -m -s /bin/bash dev \
  && echo "dev ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/dev \
  && chmod 0440 /etc/sudoers.d/dev

USER dev
WORKDIR /home/dev/workspace

# Install nvm
ENV NVM_DIR=/home/dev/.nvm
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash

# Install Node 24 (LTS) and enable corepack (for pnpm/yarn if you ever want them)
SHELL ["/bin/bash", "-lc"]
RUN source "$NVM_DIR/nvm.sh" \
  && nvm install 24 \
  && nvm alias default 24 \
  && nvm use default \
  && corepack enable

# Make Node/npm on PATH for all interactive shells
RUN echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc \
  && echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc \
  && echo 'nvm use default > /dev/null 2>&1' >> ~/.bashrc
