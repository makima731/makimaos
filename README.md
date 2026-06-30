# MakimaOS (x64)

A custom, lightweight 64-bit operating system written from scratch.

Current Status: The project has successfully transitioned into high-level C execution. The kernel boots into 64-bit Long Mode and initializes a dedicated VGA terminal console subsystem with dynamic text coloring and automated screen scrolling.

## Features (v1.2.0)

* **64-bit Architecture (x86_64):** Fully transitions from 32-bit protected mode into 64-bit Long Mode space.
* **Hardware Verification:** Safe runtime validation for Multiboot2 compliance, CPUID availability, and 64-bit processor topology.
* **Stable Page Table Subsystem:** Uses Bitwise shifts (`shl`) for strict 64-bit huge page mapping (2 MiB pages) without breaking architecture registers.
* **High-Level C Runtime:** Completely freestanding execution environment fully detached from standard hosted OS libraries.
* **VGA Terminal Console Driver:** Modular abstraction tier (`print.h`/`print.c`) to control the video buffer matrix (`0xb8000`).
* **Automated Screen Scrolling:** Dynamic scrolling engine that automatically shifts characters up and clears the lower text boundary layer upon edge overflows.
* **Reproducible Docker Environment:** Completely isolated build pipeline utilizing custom `x86_64-elf` cross-compilers.

## Project Structure

* `src/intf/print.h` — VGA color palettes, system definitions, and printing entry points.
* `src/impl/kernel/main.c` — Main kernel entrance executing high-level C logic.
* `src/impl/kernel/print.c` — Main screen driver processing text output and row clears.
* `src/impl/x86_64/boot/` — Bootstrap files handling early initialization, GDT64 loading, and page tables setup.

## Prerequisites

You only need Docker and QEMU installed on your host system:

* **Container Engine:** Docker Desktop / Docker Engine
* **Emulator:** qemu-system-x86_64

The internal container toolchain automatically provisions `x86_64-elf-gcc`, `x86_64-elf-ld`, `nasm`, and `grub-mkrescue`.

## Building and Running

1. **Build the Docker compiler environment:**
   ```bash
   docker build buildenv -t makimaos-buildenv
   ```

2. **Compile the Kernel and generate the ISO image:**
   * On Linux / macOS (bash/zsh):
     ```bash
     docker run --rm -it -v "\$(pwd)":/root/env makimaos-buildenv make build-x86_64
     ```
   * On Windows (PowerShell):
     ```powershell
     docker run --rm -it -v \${PWD}:/root/env makimaos-buildenv make build-x86_64
     ```

3. **Clean the build cache (Optional):**
   ```bash
   docker run --rm -it -v "\$(pwd)":/root/env makimaos-buildenv make clean
   ```
4. **Build:**
   ```bash
   make build-x86_64
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
- [x] Freestanding High-Level C execution environment
- [x] Advanced VGA Console Driver with automated screen scrolling
- [ ] Interrupt Descriptor Table (IDT) and Hardware Interrupts handling
- [ ] Interactive Keyboard Input Driver (handling keystrokes)
- [ ] Dynamic Memory Allocation (Kernel Heap)
- [ ] User Mode and Multitasking

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.
