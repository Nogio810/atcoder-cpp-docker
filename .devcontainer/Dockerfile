FROM ubuntu:22.04

# 環境を非対話モードに
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

# 基本ツール・開発ツールのインストール
RUN apt-get update && apt-get install -y \
    build-essential \
    g++ \
    gdb \
    cmake \
    git \
    zip \
    unzip \
    curl \
    wget \
    vim \
    python3 \
    python3-pip \
    nodejs \
    npm \
    # AtCoder環境に合わせたライブラリ
    libgmp-dev \
    libmpfr-dev \
    libmpc-dev \
    libboost-all-dev \
    # 日本語環境
    language-pack-ja-base \
    language-pack-ja \
    fonts-ipafont \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 日本語ロケールの設定
RUN locale-gen ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8
ENV LANGUAGE=ja_JP:ja
ENV LC_ALL=ja_JP.UTF-8

# online-judge-tools (oj)
RUN pip3 install --no-cache-dir online-judge-tools

# atcoder-cli (acc)
RUN npm install -g atcoder-cli

# 作業ディレクトリを作成
WORKDIR /app

# 開発用ユーザーを作成（sudo権限付与）
RUN apt-get update && apt-get install -y sudo && \
    useradd -m devuser && \
    echo "devuser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/devuser && \
    chmod 0440 /etc/sudoers.d/devuser && \
    chown -R devuser:devuser /app

# accの設定ファイルを準備
USER devuser
RUN mkdir -p /home/devuser/.config/atcoder-cli-nodejs

# よく使うC++の実行環境を準備（.bashrcに追加）
RUN echo 'alias g++="g++ -std=c++17 -Wall -Wextra -O2"' >> ~/.bashrc

# デフォルトのテンプレートを準備する場合はここで追加
# RUN mkdir -p /home/devuser/atcoder-workspace

WORKDIR /app
CMD ["/bin/bash"]