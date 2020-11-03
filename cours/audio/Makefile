HEPTC=heptc
HEPTLIB=$(shell heptc -where)
CC=gcc
CFLAGS=-g $(shell pkg-config --cflags --libs sdl2 sndfile) -I$(HEPTLIB)/c -lm

TARGET=audio

.PHONY: all clean test

all: $(TARGET)

clean:
	rm -f $(TARGET) *.{epci,log,mls,obc}
	rm -rf audio_c

test: $(TARGET)
	./$(TARGET)

$(TARGET): audio_c/audio_types.c audio_c/audio.c main.c mathext.c
	@pkg-config --exists sdl2 || \
		( echo "La bibliothèque SDL2 est absente."; exit 1 )
	@pkg-config --exists sndfile || \
		( echo "La bibliothèque sndfile est absente."; exit 1 )
	$(CC) $(CFLAGS) -o $@ -I audio_c -I. $^

audio_c/audio_types.c audio_c/audio.c: audio.ept main.c mathext.epci
	$(HEPTC) -target c -O $<

%.epci: %.epi
	$(HEPTC) $<
