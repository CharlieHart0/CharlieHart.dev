+++
title = 'Creating a Gameboy Emulator - Ongoing'
date = 2024-03-03T16:23:00Z
publishDate = 2024-03-03T16:23:00Z
draft = false
tags = ['Ongoing','Gameboy','Misc','Emulation','C++']
ShowToc = true
+++

My first emulator project is to write a Gameboy emulator, in C++, and is a project with several stages, so this post will be updated over time as new sections are added.

At the time of writing, I have implemented the Gameboy's CPU, memory bus and instruction set.

## Github Repository

[GitHub - CharlieHart0/GameBoyEmu: A GameBoy emulator](https://github.com/CharlieHart0/GameBoyEmu)

(The project is ongoing, and cannot be used yet)

## CPU

The most important part of the CPU to be modelled is the set of 8 bit registers, of which there are 8, A,B,C,D,E,F,H and L.

The register F is used as the flag register, and the higher half of this register is used for the carry, half carry, subtraction and zero flag bits respectively, which are used to indicate the result of operations.

The register pairs AF, BC, DE and HL can also be used to store a single 16 bit value, instead of two 8 bit values, and the register pair HL is often used to store 16 bit memory addresses.

Also in the CPU model are the program counter and stack pointer, each of which are 16 bit memory addresses. The program counter points to the location of the next instruction bit to be excecuted, while the stack pointer points to the top of the stack - the stack in the Gameboy starts at the end of memory and works its way backwards as it grows.

### Instructions

In order to emulate the Gameboy's CPU, I had to implement the instruction set, which has about 500 different combinations of instruction and operands. A table of opcodes can be found [here](https://meganesu.github.io/generate-gb-opcodes/)

<img title="" src="https://imgur.com/oq7Mooa" alt="">

A table of the 8-bit opcodes. 

If the byte 0xCB is used as an instruction byte, then a second table of 255 opcodes is used, where the byte following 0xCB is used. These are referred to as the 16-bit prefixed opcodes, and are mostly bit operations, to make a total of around 500 instructions.

The instructions include the following, and many others:

- Addition and subtraction

- Bitwise AND, OR, XOR and comparison operations

- Moving values between registers

- Pushing and Popping the values of 16 bit register pairs to the stack

- Control flow operations such as Jumping based on the value of flags in the flag register

- Bit rotations, setting and resetting of bits, and bit swapping.





## Memory Bus

## Future Work
