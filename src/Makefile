CFLAGS+=$(RPM_OPT_FLAGS) -Wall -D_GNU_SOURCE -fPIE
LDFLAGS+=$(RPM_LD_FLAGS) -pie -z relro -z now

PROGS=usernetctl netreport usleep \
	consoletype genhostid rename_device
CONSOLE_INIT_OBJS=console_init.o shvar.o
USLEEP_OBJS=usleep.o

mandir=/usr/share/man

all:	$(PROGS)

clean:
	rm -f $(PROGS) *.o *~

install:
	mkdir -p $(ROOT)/usr/bin $(ROOT)/usr/sbin $(ROOT)$(mandir)/man{1,8} $(ROOT)/etc $(ROOT)/usr/lib/udev
	install -m 755 usleep $(ROOT)/usr/bin/usleep
	install -m 4755 usernetctl $(ROOT)/usr/sbin/usernetctl
	install -m 2755 netreport $(ROOT)/usr/sbin/netreport
	install -m 755 genhostid $(ROOT)/usr/sbin/genhostid
	install -m 755 consoletype $(ROOT)/usr/sbin/consoletype
	install -m 755 sushell $(ROOT)/usr/sbin/sushell
	install -m 755 rename_device $(ROOT)/usr/lib/udev/rename_device
	install -m 644 genhostid.1 $(ROOT)$(mandir)/man1
	install -m 644 netreport.1 $(ROOT)$(mandir)/man1
	install -m 644 usleep.1 $(ROOT)$(mandir)/man1
	install -m 644 usernetctl.8 $(ROOT)$(mandir)/man8
	install -m 644 consoletype.1 $(ROOT)$(mandir)/man1
	install -m 644 ifup.8 $(ROOT)$(mandir)/man8
	install -m 644 sushell.8 $(ROOT)$(mandir)/man8
	ln -s ifup.8 $(ROOT)$(mandir)/man8/ifdown.8

usleep: $(USLEEP_OBJS)
	$(CC) $(LDFLAGS) -o $@ $(USLEEP_OBJS) -lpopt

usernetctl.o: usernetctl.c
	$(CC) $(CFLAGS) -c usernetctl.c -o usernetctl.o

usernetctl: usernetctl.c usernetctl.o
	$(CC) $(LDFLAGS) -o $@ $@.o

netreport.o: netreport.c
	$(CC) $(CFLAGS) -c netreport.c -o netreport.o

netreport: netreport.o
	$(CC) $(LDFLAGS) -o $@ $@.o

shvar.o: shvar.c
	$(CC) $(CFLAGS) `pkg-config glib-2.0 --cflags` -c shvar.c -o shvar.o

rename_device: rename_device.c
	$(CC) $(CFLAGS) `pkg-config glib-2.0 --cflags` -o $@ $< $(LDFLAGS) `pkg-config glib-2.0 --libs`

