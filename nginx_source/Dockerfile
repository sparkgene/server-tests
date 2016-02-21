FROM ubuntu:14.04

RUN apt-get update && apt-get install -y ca-certificates build-essential wget libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev

RUN \
  mkdir /tmp/openssl && \
  wget https://www.openssl.org/source/openssl-1.0.1r.tar.gz -O /tmp/openssl/openssl-1.0.1r.tar.gz && \
  cd /tmp/openssl && \
  tar -xvzf openssl-1.0.1r.tar.gz && \
  cd openssl-1.0.1r && \
  ./config \
    --prefix=/usr \
    --openssldir=/usr/ssl && \
  make && make install && \
  ./config shared \
    --prefix=/usr/local \
    --openssldir=/usr/local/ssl && \
  make clean && \
  make && make install

RUN \
  mkdir /tmp/nginx && \
  wget http://nginx.org/download/nginx-1.9.5.tar.gz -O /tmp/nginx/nginx-1.9.5.tar.gz && \
  cd /tmp/nginx && \
  tar -xzvf nginx-1.9.5.tar.gz && \
  cd nginx-1.9.5 && \
  ./configure \
    --prefix=/usr/local/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --pid-path=/var/run/nginx.pid \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-openssl=/tmp/openssl/openssl-1.0.1r \
    --with-http_realip_module \
    --with-http_stub_status_module \
    --with-threads \
    --with-ipv6 && \
  make  && \
  make install

RUN apt-get purge build-essential -y && \
  rm -rf /var/lib/apt/lists/*

RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
  ln -sf /dev/stderr /var/log/nginx/error.log

VOLUME ["/var/cache/nginx"]

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
