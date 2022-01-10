FROM php:8.0-fpm-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]
# SHELL命令は何も指定しない場合は SHELL ["/bin/sh", "-c"] がデフォルト値(Linuxの場合)
# パイプ中のあらゆる段階でエラーがあれば失敗とするため、pipefailオプションを明示的に指定。
# -o オプションはオプションを設定するためのオプション。
# -e オプションを定義しておくと、そのシェルスクリプト内で何らかのエラーが発生した時点で、それ以降の処理を中断できる。
# -u オプションを定義しておくと、未定義の変数に対して読み込み等を行おうとした際にエラーとなる。
# -x オプションを定義しておくと、実行したコマンドを全て標準エラー出力に出してくれる。
# SHELL命令は必須ではない

ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer
# ENV命令はコンテナ内のサーバー環境変数を設定

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath
# Dockerfile内で apt を実行するとCLIインターフェース向けではないと警告が表示されるため、apt-getを使用
# apt-get update インストール可能なパッケージの「一覧」を更新。実際のパッケージのインストール、アップグレードなどは行わない
# apt-get clean && rm -rf /var/lib/apt/lists/* パッケージインストールで使用したキャッシュファイルを削除
# docker-php-ext-install intl pdo_mysql zip bcmath PHPの拡張ライブラリをインストール
COPY ./php.ini /usr/local/etc/php/php.ini

WORKDIR /work