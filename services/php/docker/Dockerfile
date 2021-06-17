FROM debian

# package with compile:
#   -- build-essential autoconf automake libtool bison3.7 re2c
RUN apt-get update && apt-get install -y \
    build-essential autoconf automake libtool re2c wget git gdb && \
    rm -rf /var/lib/apt/lists/*

ENV PATH "/opt/bison/bin:$PATH"
ENV PATH "/opt/php/bin:$PATH"

RUN git clone https://github.com/php/php-src.git /root/php-src && \
    git clone https://github.com/derickr/vld.git /root/vld && \
    git clone https://github.com/xdebug/xdebug.git /root/xdebug

RUN cd /tmp && wget http://ftp.gnu.org/gnu/bison/bison-3.7.tar.gz && \
    tar -xvf bison-3.7.tar.gz && \
    rm bison-3.7.tar.gz && \
    cd bison-3.7 && \
    ./configure --prefix=/opt/bison --with-libiconv-prefix=/opt/libiconv && \
    make && make install && \
    rm -rf /tmp/bison-3.7

COPY .gdbinit /tmp/.gdbinit
ENV PHP_VERSION PHP-7.4

# install php, vld
RUN cd /root/php-src && \
    git checkout $PHP_VERSION && \
    ./buildconf --force && \
    ./configure --disable-all --enable-debug --prefix=/opt/php && \
    make && make install && \
    cp /root/php-src/.gdbinit /root/.gdbinit && \
    cat /tmp/.gdbinit >> /root/.gdbinit  && \
    cd /root/vld && \
    phpize && \
    ./configure && \
    make && make install && \
    echo "extension=vld.so" >> /opt/php/lib/php.ini && \
    cd /root/xdebug && \
    mkdir /www

# install xdebug
RUN cd /root && wget http://xdebug.org/files/xdebug-3.0.4.tgz && \
    tar -xvzf xdebug-3.0.4.tgz && \
    rm xdebug-3.0.4.tgz && \
    cd xdebug-3.0.4 && \
    phpize && \
    ./configure && \
    make && make install && \
    echo "zend_extension=xdebug.so" >> /opt/php/lib/php.ini


WORKDIR /www
#VOLUME ["/www"]

CMD ["/bin/bash"]
