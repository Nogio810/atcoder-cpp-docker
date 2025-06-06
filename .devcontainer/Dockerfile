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
    # ネットワークツール
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 日本語ロケールの設定
RUN locale-gen ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8
ENV LANGUAGE=ja_JP:ja
ENV LC_ALL=ja_JP.UTF-8

# AtCoder関連ツールのインストール
RUN pip3 install --no-cache-dir \
    online-judge-tools \
    aclogin \
    requests \
    beautifulsoup4

# atcoder-cli (acc)
RUN npm install -g atcoder-cli

# 作業ディレクトリを作成
WORKDIR /app

# 開発用ユーザーを作成（sudo権限付与）
RUN apt-get update && apt-get install -y sudo && \
    useradd -m devuser && \
    echo "devuser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/devuser && \
    chmod 0440 /etc/sudoers.d/devuser && \
    chown -R devuser:devuser /app && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY templates/ /app/templates/

# ユーザーを切り替え
USER devuser

# accの設定ディレクトリを準備
RUN mkdir -p /home/devuser/.config/atcoder-cli-nodejs

# よく使うC++の実行環境を準備（.bashrcに追加）
RUN echo 'alias g++="g++ -std=c++17 -Wall -Wextra -O2"' >> ~/.bashrc && \
    echo 'alias gpp="g++ -std=c++17 -Wall -Wextra -O2"' >> ~/.bashrc && \
    echo 'alias run="g++ -std=c++17 -Wall -Wextra -O2 -o main main.cpp && ./main"' >> ~/.bashrc

# AtCoder環境セットアップ用のヘルパースクリプトを作成
RUN mkdir -p /home/devuser/bin && \
    cat > /home/devuser/bin/setup-atcoder.sh << 'EOF'\
    #!/bin/bash
    echo "🚀 AtCoder環境のセットアップガイド"\
    echo ""\
    echo "📋 ログイン手順："\
    echo "1. ブラウザでAtCoderにログイン"\
    echo "2. F12キー → Application/Storage → Cookies → atcoder.jp"\
    echo "3. REVEL_SESSIONの値をコピー"\
    echo "4. ターミナルで 'aclogin' を実行してクッキー値を貼り付け"\
    echo ""\
    echo "🔧 利用可能なコマンド："\
    echo "  aclogin                    # AtCoderセッション設定"\
    echo "  acc new [contest]          # コンテスト環境作成"\
    echo "  acc add [problem]          # 問題追加"\
    echo "  oj test                    # テスト実行"\
    echo "  acc submit                 # 解答提出"\
    echo "  run                        # main.cppをコンパイル・実行"\
    echo ""\
    echo "📚 便利なエイリアス："\
    echo "  g++   = g++ -std=c++17 -Wall -Wextra -O2"\
    echo "  gpp   = g++ -std=c++17 -Wall -Wextra -O2"\
    echo "  run   = コンパイル→実行"\
    echo ""\
    echo "🎯 まずは 'aclogin' でログイン情報を設定してください！"\
    EOF

# スクリプトに実行権限を付与
RUN chmod +x /home/devuser/bin/setup-atcoder.sh

# PATHにユーザーのbinディレクトリを追加
RUN echo 'export PATH=$PATH:/home/devuser/bin' >> ~/.bashrc

WORKDIR /app
CMD ["/bin/bash"]