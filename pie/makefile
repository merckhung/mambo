CC=aarch64-linux-gnu-gcc
LD=aarch64-linux-gnu-ld

CFLAGS= -Os -Wall -g -std=c99 #-DPIE_AUTOINC

C_ARCH = $(shell $(CC) -dumpmachine | awk -F '-' '{print $$1}')
ifeq ($(findstring arm, $(C_ARCH)), arm)
	NATIVE_TARGETS = arm thumb
endif
ifeq ($(C_ARCH),aarch64)
	NATIVE_TARGETS = a64
endif

.SECONDARY:
.PHONY: native print_arch all pie clean

native: print_arch $(NATIVE_TARGETS)
	
print_arch:
	$(info PIE: detected architecture "$(C_ARCH)")

all: thumb arm a64

%:
	$(MAKE) --no-print-directory ARCH=$@ pie

pie: pie-$(ARCH)-decoder.o pie-$(ARCH)-encoder.o pie-$(ARCH)-field-decoder.o

pie-$(ARCH)-%.o: pie-$(ARCH)-%.c pie-$(ARCH)-%.h
	$(CC) -c $(CFLAGS) $< -o $@

pie-$(ARCH)-%.h: generate_%.rb $(ARCH).txt
	ruby $< $(ARCH) header > $@

pie-$(ARCH)-%.c: generate_%.rb $(ARCH).txt
	ruby $< $(ARCH) > $@

clean:
	rm -f *.o pie-arm-*.h pie-thumb-*.h pie-a64-*.h pie-*.c

