# FreeRTOSGrind
Relearning FreeRTOS Through 9 Short Projects

## Board

STM32 NUCLEO-H7A3ZI-Q w/ On Board STLINK-V3E

## Build PC

OS: Arch Linux x86_64

Kernel: 6.16.8-arch1-1

DE: Cinnamon (CubeMX won't work in Sway)

IDE: Neovim / STM32CubeMX

Misc Packages: arm-non-eabi-gcc (14.2), openocd, stlink

## Build Instructions

### STM32CubeMX

Configure the project output to Makefile

`Project Manager->Project->Toolchain/IDE`

Copy all used libraries into the project folder

`Project Manager->Code Generator->STM32Cube MCU packages and embedded software packs`

(Optional)
- Generate peripheral initialization as a pair of '.c/.h' files per peripheral
- Back up previously generated files when re-generating
- Keep User Code when re-generating

After adding the FreeRTOS Software Pack, switch the SYS Timebase Source to a
timer

Generate the Code

### Compiling and LSP

Normal GNU make commands

`bear -- make` to compile compile_commands.json

### Flashing

Can use `st-info --probe` to verify the board:
```bash
st-info --probe
Found 1 stlink programmers
  version:    V3J13
  serial:     002000483038510534333935
  flash:      2097152 (pagesize: 8192)
  sram:       131072
  chipid:     0x480
  dev-type:   STM32H7Ax_H7Bx
```

Create a file called openocd.cfg in the project root:
```
# Programmer, can be changed to several interfaces
source [find interface/stlink.cfg]

# The target MCU. This should match your board
source [find target/stm32h7x.cfg]
```

Flash using this command:
```bash
openocd -f ./openocd.cfg -c "program build/<projectname>.elf verify reset exit"
```
