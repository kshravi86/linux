# virt Directory

The `virt/` directory in the Linux kernel source tree contains code related to virtualization technologies, primarily focusing on the Kernel-based Virtual Machine (KVM). KVM allows Linux to act as a Type 1 hypervisor, enabling it to create and run virtual machines (VMs) with near-native performance.

## Purpose

The primary purpose of the `virt/` directory is to:

1.  **Provide the core KVM infrastructure**: Implement the architecture-independent parts of KVM, which manage the lifecycle of VMs, virtual CPUs (vCPUs), and their interaction with the host kernel.
2.  **Interface with hardware virtualization extensions**: While the architecture-specific parts of KVM that directly use hardware virtualization extensions (like Intel VT-x or AMD-V) reside in `arch/<architecture>/kvm/`, this directory provides the generic framework they plug into.
3.  **Manage VM memory and I/O**: Handle memory virtualization for guests and facilitate emulated or paravirtualized I/O.
4.  **Support paravirtualization features**: Include code for paravirtualized devices or interfaces (like VirtIO, though VirtIO drivers themselves are in `drivers/virtio/`) that can improve VM performance.

## Contents

The `virt/` directory is relatively small compared to giants like `drivers/` or `net/`, but its contents are highly specialized and critical for virtualization. The main component is KVM:

*   **`kvm/`**: This is the primary subdirectory and contains the architecture-independent core of KVM.
    *   **`kvm_main.c`**: Core KVM logic, including:
        *   Creation and management of VM file descriptors.
        *   Creation and management of vCPU file descriptors.
        *   Handling KVM ioctls for VM and vCPU control (e.g., setting memory regions, running vCPUs, injecting interrupts).
        *   Management of KVM slots (memory regions mapped into the guest).
        *   Coalesced MMIO (Memory-Mapped I/O) handling.
        *   Dirty page logging for live migration.
    *   **`eventfd.c`**: Support for using eventfds to signal events to/from KVM guests (e.g., for I/O notification or interrupt injection).
    *   **`irqchip.c`**: Management of virtual interrupt controllers (though the specific emulation logic might be architecture-specific or in `drivers/irqchip/`).
    *   **`vfio.c`**: Integration with VFIO (Virtual Function I/O) for assigning physical devices directly to guests (PCI passthrough).
    *   **`async_pf.c`**: Asynchronous page fault handling for guests.
    *   **`coalesced_mmio.c`**: Implements coalesced MMIO, a technique to batch MMIO accesses from the guest to reduce VM exit overhead.
    *   **`binary_stats.c`**: Provides a binary interface for KVM statistics.
    *   **`pfncache.c`**: Guest page fault optimization by caching guest PFNs (Page Frame Numbers).
    *   **`guest_memfd.c`**: Support for memory backends using memfd_secret for guests.
    *   **`dirty_ring.c`**: Manages the dirty page ring buffer for tracking guest memory writes.

*   **`lib/`**: Contains library code that might be shared within virtualization components.
    *   **`irqbypass.c`**: A manager for handling interrupts in a way that can bypass the guest OS for performance, often used with assigned devices.

It's important to note the division of labor:

*   **`virt/kvm/`**: Architecture-independent KVM core.
*   **`arch/<architecture>/kvm/`**: Architecture-specific KVM code that directly interacts with hardware virtualization extensions (e.g., `arch/x86/kvm/vmx.c` for Intel VT-x, `arch/x86/kvm/svm.c` for AMD-V). This is where guest state is saved/restored, VM exits are handled, and CPU-specific virtualization features are implemented.
*   **`drivers/kvm/`**: While less common for core KVM logic, this might contain drivers for auxiliary KVM-related hardware or interfaces if they don't fit neatly into the arch-specific model.
*   **`drivers/virtio/`**: Drivers for VirtIO paravirtualized devices (e.g., `virtio_net`, `virtio_blk`, `virtio_pci`). These allow guests to use efficient, standardized interfaces for I/O rather than fully emulating hardware.
*   **`drivers/vfio/`**: VFIO framework for secure device assignment to user space, which KVM leverages for PCI passthrough.

## Use for Linux OS

The code in the `virt/` directory, primarily KVM, transforms Linux into a powerful hypervisor:

*   **Server Virtualization**: Enables running multiple isolated server instances (VMs) on a single physical machine, improving resource utilization and flexibility. This is a cornerstone of cloud computing.
*   **Desktop Virtualization**: Allows users to run different operating systems (e.g., Windows, other Linux distributions) as guests on their Linux desktop.
*   **Development and Testing**: Provides isolated environments for software development, testing, and debugging without affecting the host system.
*   **Security Sandboxing**: VMs can be used to run untrusted applications in a more strongly isolated environment than traditional process-based sandboxes.
*   **Legacy Application Support**: Allows running older operating systems or applications that are not compatible with the current host environment.
*   **Near-Native Performance**: By leveraging hardware virtualization extensions, KVM can run guests with very little performance overhead for most workloads.
*   **Integration with User-Space Tools**: KVM is typically managed by user-space tools like QEMU, libvirt, and cloud orchestration platforms (e.g., OpenStack, Kubernetes via Kubevirt), which use the KVM API (ioctl interface on `/dev/kvm`) to create and control VMs.

KVM, with its core in `virt/kvm/` and its architecture-specific counterparts, is a critical component for modern Linux systems, enabling a wide range of virtualization use cases. Its integration into the mainline kernel means it benefits from the kernel's stability, performance, and feature development.
