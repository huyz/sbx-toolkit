# templates/mise/Dockerfile
#
# Extends the official sandbox base with mise (https://github.com/jdx/mise).
# This is the toolchain-bearing option for projects that want automatic
# activation of mise.toml / .tool-versions.
#
# Build:
#   docker build --build-arg AGENT=claude-code -f templates/mise/Dockerfile .
#
# Extend this in your own Dockerfile:
#   ARG AGENT=claude-code
#   FROM localhost:8050/sandbox-mise:${AGENT}
#   USER root
#   RUN apt-get update && apt-get install -y my-tool
#   USER agent

ARG AGENT=claude-code
FROM docker/sandbox-templates:${AGENT}

ARG AGENT=claude-code

LABEL org.opencontainers.image.description="sbx-toolkit mise template (${AGENT})"

USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

USER agent

RUN curl https://mise.run | sh
RUN echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc \
 && echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bash_profile
RUN mkdir -p ~/.config/mise \
 && printf '[settings]\ntrusted_config_paths = ["/workspace"]\n' \
    >> ~/.config/mise/config.toml
