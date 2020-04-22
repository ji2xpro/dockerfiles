# 指定基础镜像包：alpine最新版
FROM alpine:latest

# 指定维护者信息
MAINTAINER JimmyJiang <jimmyjiang520@163.com>

# 设置环境变量：数据库版本
ENV RABBITMQ_VERSION=3.8.3 \
    ERLANG_PKG_VERSION=22.1-r0 \
    RABBITMQ_HOME=/rabbitmq \
    # RABBITMQ_PACKAGE=rabbitmq-server-generic-unix-3.8.3.tar.xz
    RABBITMQ_PACKAGE=rabbitmq-server-generic-unix-latest-toolchain-3.8.3.tar.xz

ENV PATH=${RABBITMQ_HOME}/sbin:$PATH

COPY ${RABBITMQ_PACKAGE} /

# 使用阿里镜像 https://mirrors.aliyun.com
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \ 
    # Setup Erlang, download RabbitMQ and setup the management plugin
    # apk add --update --no-cache curl tar xz \
    #     erlang=${ERLANG_PKG_VERSION} erlang-mnesia=${ERLANG_PKG_VERSION} \
    #     erlang-public-key=${ERLANG_PKG_VERSION} erlang-crypto=${ERLANG_PKG_VERSION} \
    #     erlang-hipe=${ERLANG_PKG_VERSION} \
    #     erlang-ssl=${ERLANG_PKG_VERSION} erlang-sasl=${ERLANG_PKG_VERSION} \
    #     erlang-asn1=${ERLANG_PKG_VERSION} erlang-inets=${ERLANG_PKG_VERSION} \
    #     erlang-os-mon=${ERLANG_PKG_VERSION} erlang-xmerl=${ERLANG_PKG_VERSION} \
    #     erlang-eldap=${ERLANG_PKG_VERSION} erlang-syntax-tools=${ERLANG_PKG_VERSION} && \
    for p in erlang erlang-mnesia erlang-public-key erlang-crypto erlang-ssl erlang-sasl \
                erlang-asn1 erlang-inets erlang-os-mon erlang-xmerl erlang-eldap erlang-syntax-tools;do \
                ERLS="$ERLS $p=$ERLANG_PKG_VERSION"; done && \
    apk add --update --no-cache curl tar xz wget $ERLS  && \

    mkdir -p ${RABBITMQ_HOME} && \
    tar -xvf ${RABBITMQ_PACKAGE} -C ${RABBITMQ_HOME} --strip-components 1 && \
    # wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v$RABBITMQ_VERSION/rabbitmq-server-generic-unix-latest-toolchain-$RABBITMQ_VERSION.tar.xz | tar -xvf -C ${RABBITMQ_HOME} --strip-components 1 && \
    # curl -sSL https://github.com/rabbitmq/rabbitmq-server/releases/download/v${RABBITMQ_VERSION}/rabbitmq-server-generic-unix-${RABBITMQ_VERSION}.tar.xz | tar -xJ -C ${RABBITMQ_HOME} --strip-components 1 && \
    # rm -rf /share/**/rabbitmq*.xz && \
    apk del --purge curl tar xz && \
    addgroup rabbitmq && \
    adduser -DS -g "" -G rabbitmq -s /bin/sh -h /var/lib/rabbitmq rabbitmq && \
    mkdir -p /data/rabbitmq && \
    chown -R rabbitmq:rabbitmq /data/rabbitmq && \
    # 清除apk缓存
    rm -rf /var/cache/apk/*

COPY ./docker-entrypoint.sh /usr/local/bin/
RUN ln -s usr/local/bin/docker-entrypoint.sh /entrypoint.sh # backwards compat
RUN chmod +x usr/local/bin/docker-entrypoint.sh

# Define mount points.
# VOLUME ["/data/rabbitmq"]
VOLUME ["${RABBITMQ_HOME}/var"]

# Expose ports for AMQP protocol and rabbitmq management plugin respectively
EXPOSE 5672/tcp 15672/tcp

# 设置容器启动时执行的命令
CMD ["docker-entrypoint.sh"]