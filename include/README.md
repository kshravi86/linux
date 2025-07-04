# include Directory

The `include/` directory at the root of the Linux kernel source tree is a crucial part of the kernel's build system and internal structure. It contains header files that are used throughout the kernel.

## Purpose

The primary purposes of the `include/` directory are:

1.  **Provide Kernel-Internal APIs and Definitions**: It houses header files that define data structures, macros, function prototypes, and constants used by various kernel subsystems. These are generally not meant for direct use by user-space applications.
2.  **Expose User-Space APIs (UAPI)**: A specific subdirectory, `include/uapi/`, contains header files that define the Application Programming Interface (API) that the kernel presents to user-space programs. These are the headers that get installed on a system for developers to compile applications against (e.g., in `/usr/include/linux/`).
3.  **Facilitate Modularity and Compilation**: Header files allow different parts of the kernel to share definitions and interfaces, enabling the compilation of individual kernel modules and subsystems.
4.  **Architecture-Specific Definitions**: While this top-level `include/` directory contains generic headers, architecture-specific headers are found in `arch/<architecture>/include/`.

## Contents

The `include/` directory is organized into several subdirectories and also contains some top-level header files. Key components include:

*   **`linux/`**: This is the largest and most important subdirectory. It contains a vast number of header files related to core kernel subsystems and features. Examples:
    *   `fs.h`: Core VFS (Virtual File System) structures and definitions.
    *   `sched.h`: Scheduler and task (process/thread) related structures.
    *   `mm.h`: Memory management structures and functions.
    *   `netdevice.h`: Network device interface.
    *   `skbuff.h`: Socket buffer (network packet) manipulation.
    *   `module.h`: Kernel module infrastructure.
    *   `pci.h`: PCI bus interface.
    *   `usb.h`: USB interface.
    *   `types.h`: Basic kernel data types.
    *   `kernel.h`: General kernel utility functions and macros.
    *   `list.h`: Kernel's linked list implementation.
    *   Many, many more, covering almost every aspect of the kernel.

*   **`uapi/` (User API)**: This subdirectory contains headers that define the interface between the kernel and user-space applications.
    *   **`linux/` (within `uapi/`)**: Headers for standard Linux system calls and structures (e.g., `uapi/linux/ioctl.h`, `uapi/linux/input.h`, `uapi/linux/socket.h`). These are the files typically installed into system include directories.
    *   **`asm-generic/` (within `uapi/`)**: Generic UAPI headers that can be shared across architectures.
    *   Architecture-specific UAPI headers (e.g., `uapi/asm-x86/`, `uapi/asm-arm/`) are symbolic links or copies managed by the build system, pointing to the actual files in `arch/<architecture>/include/uapi/asm/`.

*   **`asm-generic/`**: Generic header files that provide default implementations or common definitions for functionalities that might otherwise be architecture-specific. Architectures can choose to use these or provide their own versions. Examples:
    *   `io.h`: Generic I/O port access.
    *   `mman.h`: Generic memory mapping definitions.
    *   `signal.h`: Generic signal definitions.

*   **`acpi/`**: Headers related to ACPI (Advanced Configuration and Power Interface).
*   **`clocksource/`**: Headers for clock source management.
*   **`crypto/`**: Headers for the kernel's cryptographic API.
*   **`drm/`**: Headers for the Direct Rendering Manager (graphics).
*   **`keys/`**: Kernel key management facility headers.
*   **`media/`**: Headers for media device support (V4L2, DVB, etc.).
*   **`net/`**: Extensive set of headers for networking protocols and subsystems.
*   **`rdma/`**: Headers for Remote Direct Memory Access.
*   **`scsi/`**: Headers for the SCSI subsystem.
*   **`sound/`**: Headers for the ALSA (Advanced Linux Sound Architecture) and other sound subsystems.
*   **`target/`**: Headers for SCSI target mode drivers (e.g., iSCSI target).
*   **`trace/`**: Headers for kernel tracing facilities (ftrace, tracepoints, perf events).
*   **`video/`**: Headers related to video framebuffers and display.
*   **`xen/`**: Headers for Xen virtualization support.

*   **Top-level headers**: Some important headers reside directly in `include/`, such as:
    *   `Kbuild`: Used by the kbuild system.
    *   `asm-generic-yguard.h`, `linux-yguard.h`, `uapi-yguard.h`: Header inclusion guards to prevent accidental direct inclusion of internal headers.

## Use for Linux OS

The `include/` directory is essential for both kernel development and user-space application development:

*   **Kernel Compilation**: Provides the necessary definitions and declarations for compiling the kernel itself and its modules.
*   **Kernel Development**: Serves as the primary reference for kernel-internal data structures, functions, and macros.
*   **User-Space Development**: The `include/uapi/` subdirectory defines the stable API that user-space programs use to interact with the kernel via system calls and ioctls. These headers are distributed with C libraries (like glibc) and development packages.
*   **Hardware Abstraction**: Together with `arch/<architecture>/include/`, it helps in abstracting hardware differences.
*   **Interface Definition**: Defines the contracts between different kernel subsystems.

It's a central repository of type definitions, constants, macros, and function prototypes that glue the entire kernel together and define its public-facing interface. The separation between internal kernel headers and UAPI headers is critical for maintaining a stable ABI (Application Binary Interface) for user-space while allowing the kernel internals to evolve.
