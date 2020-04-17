#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.o - Builds <FILE>.o object file
#      build - Builds and links all source files
#      all - Same as build
#      clean - removes all generated files
#	   compile-all - Compile all object files, but DO NOT link
#
# Platform Overrides:
#      CPU - ARM Cortex Architecture (cortex-m0plus, cortex-m4)
#      ARCH - ARM Architecture (arm, thumb)
#      SPECS - Specs file to give the linker (nosys.specs, nano.specs)
#
#------------------------------------------------------------------------------
PLATFORM = HOST

ifeq ($(PLATFORM),HOST)
	include sources_HOST.mk
else
	include sources_MSP432.mk
endif

# Architecture Specific Flags
CPU = cortex-m4
ARCH = thumb
SPECS = nosys.specs

# Platform Specific Flags
LINKER_FILE = ../msp432p401r.lds 

# Compile Defines
ifeq ($(PLATFORM),HOST)  					
	CC = gcc -D$(PLATFORM)
	CFLAGS = -Wall
	LDFLAGS = -Wl,-Map=$(TARGET).map
	CPPFLAGS = -std=c99 -Werror -E
	ASFLAGS = -g -Wall -S
else
	CC = arm-none-eabi-gcc -D$(PLATFORM)
	LD = arm-none-eabi-ld
	CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -Wall
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CPPFLAGS = -mcpu=$(CPU) -m$(ARCH) -std=c99 -Werror -E
	ASFLAGS = -g -std=c99 -Wall -S
endif
TARGET = c1m2

OBJS = $(SRCS:.c=.o)

%.o : %.c
	$(CC) -c $< $(CFLAGS) $(INCLUDES) -o $@

CPPFS = $(SRCS:.c=.i)

%.i : %.c
	$(CC) $(INCLUDES) $(CPPFLAGS) $< -o $@

ASFS = $(SRCS:.c=.asm)

%.asm : %.c
	$(CC) $(INCLUDES) $(ASFLAGS) -o $@ $<


.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map $(CPPFS) $(ASFS)

.PHONY: compile-all
compile-all: $(OBJS)
