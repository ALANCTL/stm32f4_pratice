#======================================================================#
#Output files
EXECUTABLE=stm32_executable.elf
BIN_IMAGE=stm32_bin_image.bin

#======================================================================#
#Cross Compiler
CC=arm-none-eabi-gcc
OBJCOPY=arm-none-eabi-objcopy

#======================================================================#
#Flags
CFLAGS=-g -mlittle-endian -mthumb \
	-mcpu=cortex-m4 \
	-mfpu=fpv4-sp-d16 -mfloat-abi=hard \
	--specs=nano.specs \
	--specs=nosys.specs
CFLAGS+=-D USE_STDPERIPH_DRIVER
CFLAGS+=-D STM32F4xx
CFLAGS+=-D __FPU_PRESENT=1 \
        -D ARM_MATH_CM4 \
	-D __FPU_USED=1
CFLAGS+=-I./


#stm32-flash
CFLAGS+=-Wl,-T,stm32_flash.ld

#======================================================================#
#lib

#Stm32 libraries
ST_LIB=./lib/STM32F4xx_StdPeriph_Driver
CFLAGS+=-I./lib/CMSIS/ST/STM32F4xx/Include

#CMSIS libraries
CFLAGS+=-I./lib/CMSIS/Include

#StdPeriph includes
CFLAGS+=-I$(ST_LIB)/inc

#======================================================================#
#Source code
SRC=./lib/CMSIS/system_stm32f4xx.c

#StdPeriph
SRC+=$(ST_LIB)/src/misc.c \
	$(ST_LIB)/src/stm32f4xx_rcc.c \
	$(ST_LIB)/src/stm32f4xx_dma.c \
	$(ST_LIB)/src/stm32f4xx_flash.c \
	$(ST_LIB)/src/stm32f4xx_gpio.c \
	$(ST_LIB)/src/stm32f4xx_usart.c \
	$(ST_LIB)/src/stm32f4xx_tim.c\
	$(ST_LIB)/src/stm32f4xx_spi.c\
	$(ST_LIB)/src/stm32f4xx_i2c.c

#Major programs
SRC+=./main.c

#======================================================================#
#STM32 startup file
STARTUP=./startup_stm32f4xx.s

#======================================================================#
#Make rules

#Make all
all:$(BIN_IMAGE)

$(BIN_IMAGE):$(EXECUTABLE)
	$(OBJCOPY) -O binary $^ $@

STARTUP_OBJ = startup_stm32f4xx.o

$(STARTUP_OBJ): $(STARTUP) 
	$(CC) $(CFLAGS) $^ -c $(STARTUP)

$(EXECUTABLE):$(SRC) $(STARTUP_OBJ)
	$(CC) $(CFLAGS) $^ -o $@

#Make clean
clean:
	rm -rf $(EXECUTABLE)
	rm -rf $(BIN_IMAGE)
#Make flash
flash:
	st-flash write $(BIN_IMAGE) 0x8000000

#======================================================================
.PHONY:all clean flash
