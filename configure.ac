dnl Process this file with Autoconf to produce a configure script.
dnl $Id$
dnl
dnl Written by Russ Allbery <rra@stanford.edu>
dnl Copyright 2007 Board of Trustees, Leland Stanford Jr. University
dnl
dnl See README for licensing terms.

AC_REVISION($Revision: 2820 $)
AC_PREREQ(2.57)
AC_INIT([kadmin-remctl], [1.0], [rra@stanford.edu])
AC_CONFIG_AUX_DIR([tools])
AM_INIT_AUTOMAKE
AM_MAINTAINER_MODE

AC_PROG_CC
AC_PROG_INSTALL

AC_ARG_WITH([remctl],
    AC_HELP_STRING([--with-remctl=DIR],
        [Prefix for remctl headers and libraries]),
    [if test x"$withval" != xno ; then
        CPPFLAGS="-I${withval}/include $CPPFLAGS"
        LDFLAGS="-L${withval}/lib $LDFLAGS"
     fi])

RRA_LIB_KRB5([gssapi], [true])

AC_CONFIG_FILES([Makefile])
AC_CONFIG_HEADER([config.h])
AC_OUTPUT