# 指定基础镜像包：alpine最新版
FROM alpine:latest

# 指定维护者信息
MAINTAINER JimmyJiang <jimmyjiang520@163.com>

# 设置环境变量：数据库版本
ENV REDIS_VERSION=5.0.7-r0

# 使用阿里镜像 https://mirrors.aliyun.com
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \ 
# 安装redis（apk add --no-cache --purge -uU redis）
apk add --update redis=${REDIS_VERSION} && \
mkdir /data && \
chown -R redis:redis /data && \
# 清除apk缓存
rm -rf /var/cache/apk/*

COPY ./redis.conf /etc/redis.conf

VOLUME ["/data"]

# 暴露容器运行时对外的监听端口
EXPOSE 6379

# 设置容器启动时执行的命令
CMD ["redis-server","/etc/redis.conf"]