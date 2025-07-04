# security Directory

The `security/` directory in the Linux kernel source tree is dedicated to security-related code, primarily focusing on the Linux Security Modules (LSM) framework and various security subsystems that enhance the kernel's security capabilities.

## Purpose

The primary purposes of the `security/` directory are:

1.  **Implement Mandatory Access Control (MAC)**: Provide the infrastructure and specific implementations for MAC systems. MAC systems allow for defining fine-grained access control policies that go beyond the standard Discretionary Access Control (DAC) provided by user/group permissions.
2.  **Enhance Kernel Security**: Introduce various mechanisms to harden the kernel and protect against different types of attacks or vulnerabilities.
3.  **Provide Security Auditing**: Integrate with auditing frameworks to log security-relevant events.
4.  **Manage Security-Sensitive Kernel Objects**: Handle security aspects of kernel objects like keys and credentials.

## Contents

The `security/` directory is structured around the LSM framework and specific security modules/subsystems:

*   **`security.c`**: The core of the LSM framework. It defines the hooks that are placed throughout the kernel code at points where security decisions need to be made (e.g., before accessing a file, sending a signal, or performing a network operation). When such a hook is triggered, the LSM framework calls out to the registered security module(s) to get a decision.
*   **`lsm_audit.c`**: Provides helper functions for LSM modules to log audit messages.
*   **`commoncap.c`**: Generic capability checking logic used by some LSMs.
*   **`inode.c`**: Security-related aspects of inode management, often used by LSMs to store security labels on files.

*   **LSM Module Subdirectories**: Each subdirectory typically represents a specific security module that can be enabled and stacked (some can be primary, others secondary).
    *   **`selinux/`**: SELinux (Security-Enhanced Linux), a comprehensive MAC system developed by the NSA. It uses security labels and a detailed policy to enforce access controls.
    *   **`smack/`**: Smack (Simplified Mandatory Access Control Kernel), another label-based MAC system designed for simplicity.
    *   **`apparmor/`**: AppArmor, a path-name based MAC system that uses profiles to restrict program capabilities.
    *   **`tomoyo/`**: TOMOYO Linux, a path-name based MAC system focused on learning mode and behavior analysis.
    *   **`loadpin/`**: An LSM that ensures all kernel modules and firmware are loaded from a trusted, unwriteable source (e.g., a read-only rootfs or specific partition), preventing unauthorized code loading.
    *   **`yama/`**: An LSM that provides additional restrictions, primarily on `ptrace` (process tracing) scope to limit information leakage or unauthorized process manipulation.
    *   **`safesetid/`**: An LSM that restricts UID/GID transitions (setuid/setgid) to mitigate risks associated with privilege escalation.
    *   **`bpf/`**: Contains LSM hooks specifically for BPF (Berkeley Packet Filter) operations, allowing security modules to control BPF program loading and map access.
    *   **`landlock/`**: A sandboxing LSM that allows unprivileged processes to restrict their own ambient capabilities regarding file system access, network access, etc.

*   **`integrity/`**: Kernel integrity subsystem.
    *   **`ima/`**: Integrity Measurement Architecture (IMA). IMA can measure (hash) files before they are executed or mmaped and can appraise (verify signatures of) files against a trusted keyring.
    *   **`evm/`**: Extended Verification Module (EVM). EVM protects file metadata and security extended attributes (xattrs) by signing them, preventing offline tampering.
    *   `digsig.c`: Digital signature verification.
    *   `platform_certs/`: Code for loading platform-provided certificates (e.g., from UEFI).

*   **`keys/`**: Kernel key retention service.
    *   `key.c`, `keyring.c`: Core key and keyring management.
    *   `request_key.c`: Mechanism for user space to request keys from the kernel or for the kernel to upcall to user space to get a key.
    *   `encrypted-keys/`: Support for encrypting/decrypting kernel keys.
    *   `trusted-keys/`: Support for keys that are protected by hardware (like a TPM) or a trusted subsystem.

*   **`lockdown/`**: Kernel lockdown mechanism. When enabled, it restricts certain kernel features that could be used to compromise the running kernel, even by UID 0. This is particularly relevant for UEFI Secure Boot environments.

*   **`min_addr.c`**: Implements restrictions on the minimum virtual memory address accessible to user-space, helping to mitigate null-pointer dereference exploits.

*   **`device_cgroup.c`**: While cgroups are primarily in `kernel/cgroup/`, this file implements the device controller for cgroups, which allows restricting access to device nodes.

*   **`Kconfig`**, **`Makefile`**: Build system files for configuring and compiling security features and LSMs.

## Use for Linux OS

The `security/` directory provides mechanisms that significantly enhance the security posture of a Linux system:

*   **Mandatory Access Control**: LSMs like SELinux, AppArmor, and Smack allow administrators to define and enforce security policies that are more granular and robust than standard UNIX permissions. This helps in containing breaches and limiting the capabilities of processes.
*   **Integrity Protection**: IMA and EVM help ensure the integrity of the running system by detecting unauthorized modifications to files and metadata.
*   **Secure Key Management**: The kernel key service provides a secure way to store and manage sensitive information like encryption keys and authentication tokens.
*   **Hardening**: Features like kernel lockdown, `ptrace` restrictions (Yama), and `setuid` restrictions (SafeSetID) reduce the attack surface of the kernel.
*   **Sandboxing**: Landlock provides a way for applications to voluntarily restrict their own privileges, improving security through principle of least privilege.
*   **Auditing**: Integration with the audit framework ensures that security-relevant actions can be logged and reviewed.
*   **Trusted Computing**: Support for TPMs and trusted keys enables features that rely on a hardware root of trust.

The LSM framework is a key design feature, allowing different security models to be plugged into the kernel without modifying the core kernel code. This modularity means that users can choose the security module that best fits their needs, or even stack multiple modules for layered security (though typically only one "major" MAC module is active at a time). The components in this directory are crucial for building secure Linux systems, especially in environments with stringent security requirements.
