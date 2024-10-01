+++
title = 'Creating a Gameboy Emulator - Part 2 - Ongoing'
date = 2024-10-01T16:23:00Z
publishDate = 2024-10-01T16:23:00Z
draft = false
tags = ['Ongoing','Gameboy','Misc','Emulation','C++', 'Featured']
ShowToc = true
+++

This is part 2 of [an ongoing project](https://charliehart.dev/tags/emulation/) in [C++](https://charliehart.dev/tags/c++) to create an emulator of the Nintendo Gameboy. Since [the last post](https://charliehart.dev/misc/gameboy-emulator-en/), I have added [a GUI for debugging](https://charliehart.dev/misc/gameboy-emulator-part2-en/#sdl-and-imgui), including inspectors for [the CPU state](https://charliehart.dev/misc/gameboy-emulator-part2-en/#cpu-inspector), [the Gameboy's Memory](https://charliehart.dev/misc/gameboy-emulator-part2-en/#memory-inspector), [a visualization of Graphics Memory](https://charliehart.dev/misc/gameboy-emulator-part2-en/#graphics-inspector), and [a ROM loader](https://charliehart.dev/misc/gameboy-emulator-part2-en/#rom-loader).

## Github Repository

https://github.com/CharlieHart0/GameBoyEmu
(The project is ongoing, and is not yet fully usable)

## SDL and ImGui

[ImGui](https://github.com/ocornut/imgui), a C++ GUI library, was used in combination with [SDL2](https://www.libsdl.org/) and [OpenGL3](https://www.opengl.org/) to add a GUI to the program, through which different parts of the emulator could be debugged. Eventually, SDL will be used to manage user input, audio, and rendering the gameboy's screen to the user's display. 

<img title="" src="https://i.imgur.com/un18AY8.png" alt="">

## GUI windows

Each of the following sections has its own window in the application:

### CPU Inspector

The CPU Inspector displays to the user the current state of all of the CPU's registers, the program counter, the stack pointer, and the last called instruction. While in debug mode, the inspector also shows the number of instructions completed, and the average duration of instructions, in nanoseconds, which is used to ensure that the performance of the CPU can keep up with the clock speed of the Gameboy's CPU.

 In addition to this, it also lets the user apply a speed modifier to the CPU loop, which allows the user to see what the CPU is doing at an appropriate timescale, as the step by step state of the ~4MHz CPU would be impossible for the user to view in realtime. The user can also click "Tick CPU" to step the CPU through instructions one at a time.

<img title="" src="https://i.imgur.com/4hioobZ.png" alt="">

### Memory Inspector

The Memory Inspector, inspired by programs such as [HxD](https://mh-nexus.de/en/hxd/), displays the contents of the Gameboy's 63556 byte memory, showing 256 bytes at a time. Addresses are highlighted with different colours if they are significant, such as:

- The current location of the Program Counter
- The current location of the Stack Pointer
- Addresses within the stack
- Bookmarked addresses (Significant hardware registers, user defined bookmarks etc.)
- The results of Find/Replace actions

On the right hand side of the window, the value is shown in a variety of formats, including Hex, Decimal, Binary, ASCII, and as both an 8 bit, and prefixed Instruction byte. Buttons can be used to seek left and right around the neighbouring bytes, as well as to jump to the beginning or the end of the memory.

<img title="" src="https://i.imgur.com/aN2cWiD.png" alt="">

#### Bookmarked Addresses

Within the memory inspector, there are almost one hundred addresses bookmarked by default, for example the audio registers, LCD scrolling position and status registers, and many more. These are all values which are important to check during debugging, and would otherwise be difficult to locate. Through a menu, the user can jump to any of these bookmarked addresses, or add their own bookmarks. Bookmarked Addresses are highlighted in the memory inspector.

<img title="" src="https://i.imgur.com/IjqY2WQ.png" alt="">

#### Find/Replace

The Find/Replace menu can be used to locate values within a selected area of memory, or the entire memory. The results will be highlighted in the memory inspector with a yellow background, so the user can easily find them. If memory editing has been enabled, the user can also replace the found values with another value.

<img title="" src="https://i.imgur.com/mAFiRbJ.png" alt="">

### ROM Loader

The ROM loader is used to load a ROM file (this could be a homebrew game, or debug ROM for example) into the Gameboy's memory, as well as extracting and displaying the metadata found on the [ROM header](https://gbdev.io/pandocs/The_Cartridge_Header.html).

One of the most important pieces of information in the ROM header is the [cart type](https://gbdev.io/pandocs/The_Cartridge_Header.html#0147--cartridge-type), which defines the hardware found on a physical cartridge, including extra RAM, ROM banks, rumble packs and more. Many of these hardware features need to be implemented and used on top of the default hardware found in the Gameboy. [ROM banks](https://gbdev.io/pandocs/MBCs.html#mbcs), for example, are used on many games, and the Gameboy must be able to access these banks to load them into the memory during runtime. 

Note that the ROM is not currently loaded into the Gameboy's memory when using the ROM loader, as the boot ROM must finish first, which requires further development of the [PPU](https://charliehart.dev/misc/gameboy-emulator-part2-en/#ppu). Therefore, its only use at the moment is to display header information.

<img title="" src="https://i.imgur.com/IhShysn.png" alt="">

Example: This is what the header of [Pokemon Red](https://en.wikipedia.org/wiki/Pok%C3%A9mon_Red,_Blue,_and_Yellow) might look like.

### Graphics Inspector

The Graphics Inspector displays the contents of the VRAM (Tile Data) in real time. This consists of three blocks of 128 8x8 pixel tiles, which are used to construct the scenes in a Gameboy game. There are only four different colours to choose from in each tile, and [each 8x8 tile occupies 16 bytes in memory.](https://gbdev.io/pandocs/Tile_Data.html)

<img title="" src="https://i.imgur.com/JTWrQcM.png" alt=""> Block 0 of the VRAM. This image shows random data in the VRAM, as roms are not yet loadable.

## PPU

The PPU (Pixel Processing Unit) is responsible for rendering and displaying rows of pixels to the LCD display of the Gameboy, in a similar way to how an old CRT screen would work. Horizontal lines are drawn, with each line consisting of three modes, OAM scan, Drawing, and Horizontal blank. When the frame has been fully drawn, and the current scan line is more than or equal to 144, the VBlank period starts, where most of the changes to the VRAM by the CPU will occur.

<img title="" src="https://i.imgur.com/3n1505i.png" alt="">

[Credit: Rendering - Pan Docs](https://gbdev.io/pandocs/Rendering.html#ppu-modes)

Currently, the PPU follows the expected timing of a Gameboy PPU as seen above (1 dot = 238.45 nanoseconds), although nothing is actually done, and no display exists yet, only the LY register (current scan line) is incremented.

## Future Work

The next steps for this project include:

- Further develop the PPU to write the correct values to all relevant hardware registers
- Ensure mutual exclusion locks are applied correctly to the PPU thread when accessing shared resources - This is already implemented for the CPU and UI threads
- Add memory mapping support for ROMs with a memory mapper
- Run simple one-bank ROMs such as tetris successfully, though without a display.
