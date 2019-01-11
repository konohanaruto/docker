FROM php:7.2-fpm

# 官方Dockerfile地址
# https://github.com/docker-library/php/blob/c88c3d52f41a370f3a62e3ded62b7b223b4cb846/7.2/stretch/fpm/Dockerfile
# 安装基本依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libxml2-dev \
    git \
    libssl-dev \
    libevent-dev \
    libmemcached-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli pdo pdo_mysql hash zip mbstring \
    && docker-php-ext-install opcache

# 安装Imagick扩展，因为上面我们已经安装了libmagickwand-dev系统库，所以这里可以直接安装扩展
RUN pecl install imagick && docker-php-ext-enable imagick

# mcrypt扩展没有在php7.2中被提供，这里单独用pecl安装，此外上方必须要先安装系统库libmcrypt-dev，坑。
# 参考：https://github.com/docker-library/php/issues/623
RUN pecl install mcrypt-1.0.1 && docker-php-ext-enable mcrypt

# 安装mongodb扩展
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# 安装xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# 安装php-redis扩展
ENV PHPREDIS_VERSION 4.2.0

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz  \
    && mkdir /tmp/redis \
    && tar -xf /tmp/redis.tar.gz -C /tmp/redis \
    && rm /tmp/redis.tar.gz \
    && ( \
    cd /tmp/redis/phpredis-$PHPREDIS_VERSION \
    && phpize \
    && ./configure \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r /tmp/redis \
    && docker-php-ext-enable redis

# 安装memcached扩展

RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
    && mkdir -p memcached \
    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && ( \
    cd memcached \
    && phpize \
    && ./configure \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r memcached \
    && rm /tmp/memcached.tar.gz \
    && docker-php-ext-enable memcached

#安装swoole扩展
RUN pecl install swoole && docker-php-ext-enable swoole


# 拷贝opcache配置文件到指定php配置目录，具体查看: (这里我尚未解决，待成功后查看下opcache装到哪个目录了，这里需要先cd进去)
#COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini（如果volume的容器目录被host机挂载，那么之前的类似ADD、COPY等命令复制到image的文件不会被复制到新的挂载点，坑,这>条命令COPY复制当前目录的opcache.ini文件，所以它必须在当前的Dockerfile所在目录一致，但复制成功后，如果/usr/local/etc/php/conf.d/或者/usr/local/etc/php目录被挂载，实际上image的内容不会被复制到volume，实际上docker-compose.yml中我们已经添加了etc/php:/usr/local/etc/php，所以这条命令最终是不会生效的，必须放在volume的host机目录才能生效，这里特地记录一下子。。。）
# 以上解决了这个问题，创建etc/php/opcache.ini文件，并绑定到volume，解决了这个问题

# 删除下载的文件包，清理空间
RUN rm -rf /var/lib/apt/lists/*