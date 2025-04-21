## Process this file with autoconf to create configure. -*- autoconf -*-

AC_PREREQ([2.67])

## ------------------------ ##
## Autoconf initialisation. ##
## ------------------------ ##

# Release version

m4_define([package_version_major],[0])
m4_define([package_version_minor],[1])
m4_define([package_version_micro],[4])

# LDFLAGS -version-info
# This flag accepts an argument of the form ‘current[:revision[:age]]’. So, 
# passing -version-info 3:12:1 sets current to 3, revision to 12, and age to 1.
#
# If either revision or age are omitted, they default to 0. Also note that age 
# must be less than or equal to the current interface number.
#
# Start with version information of ‘0:0:0’ for each libtool library.
# Update the version information only immediately before a public release.
# More frequent updates are unnecessary, and only guarantee that the current 
# interface number gets larger faster.
#
#   If the library source code has changed at all since the last update, then 
#   increment revision (‘c:r:a’ becomes ‘c:r+1:a’).
#   If any interfaces have been added, removed, or changed since the last update, 
#   increment current, and set revision to 0.
#   If any interfaces have been added since the last public release, then 
#   increment age.
#   If any interfaces have been removed or changed since the last public release, 
#   then set age to 0. 
#
# Never try to set the interface numbers so that they correspond to the release 
# number of the package. This is an abuse that only fosters misunderstanding of 
# the purpose of library versions.

# API version
m4_define([api_version],[1:0:0])
 
