MODULE := libcutils.a
SRC := properties.c \
       fs.c \
       multiuser.c \
       socket_inaddr_any_server.c \
       socket_local_client.c \
       socket_local_server.c \
       socket_loopback_client.c \
       socket_loopback_server.c \
       socket_network_client.c \
       sockets.c \
       ashmem-host.c
CFLAGS := -DANDROID_SMP=1 \
	  -DHAVE_PTHREADS=1 \
	  -DPROP_NAME_MAX=32 \
	  -DPROP_VALUE_MAX=92
OBJ := $(SRC:.c=.o)

all: $(MODULE)

%.o: %.c
	$(CROSS_COMPILE)gcc $(CFLAGS) -c -I../include -o $@ $<

$(MODULE): $(OBJ)
	ar rcs $@ $?
	ranlib $@

clean:
	rm -f $(MODULE) *.o
