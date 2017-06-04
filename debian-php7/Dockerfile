# Build php: some deps are required
FROM debian AS phpbuild
RUN apt-get update && \
    apt-get install -y make autoconf automake libtool-bin m4 wget gzip bzip2 bison g++ \
    libc6-dev-i386 g++-multilib && \
    mkdir /build && cd /build && \
    wget https://github.com/pmmp/php-build-scripts/raw/master/compile.sh && \
    chmod a+x compile.sh && \
    sed -i 's%https://gmplib.org/download/gmp/%http://gnu.uberglobalmirror.com/gmp/%g' compile.sh && \
    ./compile.sh -t linux64 -l -j 4 -c -s

# TODO: replace it to alpine, if possible
FROM debian

# copy php binaries from the past section
RUN mkdir -p /opt/php && apt-get update && apt-get install -y libc6 && rm -rf /var/lib/apt/lists/*
COPY --from=phpbuild /build/bin /opt/php

# set PATH
ENV PATH ${PATH}:/opt/php/php7/bin

RUN php -version
