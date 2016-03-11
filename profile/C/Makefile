CC = gcc 
OPT_FLAGS = -O0

SOURCE = $(wildcard *.c)
OBJECTS = $(patsubst %.c, %.o, $(SOURCE))
EXECUTABLES = $(patsubst %.c, %.e, $(SOURCE))


objects: $(OBJECTS)

all: me_1 me_2

me_1 : profile_me_1.e

me_2 : profile_me_2.e

%.o: %.c
	$(CC) -c $^ -pg $(OPT_FLAGS) -o $@

%.e: %.o
	$(CC) $^ -lm -o $@ 

clean:
	rm -rfv $(OBJECTS) $(EXECUTABLES)
