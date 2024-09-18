# ベースイメージ
FROM php:8.2-fpm

# システムの依存関係をインストール
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# PHP拡張モジュールのインストール
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd zip pdo pdo_mysql

# Composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 作業ディレクトリを設定
WORKDIR /var/www

# ソースコードをコンテナにコピー
COPY . /var/www

# 権限の設定
RUN chown -R www-data:www-data /var/www

# エントリーポイント
CMD ["php-fpm"]
