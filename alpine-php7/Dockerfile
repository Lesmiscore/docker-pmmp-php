# Build php: some deps are required
FROM alpine AS phpbuild
RUN apk add --no-cache make autoconf automake m4 wget gzip bzip2 bison g++ libtool && \
    mkdir /build && cd /build && \
    wget --no-check-certificate -O compile.sh https://github.com/pmmp/php-build-scripts/raw/master/compile.sh && \
    chmod a+x compile.sh && \
    sed -i 's%https://gmplib.org/download/gmp/%http://gnu.uberglobalmirror.com/gmp/%g' compile.sh && \
    ./compile.sh -t linux64 -l -j 4 -c -s

# TODO: replace it to alpine, if possible
FROM alpine

# copy php binaries from the past section
RUN mkdir -p /opt/php && apk add --no-cache libc6
COPY --from=phpbuild /build/bin /opt/php

# set PATH
ENV PATH ${PATH}:/opt/php/php7/bin

RUN php -version
