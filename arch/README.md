# arch Directory

This directory contains architecture-specific code for the Linux kernel. Each subdirectory within `arch/` is dedicated to a particular processor architecture (e.g., `x86`, `arm64`, `riscv`).

## Purpose

The primary purpose of the `arch/` directory is to abstract and manage the hardware differences between various CPU architectures, allowing the majority of the kernel code to remain architecture-independent. It provides the necessary interface between the generic kernel and the specific hardware it's running on.

## Contents

Within each architecture-specific subdirectory (e.g., `arch/x86/`), you will typically find:

*   **`boot/`**: Code related to the boot process for that architecture, including bootloaders, decompression routines, and setup code.
*   **`configs/`**: Default kernel configuration files (`defconfig`) for different platforms or variants of the architecture.
*   **`include/asm/`**: Architecture-specific header files that define low-level details such as memory layout, register access, system call conventions, and interrupt handling.
*   **`include/uapi/asm/`**: Header files that define the userspace API for this architecture.
*   **`kernel/`**: Core architecture-specific kernel code. This includes:
    *   Interrupt handling routines.
    *   Signal handling.
    *   Process management specifics (context switching, thread creation).
    *   System call implementation details.
    *   Timekeeping and timer implementations.
    *   CPU-specific features and errata workarounds.
    *   Power management and CPU idling.
*   **`kvm/`**: Kernel-based Virtual Machine (KVM) support specific to the architecture, enabling hardware-assisted virtualization.
*   **`lib/`**: Architecture-specific library functions, often optimized assembly routines for common operations like memory copying, checksums, etc.
*   **`mm/`**: Memory management code specific to the architecture, including page table management, TLB (Translation Lookaside Buffer) handling, and memory initialization.
*   **`pci/`**: PCI (Peripheral Component Interconnect) bus support tailored for the architecture.
*   **Platform-specific directories**: Many architectures will have subdirectories for specific SoCs (System on a Chip), platforms, or machine types (e.g., `arch/arm/mach-omap2/` for TI OMAP2 processors). These contain code to support the unique hardware features of those platforms.

## Use for Linux OS

The code within the `arch/` directory is fundamental to making Linux portable across a vast range of hardware. It handles the low-level details that differ from one CPU to another, such as:

*   **Bootstrapping the kernel**: Initializing the CPU, memory, and essential hardware.
*   **Memory Management**: Setting up and managing page tables, handling memory addressing specifics.
*   **Interrupts and Exceptions**: Configuring interrupt controllers and handling hardware exceptions and software interrupts.
*   **System Calls**: Providing the bridge between user-space applications and kernel-space services.
*   **Performance Optimizations**: Implementing architecture-specific optimized routines for critical kernel functions.
*   **Hardware Abstraction**: Exposing a consistent interface to the rest of the kernel, hiding the complexities of the underlying hardware.

Without the `arch/` directory, a separate version of the Linux kernel would be needed for each CPU architecture. This directory is a cornerstone of Linux's portability and wide hardware support.