AC_INIT([flxmlrpc],
        [package_version_major.package_version_minor.package_version_micro],
        [iam_w1hkj@w1hkj.com],
        [flxmlrpc],
        [http://www.w1hkj.com])

AC_CONFIG_SRCDIR([include/XmlRpc.h])
AC_CONFIG_HEADERS([include/config.h])
AC_CONFIG_MACRO_DIR([m4])

dnl Place build system provided programs in this directory.
AC_CONFIG_AUX_DIR([build-aux])

## ------------------------ ##
## Automake Initialisation. ##
## ------------------------ ##

dnl Passing AC_PACKAGE_TARNAME, AC_PACKAGE_VERSION to AM_INIT_AUTOMAKE is
dnl obsolete as these values are obtained from the AC_INIT macro.
AM_INIT_AUTOMAKE([-Wall subdir-objects])

dnl Clean compilation output makes compiler warnings more visible
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

## -------------------------------- ##
## flxmlrpc specific configuration. ##
## -------------------------------- ##

# Add flxmlrpc header include paths
AM_CPPFLAGS="${AM_CPPFLAGS} -I\$(top_srcdir)/include -I\$(top_srcdir)/src"

## --------------- ##
## Program checks  ##
## --------------- ##

dnl Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_CPP
AC_PROG_AWK
AC_PROG_INSTALL
AC_PROG_LN_S
AM_PROG_CC_C_O
AM_PROG_LIBTOOL

AC_CHECK_PROG([cf_with_cxx], ["${CXX}"], [yes], [no])
AM_CONDITIONAL([ENABLE_CXX], [test x"${cf_with_cxx}" = "xyes"])

AC_C_INLINE

dnl Check for pkg-config presence and if not installed define a dummy macro
dnl to disable libxml2 use.  Code borrowed from:
dnl http://lists.gnu.org/archive/html/automake/2011-03/msg00008.html
# Check for pkg-config program, used for configuring some libraries.
#
m4_define_default([PKG_PROG_PKG_CONFIG],
    [AC_MSG_CHECKING([pkg-config])
    AC_MSG_RESULT([no])])

PKG_PROG_PKG_CONFIG
#PKG_INSTALLDIR

## -------------------------------------------- ##
## Various parts of flxmlrpc need these headers ##
## -------------------------------------------- ##

AC_CHECK_HEADERS([errno.h fcntl.h getopt.h limits.h locale.h malloc.h \
netdb.h sgtty.h stddef.h termio.h termios.h values.h \
arpa/inet.h dev/ppbus/ppbconf.hdev/ppbus/ppi.h \
linux/hidraw.h linux/ioctl.h netinet/in.h \
sys/ioccom.h sys/ioctl.h sys/param.h sys/socket.h sys/stat.h sys/time.h])

dnl set host_os variable
AC_CANONICAL_HOST

## ------------------------------------ ##
## System capability and library checks ##
## ------------------------------------ ##

dnl If pthread support is found, this macro defines HAVE_PTHREAD and
dnl AC_SUBST's PTHREAD_LIBS, PTHREAD_CFLAGS, and PTHREAD_CC making those
dnl variables available in Makefiles.

AC_SYS_POSIX_TERMIOS()

dnl Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_HEADER_TIME
AC_CHECK_TYPES([siginfo_t],[],[],[[#include <signal.h>]])


dnl Checks for libraries.

# The following comes from SC_TCL_LINK_LIBS
# we redefine them here because we want a separate NET_LIBS var

#--------------------------------------------------------------------
# On a few very rare systems, all of the libm.a stuff is
# already in libc.a.  Set compiler flags accordingly.
#--------------------------------------------------------------------
AC_CHECK_FUNC([sin], [MATH_LIBS=""], [MATH_LIBS="-lm"])
AC_SUBST([MATH_LIBS])

#--------------------------------------------------------------------
#   Check for the existence of the -lsocket and -lnsl libraries.
#   The order here is important, so that they end up in the right
#   order in the command line generated by make.  Here are some
#   special considerations:
#   1. Use "connect" and "accept" to check for -lsocket, and
#      "gethostbyname" to check for -lnsl.
#   2. Use each function name only once:  cannot redo a check because
#      autoconf caches the results of the last check and will not redo it.
#   3. Use -lnsl and -lsocket only if they supply procedures that
#      are not already present in the normal libraries.  This is because
#      IRIX 5.2 has libraries, but they aren't needed and they're
#      bogus:  they goof up name resolution if used.
#   4. On some SVR4 systems, can't use -lsocket without -lnsl too.
#      To get around this problem, check for both libraries together
#      if -lsocket doesn't work by itself.
#--------------------------------------------------------------------
hl_checkBoth=0
hl_oldLibs=$LIBS

AC_CHECK_FUNC([connect], [hl_checkSocket=0], [hl_checkSocket=1])

AS_IF([test "$hl_checkSocket" = 1], [
        AC_CHECK_LIB([socket],
                     [main],
                     [NET_LIBS="$NET_LIBS -lsocket"],
                     [hl_checkBoth=1],
                     [])
        ])

AS_IF([test "$hl_checkBoth" = 1], [
        hl2_oldLibs=$LIBS
        LIBS="$LIBS -lsocket -lnsl"
        AC_CHECK_FUNC([accept], [hl_checkNsl=0], [LIBS=$hl2_oldLibs])
        ])

# separate Name Service Library
AC_CHECK_FUNC([gethostbyname],
              [],
              [AC_CHECK_LIB([nsl],
                            [main],
                            [NET_LIBS="$NET_LIBS -lnsl"],
                            [],
                            [])
              ])

# Winsock2 32 bit
AC_CHECK_FUNC([gethostbyname],
              [],
              [AC_CHECK_LIB([ws2_32],
                            [main],
                            [NET_LIBS="$NET_LIBS -lws2_32"],
                            [],
                            [])
              ])

LIBS="$LIBS $NET_LIBS"

## End flxmlrpc socket test ##


dnl Checks for library functions.
AC_CHECK_FUNCS([cfmakeraw floor getpagesize getpagesize gettimeofday inet_ntoa \
ioctl memchr memmove memset pow rint select setitimer setlocale sigaction \
snprintf socket sqrt strchr strdup strerror strncasecmp strrchr strstr strtol])
AC_FUNC_ALLOCA

dnl Use flxmlrpc's termios
AC_LIBOBJ([termios])

## ------------------------ ##
## libtool  Initialisation. ##
## ------------------------ ##

LT_PREREQ([2.2.6b])

dnl N.B. LT_INIT([win32-dll]) does not work for older libtool
dnl so require new enough version above.
LT_INIT([win32-dll])


## -------------------------- ##
## Custom host configurations ##
## -------------------------- ##

dnl The host_os variable is set by the AC_CANONICAL_HOST macro above.
AS_CASE(["$host_os"],
    [freebsd*], [
        AM_CPPFLAGS="-I/usr/local/include ${AM_CPPFLAGS}"
        AM_LDFLAGS="${AM_LDFLAGS} -L/usr/local/lib"
        AC_SUBST([AM_LDFLAGS])],

    [darwin* | rhapsody*], [
        # Trick from http://fink.sourceforge.net/doc/porting/
        # TODO: check the compiler actually does support these options
        AM_CFLAGS="${AM_CFLAGS} -no-cpp-precomp"
        AM_CXXFLAGS="${AM_CXXFLAGS} -no-cpp-precomp"

        # Tell the OS X linker to allocate enough space inside the
        # libflxmlrpc.X.dylib shared object for install_name_tool(1) to
        # work. This is useful when including flxmlrpc in an app bundle.
        OSXLDFLAGS="-Wl,-headerpad_max_install_names"
        AC_SUBST([OSXLDFLAGS])],

    [mingw* | pw32* | cygwin*], [
        WINLDFLAGS="-Wl,--output-def,libflxmlrpc.def -Wl,--add-stdcall-alias"
        AC_SUBST([WINLDFLAGS])

        # Enable ld's "auto import" for executables
        WINEXELDFLAGS="-Wl,--enable-auto-import"
        AC_SUBST([WINEXELDFLAGS])
    ])


## ---------------------- ##
## External package tests ##
## ---------------------- ##

dnl Check if C99 struct initializers are supported
AC_MSG_CHECKING([whether C99 struct/array initializers are supported])
AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[]],
                  [[struct{char a;int b;}s[8]={[3]={.b=5}};]])],
                  [AC_MSG_RESULT(yes)],
                  [AC_MSG_ERROR([""
"You need a C99 compliant C compiler that supports struct/array intializers."
"Have you considered GCC lately?."])
                  ])

## ------------ ##
## Final output ##
## ------------ ##

dnl Output accumulated flags to the Makefile files.
AC_SUBST([AM_CPPFLAGS])
AC_SUBST([AM_CFLAGS])
AC_SUBST([AM_CXXFLAGS])

AC_SUBST([PACKAGE_VERSION_MAJOR],package_version_major)
AC_SUBST([PACKAGE_VERSION_MINOR],package_version_minor)
AC_SUBST([PACKAGE_VERSION_MICRO],package_version_micro)
AC_SUBST([API_VERSION],api_version)

AC_CONFIG_FILES([
flxmlrpc.pc:flxmlrpc.pc.in
Makefile
m4/Makefile
include/Makefile
src/Makefile])

AC_OUTPUT

echo \
"----------------------------------------------------------------------

 ${PACKAGE_NAME} Version ${PACKAGE_VERSION} configuration:

 Prefix         ${prefix}
 Preprocessor   ${CPP} ${CPPFLAGS}
 C Compiler     ${CC} ${CFLAGS}
 C++ Compiler   ${CXX} ${CXXFLAGS}

 Package features:
    Enable shared libs              ${enable_shared}
    Enable static libs              ${enable_static}

-----------------------------------------------------------------------"
