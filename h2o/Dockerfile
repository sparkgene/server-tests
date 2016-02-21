FROM debian:jessie

RUN \
  apt-get update && \
  apt-get install -y \
    ca-certificates \
    git \
    openssl \
    build-essential \
    cmake \
    libssl-dev \
    libyaml-dev && \
    rm -rf /var/lib/apt/lists/*

RUN \
  cd /tmp && \
  git clone --depth 1 https://github.com/h2o/h2o.git h2o && \
  cd h2o && \
  cmake . && \
  make && make install && \
  mkdir /opt/h2o

EXPOSE 8080
CMD ["/usr/local/bin/h2o", "-c", "/opt/h2o/h2o.conf"]
