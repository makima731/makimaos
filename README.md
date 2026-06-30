# MakimaOS (x64)

A custom, lightweight 64-bit operating system written from scratch.

Current Status: The project is in its early development stages. Currently, the kernel successfully validates hardware capabilities, initializes 64-bit Long Mode with paging, and boots seamlessly inside a simulated environment.

## Features (Current Stage)

* **64-bit Architecture (x86_64):** Fully transitions from 32-bit protected mode into 64-bit Long Mode.
* **Hardware Verification:** Runtime validation for Multiboot2 compliance, CPUID availability, and 64-bit processor support.
* **Virtual Memory Foundation:** Implemented 4-level paging structures using Identity Mapping with 2 MiB huge pages (mapping the first 1 GiB of physical memory).
* **Segmentation & GDT64:** Custom 64-bit Global Descriptor Table setup with proper code segment reloading via far jump.
* **Dockerized Build Toolchain:** Fully isolated cross-compiler environment to ensure reproducible builds regardless of the host OS.
* **Early Panic Subsystem:** Direct VGA text buffer writing (0xb8000) providing visual error codes ('M', 'C', 'L') during early boot failures.

## Prerequisites

The project utilizes a Docker container to isolate the toolchain. You only need Docker installed on your host system:

* **Container Engine:** Docker Desktop / Docker Engine
* **Emulator:** qemu-system-x86_64 (to test the generated bootable media)

The internal container automatically provisions `x86_64-elf-gcc`, `x86_64-elf-ld`, `nasm`, `xorriso`, and `grub-mkrescue`.

## Building and Running

1. **Clone the repository:**
   ```bash
   git clone https://github.com
   cd MakimaOS
   ```

2. **Build the Docker compiler environment:**
   ```bash
   docker build buildenv -t makimaos-buildenv
   ```

3. **Compile the Kernel using the container:**
   * On Linux / macOS (bash/zsh):
     ```bash
     docker run --rm -it -v "\$(pwd)":/root/env makimaos-buildenv make build-x86_64
     ```
   * On Windows (PowerShell):
     ```powershell
     docker run --rm -it -v \${PWD}:/root/env makimaos-buildenv make build-x86_64
     ```
4. **Build it**
   ```bash
   make x86_64
   ```

5. **Run in QEMU:**
   ```bash
   qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
   ```

## Roadmap

- [x] Boot into 32-bit Bootstrap Stage (Multiboot2 Compliant)
- [x] Hardware Safety Verification (CPUID & Long Mode check)
- [x] Early Page Table Setup & Paging Activation (2MiB Huge Pages)
- [x] Global Descriptor Table (GDT64) and Far Jump execution
- [x] Successful Transition to 64-bit Kernel Space (`long_mode_start`)
- [ ] Interrupt Descriptor Table (IDT) and Interrupt Handling
- [ ] Dynamic Memory Allocation (Kernel Heap)
- [ ] Advanced VGA/Framebuffer Video Driver (beyond raw tracking output)
- [ ] Keyboard Input Driver
- [ ] User Mode and Multitasking

## License

This project is licensed under the MIT License - see the LICENSE file for details.
