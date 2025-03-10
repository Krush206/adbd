#
# Copyright (C) 2008 The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
LOCAL_PATH := $(my-dir)
include $(CLEAR_VARS)

ifeq ($(TARGET_CPU_SMP),true)
    targetSmpFlag := -DANDROID_SMP=1
else
    targetSmpFlag := -DANDROID_SMP=0
endif
hostSmpFlag := -DANDROID_SMP=0

commonSources := \
	hashmap.c \
	native_handle.c \
	config_utils.c \
	cpu_info.c \
	load_file.c \
	open_memstream.c \
	strdup16to8.c \
	strdup8to16.c \
	record_stream.c \
	process_name.c \
	threads.c \
	sched_policy.c \
	iosched_policy.c \
	str_parms.c \

# some files must not be compiled when building against Mingw
# they correspond to features not used by our host development tools
# which are also hard or even impossible to port to native Win32
WINDOWS_HOST_ONLY :=
ifeq ($(HOST_OS),windows)
    ifeq ($(strip $(USE_CYGWIN)),)
        WINDOWS_HOST_ONLY := 1
    endif
endif
# USE_MINGW is defined when we build against Mingw on Linux
ifneq ($(strip $(USE_MINGW)),)
    WINDOWS_HOST_ONLY := 1
endif

ifneq ($(WINDOWS_HOST_ONLY),1)
    commonSources += \
        properties.c \
        fs.c \
        multiuser.c \
        socket_inaddr_any_server.c \
        socket_local_client.c \
        socket_local_server.c \
        socket_loopback_client.c \
        socket_loopback_server.c \
        socket_network_client.c \
        sockets.c \

    commonHostSources += \
        ashmem-host.c

endif


# Static library for host
# ========================================================
LOCAL_MODULE := libcutils.a
LOCAL_SRC_FILES := $(commonSources) $(commonHostSources) dlmalloc_stubs.c
LOCAL_STATIC_LIBRARIES := liblog
LOCAL_CFLAGS += $(hostSmpFlag) -DHAVE_PTHREADS -DPROP_NAME_MAX=32 -DPROP_VALUE_MAX=92
ifneq ($(HOST_OS),windows)
LOCAL_CFLAGS += -Werror
endif
LOCAL_MULTILIB := both
LOCAL_ADDITIONAL_DEPENDENCIES := $(LOCAL_PATH)/Android.mk
OBJS := $(LOCAL_SRC_FILES:.c=.o)
include $(BUILD_HOST_STATIC_LIBRARY)

all: $(LOCAL_MODULE)

%.o: %.c
	gcc $(LOCAL_CFLAGS) -c -I../include -o $@ $<

$(LOCAL_MODULE): $(OBJS)
	ar rcs $@ $?
	ranlib $@

clean:
	rm -f $(LOCAL_MODULE) *.o
