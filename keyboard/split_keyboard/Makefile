#----------------------------------------------------------------------------
# On command line:
#
# make all = Make software.
#
# make clean = Clean out built project files.
#
# make coff = Convert ELF to AVR COFF.
#
# make extcoff = Convert ELF to AVR Extended COFF.
#
# make program = Download the hex file to the device.
#                Please customize your programmer settings(PROGRAM_CMD)
#
# make teensy = Download the hex file to the device, using teensy_loader_cli.
#               (must have teensy_loader_cli installed).
#
# make dfu = Download the hex file to the device, using dfu-programmer (must
#            have dfu-programmer installed).
#
# make flip = Download the hex file to the device, using Atmel FLIP (must
#             have Atmel FLIP installed).
#
# make dfu-ee = Download the eeprom file to the device, using dfu-programmer
#               (must have dfu-programmer installed).
#
# make flip-ee = Download the eeprom file to the device, using Atmel FLIP
#                (must have Atmel FLIP installed).
#
# make debug = Start either simulavr or avarice as specified for debugging, 
#              with avr-gdb or avr-insight as the front end for debugging.
#
# make filename.s = Just compile filename.c into the assembler code only.
#
# make filename.i = Create a preprocessed source file for use in submitting
#                   bug reports to the GCC project.
#
# To rebuild project do "make clean" then "make all".
#----------------------------------------------------------------------------

# Target file name (without extension).
TARGET = split_keyboard

# Directory common source filess exist
TMK_DIR = ../../tmk_core

# Directory keyboard dependent files exist
TARGET_DIR = .

# project specific files
SRC =	matrix.c \
	i2c.c \
	serial.c \
	split-util.c \
	led.c

CONFIG_H = config.h

# MCU name
MCU = atmega32u4

COM_PORT=/dev/ttyACM0
PROGRAM_CMD=sleep 3; avrdude -p atmega32u4 -P $(COM_PORT) -c avr109 -U flash:w:$(TARGET).hex

# Processor frequency.
#     This will define a symbol, F_CPU, in all source code files equal to the
#     processor frequency in Hz. You can then use this symbol in your source code to
#     calculate timings. Do NOT tack on a 'UL' at the end, this will be done
#     automatically to create a 32-bit value in your source code.
#
#     This will be an integer division of F_USB below, as it is sourced by
#     F_USB after it has run through any CPU prescalers. Note that this value
#     does not *change* the processor frequency - it should merely be updated to
#     reflect the processor speed set externally so that the code can use accurate
#     software delays.
F_CPU = 16000000


#
# LUFA specific
#
# Target architecture (see library "Board Types" documentation).
ARCH = AVR8

# Input clock frequency.
#     This will define a symbol, F_USB, in all source code files equal to the
#     input clock frequency (before any prescaling is performed) in Hz. This value may
#     differ from F_CPU if prescaling is used on the latter, and is required as the
#     raw input clock is fed directly to the PLL sections of the AVR for high speed
#     clock generation for the USB and other AVR subsections. Do NOT tack on a 'UL'
#     at the end, this will be done automatically to create a 32-bit value in your
#     source code.
#
#     If no clock division is performed on the input clock inside the AVR (via the
#     CPU clock adjust registers or the clock division fuses), this will be equal to F_CPU.
F_USB = $(F_CPU)

# Interrupt driven control endpoint task(+60)
OPT_DEFS += -DINTERRUPT_CONTROL_ENDPOINT


# Boot Section Size in *bytes*
#   Teensy halfKay   512
#   Teensy++ halfKay 1024
#   Atmel DFU loader 4096
#   LUFA bootloader  4096
#   USBaspLoader     2048
OPT_DEFS += -DBOOTLOADER_SIZE=4096

# Changes some bootmagic settings for when the space is on the left half
OPT_DEFS += -DSPACE_ON_LEFT_HALF

# # Use I2C for communication between the halves of the keyboard
# OPT_DEFS += -DUSE_I2C

# Build Options
#   comment out to disable the options.
#
BOOTMAGIC_ENABLE = yes	# Virtual DIP switch configuration(+1000)
MOUSEKEY_ENABLE = yes	# Mouse keys(+4700)
EXTRAKEY_ENABLE = yes	# Audio control and System control(+450)
CONSOLE_ENABLE = yes	# Console for debug(+400)
COMMAND_ENABLE = yes    # Commands for debug and configuration
SLEEP_LED_ENABLE = yes  # Breathing sleep LED during USB suspend
NKRO_ENABLE = yes	# USB Nkey Rollover
ACTIONMAP_ENABLE = yes	# Use 16bit action codes in keymap instead of 8bit keycodes


ifdef ACTIONMAP_ENABLE
    KEYMAP_FILE = actionmap
else
    KEYMAP_FILE = keymap
    SRC := keymap_common.c $(SRC)
endif
ifdef KEYMAP
    SRC := $(KEYMAP_FILE)_$(KEYMAP).c $(SRC)
else
    SRC := $(KEYMAP_FILE)_plain.c $(SRC)
endif


#PS2_MOUSE_ENABLE = yes  # PS/2 mouse(TrackPoint) support
#PS2_USE_BUSYWAIT = yes # uses primitive reference code
#PS2_USE_INT = yes      # uses external interrupt for falling edge of PS/2 clock pin
#PS2_USE_USART = yes     # uses hardware USART engine for PS/2 signal receive(recomened)


# Search Path
VPATH += $(TARGET_DIR)
VPATH += $(TMK_DIR)

# Un comment this line if you want to use pjrc protocol
# include $(TMK_DIR)/protocol/pjrc.mk

include $(TMK_DIR)/protocol/lufa.mk
include $(TMK_DIR)/common.mk
include $(TMK_DIR)/rules.mk


debug-on: EXTRAFLAGS += -DDEBUG -DDEBUG_ACTION
debug-on: all

debug-off: EXTRAFLAGS += -DNO_DEBUG -DNO_PRINT
debug-off: OPT_DEFS := $(filter-out -DCONSOLE_ENABLE,$(OPT_DEFS))
debug-off: all

eeprom-left:
	sleep 3; avrdude -p atmega32u4 -P $(COM_PORT) -c avr109 -U eeprom:w:eeprom-lefthand.eep

eeprom-right:
	sleep 3; avrdude -p atmega32u4 -P $(COM_PORT) -c avr109 -U eeprom:w:eeprom-righthand.eep
