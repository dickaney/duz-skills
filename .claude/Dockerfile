FROM node:22-bookworm

ARG USER_UID=1000
ARG USER_GID=1000

RUN apt-get update && apt-get install -y \
    locales \
    tzdata \
    git \
    openssh-client \
    curl \
    wget \
    vim \
    jq \
    unzip \
    zip \
    tree \
    less \
    ripgrep \
    fd-find \
    procps \
    dnsutils \
    net-tools \
    iputils-ping \
    build-essential \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# fd-find 在 Debian 上二进制名是 fdfind，建立 fd 软链
RUN ln -s "$(which fdfind)" /usr/local/bin/fd

# Japanese UTF-8 Locale
RUN sed -i '/ja_JP.UTF-8/s/^# //g' /etc/locale.gen \
    && locale-gen
ENV LANG=ja_JP.UTF-8
ENV LANGUAGE=ja_JP:ja
ENV LC_ALL=ja_JP.UTF-8

RUN npm install -g @anthropic-ai/claude-code

# 先创建 workspace
RUN mkdir -p /workspace

# 再调整 node 用户的 UID/GID 并统一修正属主
# GID 若已被占用（如 Debian 的 dialout=20 / macOS staff=20），直接复用该组，不再 groupmod
RUN set -eux; \
    if ! getent group "$USER_GID" >/dev/null; then \
        groupmod -g "$USER_GID" node; \
    fi; \
    usermod -u "$USER_UID" -g "$USER_GID" node; \
    chown -R "$USER_UID":"$USER_GID" /home/node /workspace

USER node
WORKDIR /workspace
CMD ["/bin/bash"]
