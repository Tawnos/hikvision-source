#
# generate source file list
#
prjPath = ../../..
include $(prjPath)/Rules.make

####gcov Test,Add by zhubin 20130515 ######
ifeq ($(support_gcov), y)
NTFS3GLIB_VPATH = $(GLOBAL_VPATH)/storage/ntfs-3glib
VPATH = $(NTFS3GLIB_VPATH)/libfuse-lite $(NTFS3GLIB_VPATH)/libntfs-3g $(NTFS3GLIB_VPATH)/ntfsprogs $(NTFS3GLIB_VPATH)/src
else
VPATH = libfuse-lite libntfs-3g ntfsprogs src
endif

GLOBAL_PREFIX ?= ERROR
GLOBAL_FLAG ?= ERROR
		
SOURCES = $(foreach dir,$(VPATH),$(wildcard $(dir)/*))

C_SRCS   = $(filter %.c, $(SOURCES))
CPP_SRCS = $(filter %.cpp,$(SOURCES))

C_OBJS   = $(C_SRCS:%.c=%.o)
CPP_OBJS = $(CPP_SRCS:%.cpp=%.o)

####gcov Test,Add by zhubin 20130515 ######
ifeq ($(support_gcov), y)
INCLUDE_DIR = -I$(NTFS3GLIB_VPATH)
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/include
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/include/fuse-lite
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/include/ntfs-3g
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/libntfs-3g
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/ntfsprogs
INCLUDE_DIR += -I$(NTFS3GLIB_VPATH)/src
INCULDE_DIR += -I$(NTFS3GLIB_VPATH)/libfuse-lite
INCLUDE_DIR += -I$(GLOBAL_VPATH)/include
else
INCLUDE_DIR = -I.
INCLUDE_DIR += -Iinclude
INCLUDE_DIR += -Iinclude/fuse-lite
INCLUDE_DIR += -Iinclude/ntfs-3g
INCLUDE_DIR += -Ilibntfs-3g
INCLUDE_DIR += -Intfsprogs
INCLUDE_DIR += -Isrc
INCULDE_DIR += -Ilibfuse-lite
INCLUDE_DIR += -I$(prjPath)/include
endif


TOOL_PREFIX = $(GLOBAL_PREFIX)

CPP = $(TOOL_PREFIX)g++
CC	= $(TOOL_PREFIX)gcc
AR = $(TOOL_PREFIX)ar
RANLIB = $(TOOL_PREFIX)ranlib

####gcov Test,Add by zhubin 20130515 ######
ifeq ($(support_gcov), y)
CFLAGS = -g -D_REENTRANT -Wall -DDEV -DOS_POSIX -DHAVE_CONFIG_H
CFLAGS += -mlong-calls
else
CFLAGS = -g -D_REENTRANT -O2 -Wall -DDEV -DOS_POSIX -DHAVE_CONFIG_H
endif
#_BigBufTest_ just for epoll mode, if define _BigBufTest_ without _RTSP_EPOLL_, error!!!!
CFLAGS += #-D_BigBufTest_
#CFLAGS += -Wpadded -Wformat -Wsign-compare -Wconversion -Wpointer-arith -Wswitch-default
CFLAGS += $(GLOBAL_FLAG)


TARGET = libNTFS.a

all : $(TARGET) 

$(TARGET) : $(C_OBJS) $(CPP_OBJS)
	$(AR) r $@ $(C_OBJS) $(CPP_OBJS)
	$(RANLIB) $@
	cp $(TARGET) $(GLOBAL_LIB) -f
	#rm $(TARGET) -f

%.o:%.c
	@$(CC) $(CFLAGS) $(INCLUDE_DIR) -c $^ -o $@
%.o:%.cpp
	@$(CPP) $(CFLAGS) $(INCLUDE_DIR) -c $^ -o $@	

clean :
	rm -f $(LIB) $(C_OBJS) $(CPP_OBJS)



