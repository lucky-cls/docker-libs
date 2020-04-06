FROM php:7.2-fpm-alpine3.11


MAINTAINER cailusenlucky<cailusen.luckey@gmail.com>


ENV TIME_ZONE Asia/Shanghai


# 安装composer并允许root用户运行
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_NO_INTERACTION=1
ENV COMPOSER_HOME=/usr/local/share/composer

	## 修改镜像源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \

    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		bzip2-dev \

	## 修改时区1
	   	tzdata \
	## 修改时区2
	   	&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/timezone \


	&& apk add --no-cache \
        imagemagick-dev \
        libmcrypt-dev \
        icu-dev \
        freetype-dev \
        libjpeg-turbo-dev \
        gettext-dev \


	## 安装扩展
	&& pecl install \
		yaf \
		redis \
		imagick \
		mcrypt \

	&& docker-php-ext-enable \
		yaf \
		redis \
		imagick \
		mcrypt \


	## 安装扩展
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include \
	&& docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" \
		bcmath \
		bz2 \
		sockets \
		pdo_mysql \
		pcntl \
		mysqli \
		gettext \
		gd\
		iconv \
		zip \
		opcache \
		intl \


# 安装composer
	&& curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
	&& php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
	&& rm -f /tmp/composer-setup.* \
# 配置composer中国全量镜像
   	&& composer config -g repo.packagist composer https://packagist.phpcomposer.com \


	## 清理缓存
	&& rm -rf /tmp/*  ~/.pearrc \
	## 清理安装过的包
	&& apk del --no-network .build-deps 



	## 项目目录文件
VOLUME /var/www/html

	## 配置文件
VOLUME /usr/local/etc

STOPSIGNAL SIGQUIT

EXPOSE 9000

CMD ["php-fpm"]
