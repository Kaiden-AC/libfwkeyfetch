AC_PREREQ([2.69])
AC_INIT([libfwkeyfetch], m4_esyscmd([git rev-list --count HEAD | tr -d '\n']), [tihmstar@gmail.com])

AC_CANONICAL_SYSTEM
AC_CANONICAL_HOST
AM_PROG_LIBTOOL


AM_INIT_AUTOMAKE([subdir-objects])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIRS([m4])

AC_DEFINE([VERSION_COMMIT_COUNT], "m4_esyscmd([git rev-list --count HEAD | tr -d '\n'])", [Git commit count])
AC_DEFINE([VERSION_COMMIT_SHA], "m4_esyscmd([git rev-parse HEAD | tr -d '\n'])", [Git commit sha])
AC_SUBST([VERSION_COMMIT_COUNT], ["m4_esyscmd([git rev-list --count HEAD | tr -d '\n'])"])
AC_SUBST([VERSION_COMMIT_SHA], ["m4_esyscmd([git rev-parse HEAD | tr -d '\n'])"])


# Checks for programs.
AC_PROG_CXX([clang++])
AC_PROG_CC([clang])

# Check for operating system
AC_MSG_CHECKING([whether we need platform-specific build settings])
case $host_os in
  darwin* )
        AC_MSG_RESULT([${host_os}])
        CXXFLAGS+=" -stdlib=libc++"
        ;;
  *mingw32*|*cygwin*|*msys*)
        AC_MSG_RESULT([${host_os}])
        win32=true
        LDFLAGS+=" -no-undefined"
        ;;
esac

CXXFLAGS+=" -std=c++11"
CFLAGS+=" -std=c11 -fvisibility=hidden"

# Checks for libraries.
# libgeneral + libfragmentzip + libjssy are vendored as git submodules under
# external/ and built statically as libgeneral.a / libfragmentzip.a /
# libjssy.a, so no system pkg-config install is required for them.
# (https://github.com/tihmstar/libgeneral,
#  https://github.com/tihmstar/libfragmentzip,
#  https://github.com/tihmstar/jssy.git)
# Standardized libs (libcurl, zlib) remain via pkg-config.
LIBCURL_REQUIRES_STR="libcurl >= 8.5.0"
LIBZ_REQUIRES_STR="zlib >= 1.0"

PKG_CHECK_MODULES(libcurl, $LIBCURL_REQUIRES_STR)
PKG_CHECK_MODULES(zlib, $LIBZ_REQUIRES_STR)

AC_SUBST([libcurl_requires], [$LIBCURL_REQUIRES_STR])
# libfragmentzip requires string kept for backwards compat of generated
# variables, but is no longer a system dep (vendored).
AC_SUBST([libfragmentzip_requires], ["libfragmentzip >= 77"])
AC_SUBST([libgeneral_requires], ["libgeneral >= 92"])
AC_SUBST([libjssy_requires], ["libjssy >= 41"])

AC_ARG_WITH([libfragmentzip],
            [AS_HELP_STRING([--without-libfragmentzip],
            [do not build with libfragmentzip support @<:@default=yes@:>@])],
            [with_libfragmentzip=no],
            [with_libfragmentzip=yes])

AC_ARG_WITH([remote-keys],
            [AS_HELP_STRING([--without-remote-keys],
            [do not request firmware keys from remote public sources @<:@default=no@:>@])],
            [with_remote_keys=no],
            [with_remote_keys=yes])

AC_ARG_ENABLE([debug],
            [AS_HELP_STRING([--enable-debug],
            [enable debug build(default is no)])],
            [debug_build=true],
            [debug_build=false])

AM_CONDITIONAL(WITH_REMOTE_KEYS, test "x$with_remote_keys" = "xyes")

if test "x$win32" = "xtrue"; then
  PKG_CHECK_MODULES(openssl, "openssl")
fi

if test "x$with_remote_keys" = "xyes"; then
  AC_DEFINE(WITH_REMOTE_KEYS, 1, [Define if you build with support for requesting keys from remote public database])
fi

if test "x$with_libfragmentzip" == "xyes"; then
  # libfragmentzip is now vendored (external/), so no system check needed.
  AC_DEFINE(HAVE_LIBFRAGMENTZIP, 1, [Define if you have libfragmentzip])
else
  echo "*** Note: libfragmentzip has been disabled ***"
fi

if test "$debug_build" = true; then
  echo "*** Note: debug build requested ***"
  CFLAGS+=" -g -O0 -DDEBUG=1"
  CXXFLAGS+=" -g -O0 -DDEBUG=1"
fi


# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

# libgeneral/macros.h is generated from macros.h.in (vendored submodule).
AC_SUBST([LIBGENERAL_CLEANUP_IS_DISABLED], [0])

AC_CONFIG_FILES([Makefile
                 include/Makefile
                 libfwkeyfetch/Makefile
                 external/libgeneral/include/libgeneral/macros.h
		             libfwkeyfetch.pc])
AC_OUTPUT

echo "
Configuration for $PACKAGE-$VERSION:
-------------------------------------------

  install prefix ..........: $prefix
  with libfragmentzip .....: $with_libfragmentzip
  with remote keys ........: $with_remote_keys
  debug build  ............: $debug_build"

echo "  compiler ................: ${CC}

  Now type 'make' to build $PACKAGE-$VERSION,
  and then 'make install' for installation.
"
