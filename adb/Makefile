# Copyright 2005 The Android Open Source Project
#
# Android.mk for adb
#

LOCAL_PATH:= $(call my-dir)

# adb host tool
# =========================================================
include $(CLEAR_VARS)

# Default to a virtual (sockets) usb interface
USB_SRCS :=
EXTRA_SRCS :=

ifeq ($(HOST_OS),linux)
  USB_SRCS := usb_linux.c
  EXTRA_SRCS := get_my_path_linux.c
  LOCAL_LDLIBS += -lrt -ldl -lpthread
  LOCAL_CFLAGS += -DWORKAROUND_BUG6558362
endif

ifeq ($(HOST_OS),darwin)
  USB_SRCS := usb_osx.c
  EXTRA_SRCS := get_my_path_darwin.c
  LOCAL_LDLIBS += -lpthread -framework CoreFoundation -framework IOKit -framework Carbon
endif

ifeq ($(OSTYPE),FreeBSD)
  USB_SRCS := usb_libusb.c
  EXTRA_SRCS := get_my_path_freebsd.c
  LOCAL_LDLIBS += -lpthread -lusb
endif

ifeq ($(HOST_OS),windows)
  USB_SRCS := usb_windows.c
  EXTRA_SRCS := get_my_path_windows.c
  EXTRA_STATIC_LIBS := AdbWinApi
  ifneq ($(strip $(USE_CYGWIN)),)
    # Pure cygwin case
    LOCAL_LDLIBS += -lpthread -lgdi32
  endif
  ifneq ($(strip $(USE_MINGW)),)
    # MinGW under Linux case
    LOCAL_LDLIBS += -lws2_32 -lgdi32
    USE_SYSDEPS_WIN32 := 1
  endif
  LOCAL_C_INCLUDES += development/host/windows/usb/api/
endif

LOCAL_SRC_FILES := \
	adb.c \
	console.c \
	transport.c \
	transport_local.c \
	transport_usb.c \
	commandline.c \
	adb_client.c \
	adb_auth_host.c \
	sockets.c \
	services.c \
	file_sync_client.c \
	$(EXTRA_SRCS) \
	$(USB_SRCS) \
	usb_vendors.c

LOCAL_C_INCLUDES += external/openssl/include

ifneq ($(USE_SYSDEPS_WIN32),)
  LOCAL_SRC_FILES += sysdeps_win32.c
else
  LOCAL_SRC_FILES += fdevent.c
endif

LOCAL_CFLAGS += -O2 -g -DADB_HOST=1 -Wall -Wno-unused-parameter
LOCAL_CFLAGS += -D_XOPEN_SOURCE -D_GNU_SOURCE -DHAVE_FORKEXEC -DHAVE_PTHREADS
LOCAL_MODULE := adb
LOCAL_MODULE_TAGS := debug

LOCAL_STATIC_LIBRARIES := libzipfile libunz libcrypto_static $(EXTRA_STATIC_LIBS)
ifeq ($(USE_SYSDEPS_WIN32),)
	LOCAL_STATIC_LIBRARIES += libcutils
endif
OBJS := $(LOCAL_SRC_FILES:.c=.o)
include $(BUILD_HOST_EXECUTABLE)

all: $(LOCAL_MODULE)

%.o: %.c
	gcc $(LOCAL_CFLAGS) -c -I../include -I. -o $@ $<

$(LOCAL_MODULE): $(OBJS)
	gcc -o $(LOCAL_MODULE) -L../libcutils $^ $(LOCAL_LDLIBS) -lssl -lcutils -lcrypto

clean:
	rm -f $(LOCAL_MODULE) *.o
