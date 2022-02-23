# => ベースイメージの指定 FROM イメージ名:タグ名
FROM php:8.0-fpm-buster
# => Macのbin/bashを使うよと指定する
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]
# コンテナ内のサーバー環境片酢の定義
ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer
# ベースイメージからcomposerを取ってきてインストールする
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer
# apt-get updateでインストール可能なパッケージ一覧の更新、他はlaravelに必要なパッケージの調達
RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath
# ベースイメージからphp.iniを取ってきてコピー
COPY ./php.ini /usr/local/etc/php/php.ini
# ワークディレクトリの設定(なければ自動で作ってくれる)
WORKDIR /work