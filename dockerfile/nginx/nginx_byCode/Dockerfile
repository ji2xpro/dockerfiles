# 基础镜像
FROM alpine:latest

# 作者信息
MAINTAINER JimmyJiang <jimmyjiang520@163.com>

ENV NGINX_VERSION 1.16.1

# 修改阿里源：https://mirrors.aliyun.com
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \ 
CONFIG="\
        --prefix=/etc/nginx \
        --sbin-path=/usr/sbin/nginx \
        --modules-path=/usr/lib/nginx/modules \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --pid-path=/var/run/nginx.pid \
        --lock-path=/var/run/nginx.lock \
        --http-client-body-temp-path=/var/cache/nginx/client_temp \
        --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
        --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
        --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
        --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
        --user=nginx \
        --group=nginx \
        --with-http_ssl_module \
        --with-http_realip_module \
        --with-http_addition_module \
        --with-http_sub_module \
        --with-http_dav_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_random_index_module \
        --with-http_secure_link_module \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-http_xslt_module=dynamic \
        --with-http_image_filter_module=dynamic \
        --with-http_geoip_module=dynamic \
        --with-threads \
        --with-stream \
        --with-stream_ssl_module \
        --with-stream_ssl_preread_module \
        --with-stream_realip_module \
        --with-stream_geoip_module=dynamic \
        --with-http_slice_module \
        --with-mail \
        --with-mail_ssl_module \
        --with-compat \
        --with-http_v2_module \
    " && \
# 安装需要的软件
apk --update add openssl-dev pcre-dev zlib-dev wget build-base libxslt-dev gd-dev geoip-dev && \
# 编译安装nginx
mkdir -p /tmp/src && \
cd /tmp/src && \
wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
tar -zxvf nginx-${NGINX_VERSION}.tar.gz && \
cd /tmp/src/nginx-${NGINX_VERSION} && \
./configure $CONFIG --with-debug && \
make -j$(getconf _NPROCESSORS_ONLN) && \
make install && \
addgroup -S nginx && \
adduser -D -S -h /var/cache/nginx -s /sbin/nologin -G nginx nginx && \
apk del build-base && \
# 清除apk缓存和下载安装包
rm -rf /tmp/src && \
rm -rf /var/cache/apk/* && \
# forward request and error logs to docker log collector
ln -sf /dev/stdout /var/log/nginx/access.log && \
ln -sf /dev/stderr /var/log/nginx/error.log

VOLUME ["/var/log/nginx"]

WORKDIR /etc/nginx

# 开放80端口
EXPOSE 80

# 启动nginx命令
CMD ["nginx", "-g", "daemon off;"]