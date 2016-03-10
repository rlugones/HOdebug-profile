.PHONY: default help object executable all clean segfault dynamic static nobugs
CC = gcc

LD = $(CC)

CFLAGS = -g -O0 
CFLAGS2 = -g -O0 -DTRAPFPE
LDFLAGS = 

SOURCE_C = $(wildcard *.c)
OBJECTS_C = $(patsubst %.c, %_c.o, $(SOURCE_C))
EXECUTABLES = $(patsubst %.c, %.e, $(SOURCE_C))

default: all

help:
	@echo "\
Options:\n\n\
  make all:           make every executable (default)\n\
  make fpe1:          example 1\n\
  make fpe2:          example 2\n\
  make fpe3:          example 3\n\
  make clean:         delete output files\n\
  make help:          display this help"

objects: $(OBJECTS_C)

executable: $(EXECUTABLES)

all: objects executable

fpe1: test_fpe1.e 

fpe2: test_fpe2.e 

fpe3: test_fpe3.e 

%_c.o: %.c
	$(CC) $(CFLAGS2) -c $^ -o $@

%.e: %_c.o
	$(LD) $(LDFLAGS) $^ fpe_x87_sse/fpe_x87_sse.o -lm -o $@ 

clean:
	rm -rfv $(OBJECTS_C) $(EXECUTABLES)
