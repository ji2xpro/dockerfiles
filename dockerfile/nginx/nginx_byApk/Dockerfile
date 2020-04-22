# 基础镜像
FROM alpine:latest

# 作者信息
MAINTAINER JimmyJiang <jimmyjiang520@163.com>

# 修改阿里源：https://mirrors.aliyun.com
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
# 安装需要的软件 
apk add --update nginx && \
mkdir /run/nginx/ && \
# 清除apk缓存
rm -rf /var/cache/apk/*

# 将目录下的文件copy到镜像中
COPY conf/nginx.conf /etc/nginx/nginx.conf
COPY conf/default.conf /etc/nginx/conf.d/default.conf

WORKDIR /etc/nginx

# 开放80端口
EXPOSE 80

# 启动nginx命令
CMD ["nginx", "-g", "daemon off;"]