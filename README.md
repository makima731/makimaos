# MakimaOS (x64)

A custom, lightweight 64-bit operating system written from scratch.

Current Status: The project is in its early development stages. Currently, only the core Kernel is fully functional and ready.

## Features (Current Stage)

* 64-bit Architecture (x86_64): Runs fully in long mode.
* Core Kernel: Basic initialization, memory mapping foundation, and low-level hardware control.
* Bootloader Compatible: Designed to boot using standard modern bootloaders (e.g., Multiboot2 / Limine).

## Prerequisites

To build and run MakimaOS, you will need the following tools installed on your development machine (Arch Linux/Ubuntu):

* Compiler: gcc or clang configured for x86_64-elf cross-compilation.
* Assembler: nasm
* Build System: make or cmake
* Emulator: qemu-system-x86_64 (for testing)
* ISO Tools: xorriso or grub-mkrescue (to generate bootable media)

## Building and Running

1. Clone the repository:
   ```bash
   git clone https://github.com
   cd MakimaOS
   ```

2. Compile the Kernel:
   ```bash
   make build-x86_64
   ```

4. Run in QEMU:
   ```bash
   qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
   ```

## Roadmap

- [x] Boot into 64-bit Long Mode
- [x] Core Kernel Initialization
- [ ] Global Descriptor Table (GDT) and Interrupt Handling (IDT)
- [ ] Physical and Virtual Memory Management (Paging)
- [ ] Basic VGA/Framebuffer Video Driver
- [ ] Keyboard Input Driver
- [ ] User Mode and Multitasking

## License

This project is licensed under the MIT License - see the LICENSE file for details.
