MODULE := libmincrypt.a
SRC := dsa_sig.c p256.c p256_ec.c p256_ecdsa.c rsa.c sha.c sha256.c
CFLAGS := -Wall -Werror
OBJ := $(SRC:.c=.o)

all: $(MODULE)

%.o: %.c
	$(CROSS_COMPILE)gcc $(CFLAGS) -c -I../include -o $@ $<

$(MODULE): $(OBJ)
	ar rcs $@ $?
	ranlib $@

clean:
	rm -f $(MODULE) *.o
