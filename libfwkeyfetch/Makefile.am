AM_CFLAGS = -I$(top_srcdir)/include $(GLOBAL_CFLAGS) \
              $(libcurl_CFLAGS) $(libfragmentzip_CFLAGS) $(libgeneral_CFLAGS) $(libjssy_CFLAGS) $(openssl_CFLAGS)
AM_LDFLAGS = -L$(top_srcdir)/libfwkeyfetch $(GLOBAL_LDFLAGS) \
              $(libcurl_LIBS) $(libfragmentzip_LIBS) $(libgeneral_LIBS) $(libjssy_LIBS) $(openssl_LIBS)

lib_LTLIBRARIES = libfwkeyfetch.la

libfwkeyfetch_la_CXXFLAGS = $(AM_CFLAGS)
libfwkeyfetch_la_LIBADD = $(AM_LDFLAGS)
libfwkeyfetch_la_SOURCES = libfwkeyfetch.cpp
