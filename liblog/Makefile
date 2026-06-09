MODULE := liblog.a
SRC := logd_write.c logprint.c event_tag_map.c fake_log_device.c
CFLAGS := -DHAVE_PTHREADS=1 -DFAKE_LOG_DEVICE=1 -Werror
OBJ := $(SRC:.c=.o)

all: $(MODULE)

%.o: %.c
	$(CROSS_COMPILE)gcc $(CFLAGS) -c -I../include -o $@ $<

$(MODULE): $(OBJ)
	ar rcs $@ $?
	ranlib $@

clean:
	rm -f $(MODULE) *.o
