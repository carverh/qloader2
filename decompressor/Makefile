CC = i386-elf-gcc
LD = i386-elf-gcc
OBJCOPY = i386-elf-objcopy

CFLAGS = -flto -Os -pipe -Wall -Wextra

INTERNAL_CFLAGS =  \
	-std=gnu11 \
	-ffreestanding \
	-fno-stack-protector \
	-fno-pic \
	-fomit-frame-pointer \
	-masm=intel \
	-mno-80387 \
	-mno-mmx \
	-mno-3dnow \
	-mno-sse \
	-mno-sse2 \
	-I. \
	-Wno-address-of-packed-member

LDFLAGS = -flto -Os

INTERNAL_LDFLAGS = \
	-lgcc \
	-static-libgcc \
	-nostdlib \
	-no-pie \
	-static \
	-Tlinker.ld

.PHONY: all clean

C_FILES := $(shell find ./ -type f -name '*.c' | sort)
ASM_FILES := $(shell find ./ -type f -name '*.asm' | sort)
OBJ := $(ASM_FILES:.asm=.o) $(C_FILES:.c=.o)

all: decompressor.bin

decompressor.bin: $(OBJ)
	$(LD) $(OBJ) $(LDFLAGS) $(INTERNAL_LDFLAGS) -o decompressor.elf
	$(OBJCOPY) -O binary decompressor.elf decompressor.bin

%.o: %.c
	$(CC) $(CFLAGS) $(INTERNAL_CFLAGS) -c $< -o $@

%.o: %.asm
	nasm $< -f elf32 -o $@

clean:
	rm -f decompressor.bin decompressor.elf $(OBJ)