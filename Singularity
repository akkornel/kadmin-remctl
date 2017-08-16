# vim: ts=4 sw=4 et

BootStrap: docker
From: ubuntu:16.04

%setup
    # Copy all files into a directory in the container.
    mkdir ${SINGULARITY_ROOTFS}/kadmin-remctl
    rsync -v -rlptD --exclude image . ${SINGULARITY_ROOTFS}/kadmin-remctl/

    # Install the Stanford krb5.conf file.
    curl -L -S -o ${SINGULARITY_ROOTFS}/etc/krb5.conf https://www.stanford.edu/dept/its/support/kerberos/dist/krb5.conf

%post
    # Install build tools and required libraries
    # (Even though installing the dev packages brings in the libraries, we name
    # them explicitly so that, when we uninstall the dev packages, the
    # libraries remain installed.)
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get install -y apt-utils
    apt-get dist-upgrade -y
    apt-get install --no-install-recommends -y \
    apt-utils autoconf automake build-essential libkrb5-3 libkrb5-dev \
    libremctl1 libtool libremctl-dev

    # Patch in our configuration
    cd /kadmin-remctl
    sed -i \
    -e 's|#include <util/xmalloc.h>|#include <util/xmalloc.h>\n#define STR(s) #s|' \
    -e 's|PASSWD_FILE "/full/path/to/passwd/file"|PASSWD_FILE STR(/afs/ir.stanford.edu/service/etc/passwd.all)|' \
    -e 's|PRINCIPAL "service/password-change"|PRINCIPAL STR(service/password-change)|' \
    -e 's|HOST "password-change.example.org"|HOST STR(password-change.stanford.edu)|' \
    passwd_change.c

    # Build and install software into the container
    ./autogen
    ./configure
    make
    make install

    # Clean up build tools
    apt-get purge -y \
    autoconf automake build-essential libkrb5-dev libremctl-dev libtool
    apt-get autoremove -y
    apt-get clean

%runscript
    #!/bin/bash
    exec /usr/local/bin/passwd_change $@
