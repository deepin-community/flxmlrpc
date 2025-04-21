## Please update this variable if any new macros are created

MACROS = 

EXTRA_DIST=$(MACROS)
MAINTAINERCLEANFILES=

@MAINT@macros.dep: Makefile.am
@MAINT@ @echo '$$(top_srcdir)/aclocal.m4: $(MACROS:%=m4/%)' > $@
