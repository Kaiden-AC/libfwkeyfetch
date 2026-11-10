AM_CFLAGS = -I$(top_srcdir)/include $(GLOBAL_CFLAGS) $(libcurl_CFLAGS) $(zlib_CFLAGS) $(openssl_CFLAGS) -I$(top_srcdir)/external/libfragmentzip/include -I$(top_srcdir)/external/libgeneral/include -I$(top_builddir)/external/libgeneral/include -I$(top_srcdir)/external/jssy/include
AM_CXXFLAGS = $(AM_CFLAGS)
AM_LDFLAGS = $(libcurl_LIBS) $(zlib_LIBS) $(openssl_LIBS)

noinst_LIBRARIES = libjssy.a libgeneral.a libfragmentzip.a

libjssy_a_CFLAGS = $(AM_CFLAGS)
libjssy_a_SOURCES = $(top_srcdir)/external/jssy/jssy/jssy.c

# Vendored libgeneral (minimal: exception.cpp only, matching upstream
# --without-extras / BUILD_MINIMAL; libfwkeyfetch only uses macros.h).
libgeneral_a_CXXFLAGS = $(AM_CFLAGS)
libgeneral_a_SOURCES = $(top_srcdir)/external/libgeneral/libgeneral/exception.cpp

# Vendored libfragmentzip (single C source, statically linked).
libfragmentzip_a_CFLAGS = $(AM_CFLAGS)
libfragmentzip_a_SOURCES = $(top_srcdir)/external/libfragmentzip/libfragmentzip/libfragmentzip.c

lib_LTLIBRARIES = libfwkeyfetch.la

libfwkeyfetch_la_CXXFLAGS = $(AM_CFLAGS)
libfwkeyfetch_la_LIBADD = $(AM_LDFLAGS) libjssy.a libgeneral.a libfragmentzip.a
libfwkeyfetch_la_SOURCES = libfwkeyfetch.cpp
