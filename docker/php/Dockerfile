FROM php:8.0-fpm

COPY ./php.ini /usr/local/etc/php/

RUN apt-get update \
  && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev libonig-dev wget \
  && docker-php-ext-install pdo_mysql mysqli iconv

# PHPのGD拡張をインストール (画像のリサイズ処理で必要です)
RUN apt-get install -y wget libjpeg-dev libfreetype6-dev
RUN apt-get install -y libmagick++-dev \
libmagickwand-dev \
libpq-dev \
libfreetype6-dev \
libjpeg62-turbo-dev \
libpng-dev \
libwebp-dev \
libxpm-dev
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

# PHPのZIP拡張をインストール (composerにとって必要です)
RUN apt-get install -y unzip zlib1g-dev libzip-dev
RUN docker-php-ext-install zip

# mailhog設定
RUN curl -sSL https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 -o mhsendmail \
  && chmod +x mhsendmail \
  && mv mhsendmail /usr/local/bin/mhsendmail

# composerをインストール。
# うまく行かないときは https://getcomposer.org/download/ の項「Command-line installation」もご参照ください。
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

