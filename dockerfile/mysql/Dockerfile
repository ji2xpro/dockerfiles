# 指定基础镜像包：alpine最新版
FROM alpine:latest

# 指定维护者信息
MAINTAINER JimmyJiang <jimmyjiang520@163.com>

# 设置环境变量：数据库版本
ENV MYSQL_VERSION=10.4.12-r0

# 使用阿里镜像 https://mirrors.aliyun.com
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \ 
# 安装mysql，alpine系统下默认安装的是mariadb
apk add --no-cache mysql=${MYSQL_VERSION} mysql-client=${MYSQL_VERSION} mariadb-server-utils=${MYSQL_VERSION} && \
addgroup mysql mysql && \
mkdir /scripts && \
# 清除apk缓存
rm -rf /var/cache/apk/*

# 将启动文件复制到docker指定目录下
COPY ./startup.sh /scripts/startup.sh
RUN chmod +x /scripts/startup.sh

VOLUME ["/var/lib/mysql"]

# 暴露容器运行时对外的监听端口
EXPOSE 3306

# 设置容器启动时执行的命令
ENTRYPOINT ["/scripts/startup.sh"]