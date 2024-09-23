---
title: rustのguiライブラリeguiのテンプレートをdocker上で動かしてみる
tags:
  - ''
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

# rustのguiライブラリeguiのテンプレートをdocker上で動かしてみる

rustのguiアプリを作るのに、まずは一番簡単なeguiを使ってみようと思って、環境構築してみた。

## eguiとは

Rust の egui は、シンプルかつ即時モード（immediate mode）で動作する GUI ライブラリです。
軽量で使いやすく、クロスプラットフォーム対応しており、ゲームやツール開発に便利です。
コード内でウィジェットを直接作成・操作でき、リアルタイムなインタラクションが可能です。
（ChatGPT による説明）

## docker 上で egui のテンプレートを動かす

以下の egui の テンプレート実行してみる

[emilk/eframe_template at master](https://github.com/emilk/eframe_template/tree/master)


### 環境
windows 11 wsl ubuntu 24.04

docker CLI

### ディレクトリ構成

```
docker_rust
├── compose.yml
├── rust
│   └── Dockerfile
└── workspace
```

```yml:compose.yml
services:
  mylinux:
    build:
      context: .
      dockerfile: ./rust/Dockerfile
    container_name: myrust_gui
    hostname: myrust 
    working_dir: /workspace
    tty: true
    stdin_open: true
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - /mnt/wslg:/mnt/wslg
      - ./workspace:/workspace
    environment:
      - DISPLAY=$DISPLAY
      - PULSE_SERVER=$PULSE_SERVER
      - XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR
      - XDG_SESSION_TYPE=x11
      - WAYLAND_DISPLAY=
```

```Dockerfile:Dockerfile
FROM rust:latest

ENV CARGO_TARGET_DIR=/tmp/target \
  DEBIAN_FRONTEND=noninteractive \
  LC_CTYPE=ja_JP.utf8 \
  LANG=ja_JP.utf8

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y -q \
  locales \
  git \
  xserver-xorg \
  x11-apps \
  libxkbcommon-x11-0 \
  libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev libxkbcommon-dev libssl-dev
  && echo "ja_JP UTF-8" > /etc/locale.gen \
  && locale-gen 

WORKDIR /workspace
```

### 実行方法

以下のコマンドを実行する

wsl 上で
```bash
docker compose build
docker compose up -d
dockeer container exec -it myrust_gui bash
```

docker コンテナ内で
```bash
git clone https://github.com/emilk/eframe_template.git
cd eframe_template
cargo run --release
```

※以下のエラーが出ることがあるが、ただの通信エラーなのか
`cargo run --release`
コマンドをもう一度実行すればうまくいった
```shell-console
error: component download failed for rustc-x86_64-unknown-linux-gnu: error decoding response body
```

以下の画面が表示されれば完了

![eguiのテンプレートの画面](image.png)

### 補足

[emilk/eframe_template at master](https://github.com/emilk/eframe_template/tree/master)
に書かれている通りに実行した。

ただし、以下のエラーが発生して、waylandのコンポジターがないと出たので、waylandではなく、x11で動作させた。

```shell-console
Error: WinitEventLoop(Os(OsError { line: 81, file: "/home/anyumu/.cargo/registry/src/index.crates.io-6f17d22bba15001f/winit-0.29.9/src/platform_impl/linux/wayland/event_loop/mod.rs", error: WaylandError(Connection(NoCompositor)) }))
```

x11上で実行させるために、
xserver-xorg をインストール[^1]し、
以下の環境変数を設定した

[^1]:x11-appsもxserverの動作確認用にインストール。xeyesコマンドを打てば、目玉が表示される

```bash
export XDG_SESSION_TYPE=x11
export WAYLAND_DISPLAY=""
```

### 参考

[Rust/eguiで作るデスクトップ業務アプリ - aptpod Tech Blog](https://tech.aptpod.co.jp/entry/2023/12/19/100000)

[emilk/eframe_template at master](https://github.com/emilk/eframe_template/tree/master)

[【Rust】RustプロジェクトをDockerコンテナに移行してみる｜Rust｜開発ブログ｜株式会社Nextat（ネクスタット）](https://nextat.co.jp/staff/archives/348)

[Ubuntu 20.04 LTS に後から GUI (X Window System) を追加する - CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/ubuntu-2004-install-gui)

[WindowsのDocker環境でRust GUIアプリを動かす #gtk-rs - Qiita](https://qiita.com/t13801206/items/27b17a3b027ebdd7319b)

[【Rust】winit と tiny-skia で低レベルなグラフィックス描画〜ことはじめ〜 - A Memorandum](https://blog1.mammb.com/entry/2024/03/12/000000)


