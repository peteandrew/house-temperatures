PROJECT = temperature_humidity_node

MCU = attiny24
F_CPU = 8000000

CC=avr-gcc
OBJCOPY=avr-objcopy
AVRDUDE = avrdude

CFLAGS = -mmcu=$(MCU) -DF_CPU=$(F_CPU)UL -Os -I.
CFLAGS += -Wall -Wstrict-prototypes
CFLAGS += -std=gnu99


all: $(PROJECT).hex

$(PROJECT).hex: $(PROJECT).elf
	$(OBJCOPY) -R .eeprom -O ihex $(PROJECT).elf $(PROJECT).hex

$(PROJECT).elf: $(PROJECT).c rfm69_comms.c rfm69_comms.h spi.c spi.h
	$(CC) $(CFLAGS) $(PROJECT).c rfm69_comms.c spi.c -o $(PROJECT).elf

$(PROJECT).lst: $(PROJECT).elf
	$(OBJCOPY) -S $(PROJECT).elf $(PROJECT).lst

flash: $(PROJECT).hex
	$(AVRDUDE) -P /dev/ttyACM0 -c avrisp -b 19200 -p $(MCU) -U flash:w:$(PROJECT).hex

clean:
	rm -f *.hex *.elf *.lst
