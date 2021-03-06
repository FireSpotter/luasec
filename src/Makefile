CMOD=ssl.so
LMOD=ssl.lua

OBJS= \
 x509.o    \
 context.o \
 reentrant.o \
 ssl.o     \
 config.o  \
 ec.o

LIBS=-lssl -lcrypto -lluasocket -lpthread

WARN=-Wall -pedantic

BSD_CFLAGS=-O2 -fPIC $(WARN) $(INCDIR) $(DEFS)
BSD_LDFLAGS=-O -fPIC -shared $(LIBDIR)

LNX_CFLAGS=-O2 -fPIC $(WARN) $(INCDIR) $(DEFS)
LNX_LDFLAGS=-O -fPIC -shared $(LIBDIR)

MAC_ENV=env MACOSX_DEPLOYMENT_TARGET='$(MACVER)'
MAC_CFLAGS=-O2 -fno-common $(WARN) $(INCDIR) $(DEFS)
MAC_LDFLAGS=-bundle -undefined dynamic_lookup $(LIBDIR)

INSTALL  = install
CC      ?= cc
LD      ?= $(MYENV) cc
CFLAGS  += $(MYCFLAGS)
LDFLAGS += $(MYLDFLAGS)

.PHONY: all clean install none linux bsd macosx luasocket

all:

install: $(CMOD) $(LMOD)
	$(INSTALL) -d $(DESTDIR)$(LUAPATH)/ssl $(DESTDIR)$(LUACPATH)
	$(INSTALL) $(CMOD) $(DESTDIR)$(LUACPATH)
	$(INSTALL) -m644 $(LMOD) $(DESTDIR)$(LUAPATH)
	$(INSTALL) -m644 https.lua $(DESTDIR)$(LUAPATH)/ssl

linux:
	@$(MAKE) $(CMOD) MYCFLAGS="$(LNX_CFLAGS)" MYLDFLAGS="$(LNX_LDFLAGS)" EXTRA="$(EXTRA)"

bsd:
	@$(MAKE) $(CMOD) MYCFLAGS="$(BSD_CFLAGS)" MYLDFLAGS="$(BSD_LDFLAGS)" EXTRA="$(EXTRA)"

macosx:
	@$(MAKE) $(CMOD) MYCFLAGS="$(MAC_CFLAGS)" MYLDFLAGS="$(MAC_LDFLAGS)" MYENV="$(MAC_ENV)" EXTRA="$(EXTRA)"

luasocket:
	@cd luasocket && $(MAKE)

$(CMOD): $(EXTRA) $(OBJS)
	$(LD) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)

clean:
	cd luasocket && $(MAKE) clean
	rm -f $(OBJS) $(CMOD)

ec.o: ec.c ec.h
x509.o: x509.c x509.h compat.h
context.o: context.c context.h ec.h compat.h
ssl.o: ssl.c ssl.h context.h x509.h compat.h
reentrant.o: reentrant.c reentrant.h
config.o: config.c ec.h options.h compat.h
