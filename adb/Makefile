PROG := adbd
SRC := adb.c \
       fdevent.c \
       transport.c \
       transport_local.c \
       transport_usb.c \
       adb_auth_client.c \
       sockets.c \
       services.c \
       file_sync_service.c \
       jdwp_service.c \
       framebuffer_service.c \
       remount_service.c \
       usb_linux_client.c
CFLAGS := -O2 \
	  -g \
	  -DADB_HOST=0 \
	  -Wall \
	  -Wno-unused-parameter \
	  -DPROP_NAME_MAX=32 \
	  -DPROP_VALUE_MAX=92 \
	  -D_XOPEN_SOURCE=1 \
	  -D_GNU_SOURCE=1 \
	  -DHAVE_PTHREADS=1 \
	  -DALLOW_ADBD_ROOT=1
OBJ := $(SRC:.c=.o)

all: $(PROG)

%.o: %.c
	$(CROSS_COMPILE)gcc $(CFLAGS) -c -I../include -I. -o $@ $<

$(PROG): $(OBJ)
	$(CROSS_COMPILE)gcc -static -o $@ -L../libcutils \
					  -L../liblog \
					  -L../libmincrypt $^ -lcutils \
							      -llog \
							      -lmincrypt \
							      -lresolv \
							      -lselinux \
							      -lpcre2-8

clean:
	rm -f $(PROG) *.o
