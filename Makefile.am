## Process this file with automake to produce Makefile.in

ACLOCAL_AMFLAGS = -I m4

aclocaldir = $(datadir)/aclocal
m4datadir = $(datadir)/aclocal

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = flxmlrpc.pc

EXTRA_DIST = flxmlrpc.pc.in include/Doxyfile include/index.doc test

SUBDIRS = include src

DIST_SUBDIRS = m4 include src
