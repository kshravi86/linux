# tools Directory

The `tools/` directory in the Linux kernel source tree contains a collection of user-space tools and utilities that are related to kernel development, testing, debugging, and performance analysis. These tools are compiled and run in user space but often interact closely with kernel features or are used to manage or analyze kernel behavior.

## Purpose

The primary purposes of the `tools/` directory are:

1.  **Kernel Performance Analysis**: Provide tools like `perf` for profiling, tracing, and analyzing kernel and application performance.
2.  **Kernel Testing**: Include selftests (`testing/selftests/`) that verify the correctness of various kernel subsystems and APIs.
3.  **Debugging and Tracing Aids**: Offer utilities to help debug kernel issues or trace kernel events.
4.  **Interacting with Kernel Subsystems**: Provide user-space counterparts or utilities for specific kernel features (e.g., BPF tools, KVM tools, power management tools).
5.  **Build and Development Utilities**: Some tools might assist in the kernel build process or provide specific development functionalities (though most core build scripts are in `scripts/`).

## Contents

The `tools/` directory is diverse and contains many subdirectories, each often corresponding to a specific tool or category of tools. Key components include:

*   **`perf/`**: The `perf` tool, a powerful and versatile command-line utility for performance analysis.
    *   It uses kernel performance counters, tracepoints, kprobes, and uprobes.
    *   Can be used for profiling CPU usage, analyzing cache misses, tracing system calls, monitoring kernel events, and much more.
    *   Contains a significant amount of code for parsing event data, symbol resolution, and presenting results.

*   **`testing/selftests/`**: A large collection of self-tests for various kernel subsystems.
    *   Each subdirectory typically focuses on a specific area (e.g., `bpf/`, `cgroup/`, `drivers/`, `efivarfs/`, `ftrace/`, `ipc/`, `kvm/`, `net/`, `powerpc/`, `timers/`, `vm/`).
    *   These tests are crucial for regression testing and ensuring kernel changes don't break existing functionality.
    *   They often use specific kernel APIs or features to verify their behavior.

*   **`bpf/`**: Tools related to BPF (Berkeley Packet Filter).
    *   `bpftool/`: A command-line utility for inspecting and managing BPF programs and maps.
    *   `runqslower/`: A BPF-based tool to trace slow scheduler runqueue operations.
    *   Other example BPF programs and utilities.

*   **`power/`**: Tools related to power management.
    *   `cpupower/`: A collection of user-space utilities to control CPU frequency scaling, power states (cpuidle), and related parameters. Replaces older tools like `cpufreq-utils`.
    *   `turbostat/` (under `power/x86/`): A tool for reporting processor topology, frequency, idle power-state statistics, temperature and power on Intel CPUs.
    *   `x86_energy_perf_policy/` (under `power/x86/`): A tool to set the energy vs. performance policy preference on Intel CPUs.
    *   `acpi/`: Tools like `acpidump` for dumping ACPI tables.

*   **`hv/`**: Tools for interacting with Microsoft Hyper-V virtualization, typically for Linux guests.
    *   `hv_kvp_daemon.c`: KVP (Key-Value Pair) daemon for exchanging information with the Hyper-V host.
    *   `hv_fcopy_daemon.c`: File copy daemon.
    *   `hv_vss_daemon.c`: Volume Shadow Copy Service daemon.

*   **`kvm/`**: Tools for Kernel-based Virtual Machine (KVM).
    *   Often contains utilities for testing KVM functionality or simple KVM host tools.
    *   `kvm_stat/`: A tool to display KVM statistics.

*   **`usb/`**: Tools related to USB.
    *   `usbip/`: Tools for USB/IP, allowing USB devices to be shared over a network.
    *   `ffs-test.c`: A test utility for the Function Filesystem (USB gadget).

*   **`spi/`**: SPI (Serial Peripheral Interface) testing utilities.
    *   `spidev_test.c`: A utility for testing `spidev` devices.

*   **`gpio/`**: GPIO (General Purpose Input/Output) utilities.
    *   `lsgpio.c`: Lists GPIO chips and lines.
    *   `gpio-event-mon.c`: Monitors GPIO line events.

*   **`iio/`**: IIO (Industrial I/O) utilities.
    *   `iio_event_monitor.c`: Monitors IIO events.
    *   `iio_generic_buffer.c`: A tool to capture data from IIO devices.

*   **`leds/`**: LED control utilities.
    *   `uledmon.c`: Monitors udev events for LEDs.

*   **`objtool/`**: A tool used during the kernel build process to analyze object files for things like stack frame correctness, ORC (Oops ReWind Callstack) generation for unwinding, and static call validation.

*   **`memory-model/`**: Contains tools (`lkmm.o`, `herd`) and litmus tests for verifying and exploring the Linux kernel memory consistency model.

*   **`cgroup/`**: Tools related to cgroups, e.g., for monitoring or testing.

*   **`vm/`**: Tools related to virtual memory, such as `page-types` for debugging page flags.

*   **`virtio/`**: Tools for testing VirtIO drivers and devices (e.g., `vringh_test`, `virtio_test`).

*   **`wmi/`**: Windows Management Instrumentation tools, e.g., `dell-smbios-example.c`.

*   **`tracing/`**: Contains `rtla/`, a real-time latency analysis tool.

*   **`arch/`**: Some architectures may have architecture-specific tools here.

## Use for Linux OS

The tools in the `tools/` directory serve several important functions for the Linux ecosystem:

*   **Kernel Development and Debugging**: `perf`, `objtool`, tracing helpers, and various debugging utilities are essential for kernel developers to understand behavior, find bugs, and optimize code.
*   **Kernel Validation**: The selftests are a cornerstone of kernel quality assurance, run by developers and automated testing systems to catch regressions.
*   **System Administration and Monitoring**: Tools like `cpupower` allow administrators to configure and monitor system behavior. `perf` can also be invaluable for sysadmins diagnosing performance issues.
*   **Hardware Enablement**: Utilities for specific hardware interfaces (SPI, GPIO, IIO) can be used for testing and interacting with new hardware.
*   **Understanding Kernel Internals**: Many of these tools provide insights into how specific kernel subsystems work, making them useful for learning and exploration.
*   **User-Space Interaction**: Some tools provide the user-space components needed to interact with specific kernel features (e.g., `bpftool` for BPF, `usbip` tools for USB/IP).

While not part of the running kernel image itself, the `tools/` directory provides a rich ecosystem of utilities that are deeply intertwined with the kernel's development, testing, and operational lifecycle. Many of these tools are considered standard components for anyone working seriously with the Linux kernel.
