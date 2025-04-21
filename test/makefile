# makefile written for gnu make
CXX			= g++ 

DEBUG		= -g
OPTIMIZE	= -O2
GCCWARN		= -Wall
CPPFLAGS	= `pkg-config flxmlrpc --cflags`
CXXFLAGS	= $(DEBUG) $(GCCWARN) $(OPTIMIZE) $(CPPFLAGS)
LDLIBS		= `pkg-config flxmlrpc --libs`

TESTS		= HelloClient HelloServer TestBase64Client TestBase64Server TestValues TestXml Validator

all:		$(TESTS)

$(TESTS):

clean:
		rm -f *.o
		rm -f $(TESTS)
