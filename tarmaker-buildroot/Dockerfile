FROM ubuntu:14.04
# This will make it easier to upgrade to the next version of buildroot.
# You might have to also update the busybox version at the end of this file!
ENV BUILDROOT_VERSION 2015.02
# This will get rid of a build warning related to 'tput'.
ENV TERM dumb
RUN apt-get update -q
RUN apt-get install -qy wget build-essential libncurses-dev rsync unzip bc
# Buildroot needs Python. Let's git it!
RUN which python || ln /usr/bin/python3 /usr/bin/python
# Let's make wget output a bit less verbose.
RUN echo progress = dot:mega > /.wgetrc
RUN wget http://buildroot.uclibc.org/downloads/buildroot-$BUILDROOT_VERSION.tar.gz
# Check that we got the right tarball (update this when upgrading buildroot!)
RUN echo "d1559c7651982d62be3544561f1cb81b  buildroot-$BUILDROOT_VERSION.tar.gz" > buildroot.md5
RUN md5sum --check buildroot.md5
RUN tar -zxf buildroot-$BUILDROOT_VERSION.tar.gz
# This symlink is because WORKDIR doesn't expand $VARS.
RUN ln -s buildroot-$BUILDROOT_VERSION buildroot
WORKDIR /buildroot
RUN make defconfig
RUN sed -i "s/BR2_i386=y/# BR2_i386 is not set/" .config
RUN for CFGVAR in \
    BR2_x86_64 \
    BR2_TOOLCHAIN_BUILDROOT_LARGEFILE \
    BR2_TOOLCHAIN_BUILDROOT_INET_IPV6 \
    BR2_TOOLCHAIN_BUILDROOT_WCHAR \
    BR2_PACKAGE_BRIDGE_UTILS \
    BR2_PACKAGE_IPROUTE2 \
    BR2_PACKAGE_IPTABLES \
    ; do sed -i "s/# $CFGVAR is not set/$CFGVAR=y/" .config ; done
RUN for CFGVAR in \
    CONFIG_NC \
    CONFIG_NC_SERVER \
    CONFIG_NC_EXTRA \
    CONFIG_BRCTL \
    CONFIG_PING6 \
    CONFIG_FEATURE_IPV6 \
    CONFIG_FEATURE_IFUPDOWN_IPV6 \
# The below feature will prefer an IPv4 address over IPv6 from DNS to handle the
# original problem regarding nc using the AAAA record if it exists
    CONFIG_FEATURE_PREFER_IPV4_ADDRESS \
    CONFIG_FEATURE_BRCTL_FANCY \
    CONFIG_FEATURE_BRCTL_SHOW \
    CONFIG_FEATURE_BASH_IS_ASH \
    ; do sed -i "s/# $CFGVAR is not set/$CFGVAR=y/" package/busybox/busybox.config ; done
RUN make olddefconfig
RUN make
RUN ln -s /buildroot/output/images/rootfs.tar /rootfs.tar
