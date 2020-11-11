include Makefile.version
ASSET_DIR_PATH="\"$(shell pwd)/assets\""
CC=gcc
CFLAGS=-Wall `pkg-config --cflags sdl2` -I `heptc -where`/c \
	-D ASSET_DIR_PATH=$(ASSET_DIR_PATH) -D YEAR=$(ANNEE) \
	-D VERSION=$(VERSION) -g -fsanitize=undefined
LDFLAGS=`pkg-config --libs sdl2` -lm -fsanitize=undefined
HEPTC?=heptc

HEPT_OBJ=\
	src/challenge.o	\
	src/city.o		\
	src/globals.o		\
	src/vehicle.o		\
	src/control.o		\
	src/utilities.o	\
	src/challenge_types.o	\
	src/city_types.o	\
	src/globals_types.o	\
	src/vehicle_types.o	\
	src/control_types.o	\
	src/utilities_types.o
OBJ=$(HEPT_OBJ) \
	src/buffer.o		\
	src/trace_lib.o	\
	src/trace.o		\
	src/debug.o		\
	src/mathext.o		\
	src/map.o		\
	src/cutils.o		\
	src/simulation_loop.o	\
	src/challenge.o	\
	src/main.o
TARGET=scontest

.SUFFIXES:
.PHONY: all clean test
.PRECIOUS: %.epci %.c %.h
.SUFFIXES:

all: $(TARGET)

clean:
	rm -f $(OBJ) $(TARGET)
	rm -f src/*.{mls,obc,epci,epo,log}
	rm -rf src/*_c
	rm -f $(subst .o,.c,$(HEPT_OBJ))
	rm -f $(subst .o,.h,$(HEPT_OBJ))

test: $(TARGET)
	./$< -o logs.txt assets/00.map

$(TARGET): $(OBJ)
	$(CC) $(LDFLAGS) $^ -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

%_types.c %.c %.h %.epci &: %.ept src/mathext.epci src/debug.epci src/trace.epci
	cd `dirname $<` && $(HEPTC) -c -target c `basename $<`
	N=`dirname $<`/`basename $< .ept`; cp $${N}_c/*.{c,h} src

%.epci: %.epi
	cd `dirname $<` && $(HEPTC) `basename $<`

src/vehicle.epci: src/globals.epci src/utilities.epci src/control.epci
src/utilities.epci: src/globals.epci
src/control.epci: src/globals.epci src/utilities.epci
src/city.epci: src/globals.epci src/utilities.epci src/vehicle.epci src/map.epci
src/map.epci: src/map.epi src/globals.epci
src/challenge.epci: src/vehicle.epci src/city.epci src/map.epi
src/main.o src/map.o: src/globals.epci
src/simulation_loop.o: src/globals.epci src/challenge.epci
