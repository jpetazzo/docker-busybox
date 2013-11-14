FROM ubuntu:12.04
RUN apt-get update -q
RUN apt-get install -qy wget build-essential libncurses-dev rsync unzip bc
RUN wget http://buildroot.uclibc.org/downloads/buildroot-2013.08.1.tar.gz
RUN echo 'ffea9c3154ca2de4141ae0604ae51f4e  buildroot-2013.08.1.tar.gz' > buildroot.md5
RUN md5sum --check buildroot.md5
RUN tar -zxf buildroot-2013.08.1.tar.gz
WORKDIR /buildroot-2013.08.1
RUN make defconfig
RUN sed -i 's/BR2_i386=y/BR2_x86_64=y/' .config
RUN echo BR2_TOOLCHAIN_BUILDROOT_LARGEFILE=y >>.config
RUN echo BR2_TOOLCHAIN_BUILDROOT_INET_IPV6=y >>.config
RUN echo BR2_TOOLCHAIN_BUILDROOT_WCHAR=y >>.config
RUN echo BR2_PACKAGE_BRIDGE_UTILS=y >>.config
RUN echo BR2_PACKAGE_IPROUTE2=y >>.config
RUN echo BR2_PACKAGE_IPTABLES=y >>.config
RUN make olddefconfig
RUN make
RUN ln -s /buildroot-2013.08.1/output/images/rootfs.tar /rootfs.tar
