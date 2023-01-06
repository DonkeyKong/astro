CC = gcc
BUILD ?= debug

NOVAS = novas.a
NOVASPATH = ../$(NOVAS_CDIST)
NOVASLIB = $(NOVASPATH)/$(NOVAS)
EPHUT = ephutil.a
EPHUTPATH = ../ephutil
EPHUTLIB = $(EPHUTPATH)/$(EPHUT)

CFLAGS = -std=c99 -Wall -Werror -Wextra -D_POSIX_SOURCE -D_C99_SOURCE -I $(NOVASPATH) -I $(EPHUTPATH)
LDLIBS = -lm -lcurl

TARGET = planets
SOURCES = planets.c

OBJECTS = $(SOURCES:%.c=%.o)

ifeq ($(BUILD),debug)
CFLAGS += -g
else
ifeq ($(BUILD),release)
CFLAGS += -O3 -DNDEBUG
endif
endif

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJECTS) $(NOVASLIB) $(EPHUTLIB)

$(NOVASLIB):
	make -C $(NOVASPATH) $(NOVAS)

$(EPHUTLIB):
	make -C $(EPHUTPATH) $(EPHUT)

clean:
	-rm -f $(TARGET) $(OBJECTS) $(SOURCES:%.c=%.d)
	make -C ../$(NOVAS_CDIST) clean
	make -C ../ephutil clean

### Dependencies ###

%.d: %.c
	@set -e; rm -f $@; \
	$(CC) -MM $(CFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

ifneq ($(MAKECMDGOALS),clean)
include $(SOURCES:%.c=%.d)
endif

# vim: set noexpandtab tabstop=4 shiftwidth=4 softtabstop=4:
