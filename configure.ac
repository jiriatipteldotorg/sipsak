#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.59)
AC_INIT([sipsak],[0.9.5],[nils@sipsak.org])
AM_INIT_AUTOMAKE
AM_MAINTAINER_MODE
AC_CONFIG_SRCDIR([sipsak.c])
AC_CONFIG_HEADER([config.h])

# Checks for programs.
AC_PROG_CC
# Add -Wall if we are using GCC
if test "x$GCC" = "xyes"; then
  CFLAGS="$CFLAGS -Wall"
fi
GCC_STACK_PROTECT_CC
CHECK_PROG_DISTCC

AC_PROG_INSTALL

# Checks for libraries.
AC_CANONICAL_HOST

case "$host" in
  *-*-solaris*)
    LIBS="$LIBS -lposix4 -lsocket -lnsl"
    ;;
  *)
    LIBS="$LIBS"
    ;;
esac
AC_SUBST([LIBS])

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([ctype.h errno.h arpa/inet.h netdb.h netinet/in.h netinet/in_systm.h limits.h sys/poll.h regex.h signal.h stdlib.h stdio.h string.h sys/param.h sys/socket.h sys/time.h unistd.h sys/utsname.h],,[AC_MSG_ERROR([missing required header (see above)])],)
AC_CHECK_HEADERS([getopt.h])
AC_HEADER_SYS_WAIT
AC_HEADER_TIME

SIPSAK_IP_UDP
SIPSAK_ICMP
AC_CHECK_HEADERS([cygwin/icmp.h],,,)

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_HEADER_TIME

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_SELECT_ARGTYPES
AC_CHECK_FUNCS([getchar gethostbyname gethostname getopt getpid gettimeofday memset ntohs regcomp select socket strchr strcmp strstr strtol uname],,[AC_MSG_ERROR([missing required function (see above)])])
AC_CHECK_FUNCS([calloc getdomainname getopt_long inet_ntop strncasecmp strcasestr])

AM_PATH_SIPSAK_LIBGNUTLS([1.0.0],
  AC_DEFINE([HAVE_GNUTLS], [1], [Has gnutls])
  LIBS="$LIBS $LIBGNUTLS_LIBS -lgnutls-openssl"
  CFLAGS="$CFLAGS $LIBGNUTLS_CFLAGS"
)

if test "X$LIBGNUTLS_LIBS" == "X";then
  AC_CHECK_HEADERS([openssl/md5.h])
  AC_CHECK_LIB(crypto, MD5_Init,
	AC_DEFINE([HAVE_CRYPTO_WITH_MD5], [1], [The crypto lib has MD5 functions])
    LIBS="$LIBS -lcrypto"
  )
fi

CHECK_LIB_CARES
if test "X$SIPSAK_HAVE_ARES" == "X"; then
  CHECK_LIB_RULI
fi

SIPSAK_TIMER

AC_CONFIG_FILES([Makefile])
AC_OUTPUT