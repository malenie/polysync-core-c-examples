##########################################################
# makefile for viewer-lite
##########################################################


# source PolySync environment if not already done, assumes x86_64 if set here
# usually, the environment has these set
PSYNC_HOME ?= /usr/local/polysync
OSPL_HOME ?= $(PSYNC_HOME)/utils/x86_64.linux

# target
TARGET	:= bin/polysync-viewer-lite

# sources
SRCS    :=  src/ps_interface.c \
	    src/color.c \
	    src/ruler.c \
	    src/render.c \
	    src/ground_plane.c \
	    src/origin_model.c \
	    src/grid.c \
	    src/entity_manager.c \
	    src/gui.c \
	    src/viewer_lite.c

# object files, dep files
OBJS    := $(SRCS:.c=.o)
DEPS    := $(SRCS:.c=.dep)
XDEPS   := $(wildcard $(DEPS))

# get standard PolySync build resources
include $(PSYNC_HOME)/build_res.mk

# compiler
CC = gcc

# include GL EXT
CCFLAGS += -DGL_GLEXT_PROTOTYPES -DGLX_GLXEXT_PROTOTYPES

# includes
INCLUDE	+= -Iinclude

# libraries
LIBS	+= -lglut -lGLU -lGL -lX11 -lm -lpolysync_data_model

#
all: dirs $(TARGET)

#
ifneq ($(XDEPS),)
include $(XDEPS)
endif

# directories
dirs::
	mkdir -p doc/html
	mkdir -p doc/latex
	mkdir -p doc/pdf
	mkdir -p bin

#
$(TARGET): $(OBJS)
	$(CC) $(LDFLAGS) $(INCLUDE) -o $@ $^ $(LIBS)

#
$(OBJS): %.o: %.c %.dep
	$(CC) $(CCFLAGS) $(INCLUDE) -o $@ -c $<

#
$(DEPS): %.dep: %.c Makefile
	$(CC) $(CCFLAGS) $(INCLUDE) -MM $< > $@

#
docs: dirs
	cd doc && doxygen Doxyfile;

#
clean:
	-rm -f src/*.o
	-rm -f src/*.dep
	-rm -f $(TARGET)
	-rm -f bin/*
	-rm -rf doc/html
	-rm -rf doc/latex
	-rm -rf doc/pdf
	-rm -rf ospl-*.log
