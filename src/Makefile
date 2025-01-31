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
# Makefile for Building Embedded & Host Applications
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#   build        - Compiles and links all source files into an executable
#   compile-all  - Compiles all source files but does NOT link
#   clean        - Removes all generated files (.o, .i, .asm, .map, .out)
#   %.o         - Generates object files
#   %.i         - Generates preprocessed files
#   %.asm       - Generates assembly files
#
# Platform Overrides:
#   PLATFORM=HOST   - Builds for x86 host machine using gcc
#   PLATFORM=MSP432 - Builds for MSP432 embedded system using arm-none-eabi-gcc
#
#------------------------------------------------------------------------------

# Platform Overrides
ifndef PLATFORM
    PLATFORM = HOST
endif
include sources.mk



# Architectures Specific Flags
ifeq ($(PLATFORM), MSP432)
  CC = arm-none-eabi-gcc
  LD = arm-none-eabi-ld
  LINKER_FILE = ../msp432p401r.lds
  CPU = -mcpu=cortex-m4
  ARCH = -mthumb -march=armv7e-m -mfpu=fpv4-sp-d16 -mfloat-abi=hard
  SPECS = --specs=nosys.specs
  LDFLAGS = -T $(LINKER_FILE) $(CPU) $(ARCH) $(SPECS) -Wl,-Map=$(MAP_FILE)
  INCLUDES = -I../include/common -I../include/msp432 -I../include/CMSIS
  CPPFLAGS = -DMSP432
  CFLAGS = -Wall -Werror -g -O0 -std=c99 $(INCLUDES) $(CPPFLAGS) \
             -mthumb -mcpu=cortex-m4 -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16
else
  CC = gcc
  LD = gcc
  LDFLAGS = -Wl,-Map=$(MAP_FILE)
  INCLUDES = -I../include/common
  CPPFLAGS = -DHOST
  CFLAGS = -Wall -Werror -g -O0 -std=c99 $(INCLUDES) $(CPPFLAGS)
endif


# Define Object Files
OBJECTS = $(SOURCES:.c=.o)
EXECUTABLE = c1m2.out
MAP_FILE = c1m2.map

# Build Targets
.PHONY: all build clean compile-all

all: build

compile-all: $(OBJECTS)

%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

%.i: %.c
	$(CC) $(CFLAGS) -E -MMD -MP $< -o $@

%.asm: %.c
	$(CC) $(CFLAGS) -S $< -o $@


build: $(EXECUTABLE)

$(EXECUTABLE): $(OBJECTS)
	$(CC) $(CFLAGS) $(LDFLAGS) $(OBJECTS) -o $(EXECUTABLE) -Wl,-Map=$(MAP_FILE)

clean:
	rm -f $(OBJECTS) $(EXECUTABLE) $(MAP_FILE) *.i *.asm *.o *.d *.elf




