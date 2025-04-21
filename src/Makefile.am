# src/Makefile.am

AM_CPPFLAGS = @AM_CPPFLAGS@

FLXMLRPC_SRC = \
  XmlRpcClient.cpp \
  XmlRpcDispatch.cpp \
  XmlRpcMutex.cpp \
  XmlRpcServerConnection.cpp \
  XmlRpcServer.cpp \
  XmlRpcServerMethod.cpp \
  XmlRpcSocket.cpp \
  XmlRpcSource.cpp \
  XmlRpcUtil.cpp \
  XmlRpcValue.cpp

lib_LTLIBRARIES = libflxmlrpc.la


#libflxmlrpc_la_LDFLAGS = -version-info $(PACKAGE_CURRENT):$(PACKAGE_REVISION):$(PACKAGE_AGE)
libflxmlrpc_la_LDFLAGS = -version-info $(API_VERSION)
libflxmlrpc_la_SOURCES = $(FLXMLRPC_SRC)
libflxmlrpc_la_LIBADD = $(MATH_LIBS)
