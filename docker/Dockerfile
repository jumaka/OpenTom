FROM ubuntu:latest AS tomtom
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata keyboard-configuration
RUN apt-get install -y man-db
RUN apt-get install -y git
RUN dpkg --add-architecture i386
RUN apt-get update
RUN apt install -y autoconf chrpath fluid imagemagick libglib2.0-dev libtool subversion xsltproc gawk dh-autoreconf pkg-config libglib2.0-dev libc-dev-bin libncurses-dev cmake
RUN apt install -y vim wget binutils gettext # binutils-arm-linux-gnueabi
RUN mkdir -p /tomtom
RUN mkdir -p /tomtom/gcc
WORKDIR /tomtom/gcc
RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.30.tar.bz2
RUN wget https://ftp.gnu.org/gnu/gcc/gcc-4.9.4/gcc-4.9.4.tar.bz2
RUN wget https://gmplib.org/download/gmp/gmp-6.2.1.tar.xz
RUN wget https://www.mpfr.org/mpfr-current/mpfr-4.2.0.tar.xz
RUN wget https://www.multiprecision.org/downloads/mpc-1.3.0.tar.gz
RUN wget https://ftp.gnu.org/gnu/glibc/glibc-2.20.tar.bz2
RUN wget https://www.kernel.org/pub/linux/kernel/v3.x/linux-3.17.2.tar.xz
RUN tar xfj binutils-2.30.tar.bz2
RUN tar xfj gcc-4.9.4.tar.bz2
RUN tar xf gmp-6.2.1.tar.xz
RUN tar xf mpfr-4.2.0.tar.xz
RUN tar xf mpc-1.3.0.tar.gz
RUN tar xfj glibc-2.20.tar.bz2
RUN tar xf linux-3.17.2.tar.xz
WORKDIR /tomtom/gcc/gcc-4.9.4
# RUN for d in $(find ../binutils-2.30/* -type d -prune); do bn=$(basename $d); if [ ! -d $bn ]; then ln -s $d .; fi; done
RUN ln -s ../gmp-6.2.1 gmp
RUN ln -s ../mpc-1.3.0 mpc
RUN ln -s ../mpfr-4.2.0 mpfr
WORKDIR /tomtom/gcc
RUN mkdir -p /tomtom/gcc/build
RUN mkdir -p /tomtom/gcc/build-binutils
RUN mkdir -p /tomtom/gcc/build-glibc
RUN mkdir -p /opt/cross
WORKDIR /tomtom/gcc/build-binutils
RUN ../binutils-2.30/configure --target=arm-linux-gnueabi --prefix=/opt/cross --with-sysroot --disable-nls --disable-werror --disable-multilib
RUN make -j 6
RUN make install
WORKDIR /tomtom/gcc/linux-3.17.2
RUN PATH=/opt/cross/bin:$PATH make ARCH=arm INSTALL_HDR_PATH=/opt/cross/arm-linux-gnueabi headers_install
WORKDIR /tomtom/gcc/build
RUN PATH=/opt/cross/bin:$PATH CXXFLAGS="-std=gnu++11" ../gcc-4.9.4/configure -target=arm-linux-gnueabi --enable-languages=c,c++ --prefix=/opt/cross --without-headers --disable-nls --disable-multilib
RUN PATH=/opt/cross/bin:$PATH make -j 6 all-gcc
RUN PATH=/opt/cross/bin:$PATH make install-gcc
WORKDIR /tomtom/gcc/build-glibc
RUN PATH=/opt/cross/bin:$PATH ../glibc-2.20/configure --build=x86_64-unknown-linux-gnu --host=arm-linux-gnueabi --prefix=/opt/cross/arm-linux-gnueabi --disable-multilib libc_cv_forced_unwind=yes
RUN PATH=/opt/cross/bin:$PATH make install-bootstrap-headers=yes install-headers
RUN PATH=/opt/cross/bin:$PATH make -j 6 csu/subdir_lib
RUN PATH=/opt/cross/bin:$PATH install csu/crt1.o csu/crti.o csu/crtn.o /opt/cross/arm-linux-gnueabi/lib
RUN PATH=/opt/cross/bin:$PATH arm-linux-gnueabi-gcc -nostdlib -nostartfiles -shared -x c /dev/null -o /opt/cross/arm-linux-gnueabi/lib/libc.so
RUN touch /opt/cross/arm-linux-gnueabi/include/gnu/stubs.h
WORKDIR /tomtom/gcc/build
RUN PATH=/opt/cross/bin:$PATH make -j 6 all-target-libgcc
RUN PATH=/opt/cross/bin:$PATH make install-target-libgcc
WORKDIR /tomtom/gcc/build-glibc
RUN PATH=/opt/cross/bin:$PATH make -j 6
RUN PATH=/opt/cross/bin:$PATH make install
WORKDIR /tomtom/gcc/build
RUN PATH=/opt/cross/bin:$PATH make -j 6
RUN PATH=/opt/cross/bin:$PATH make install
WORKDIR /opt/cross
# RUN ln -s arm-linux-gnueabi /usr
# RUN ln bin/* /usr/bin
# RUN ln -s lib/gcc/arm-linux-gnueabi /usr/lib/gcc
# RUN ln -s libexec/gcc/arm-linux-gnueabi /usr/libexec/gcc
RUN mkdir -p /root/.ssh
COPY id_rsa* /root/.ssh/
COPY gitconfig /root/.gitconfig
WORKDIR /tomtom
# RUN ssh -q git@github.com
# RUN git clone git@github.com:jumaka/OpenTom.git
# WORKDIR /tomtom/OpenTom
# RUN apt install -y gcc-arm-linux-gnueabi
# RUN sed -e s,' 'ROOT=,'&'`pwd`, get_cross_env.sh > ../get_cross_env_cfg.sh
# RUN . ../get_cross_env_cfg.sh && make
CMD ["bash"]
