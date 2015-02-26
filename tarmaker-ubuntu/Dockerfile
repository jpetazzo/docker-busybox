FROM ubuntu:14.04
RUN apt-get update -q
RUN apt-get install -qy busybox-static
RUN mkdir /rootfs
WORKDIR /rootfs
RUN mkdir bin etc dev dev/pts lib proc sys tmp
RUN touch etc/resolv.conf
RUN cp /etc/nsswitch.conf etc/nsswitch.conf
RUN echo root:x:0:0:root:/:/bin/sh > etc/passwd
RUN echo root:x:0: > etc/group
RUN ln -s lib lib64
RUN ln -s bin sbin
RUN cp /bin/busybox bin
RUN $(which busybox) --install -s bin
RUN bash -c "cp /lib/x86_64-linux-gnu/lib{c,m,dl,rt,nsl,nss_*,pthread,resolv}.so.* lib"
RUN cp /lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 lib
RUN tar cf /rootfs.tar .
RUN for X in console null ptmx random stdin stdout stderr tty urandom zero ; do tar uf /rootfs.tar -C/ ./dev/$X ; done
