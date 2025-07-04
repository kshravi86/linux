# net Directory

The `net/` directory in the Linux kernel source tree is the home of the kernel's networking subsystem. It contains the implementations of various network protocols, network device management, socket layers, and other networking-related functionalities.

## Purpose

The primary purpose of the `net/` directory is to:

1.  **Implement Network Protocols**: Provide the core logic for a wide range of network protocols, including TCP/IP (IPv4 and IPv6), UDP, ICMP, ARP, Ethernet, Wi-Fi (in conjunction with `drivers/net/wireless/`), Bluetooth networking, and many others.
2.  **Manage Network Devices**: Handle the registration, configuration, and operation of network interface controllers (NICs) through the `netdevice` abstraction.
3.  **Provide Socket APIs**: Implement the socket layer, which is the primary interface for user-space applications to perform network communication (e.g., `socket()`, `bind()`, `connect()`, `send()`, `recv()`).
4.  **Handle Packet Processing**: Manage the flow of network packets through the system, including routing, forwarding, filtering (Netfilter/iptables/nftables), and queueing.
5.  **Support Network Address Translation (NAT) and Connection Tracking**: Implement mechanisms for modifying network addresses and tracking network connections.
6.  **Offer Network Utilities and Management Interfaces**: Provide tools and interfaces (e.g., via procfs, sysfs, netlink) for configuring and monitoring the networking subsystem.

## Contents

The `net/` directory is very large and complex, reflecting the richness of Linux's networking capabilities. It is organized into numerous subdirectories, typically based on protocol families or functional areas:

*   **`core/`**: Core networking infrastructure.
    *   `dev.c`: Network device management (registration, activation, deactivation).
    *   `skbuff.c`: Socket buffer (`sk_buff`) management, the fundamental data structure for network packets.
    *   `sock.c`: Generic socket layer implementation.
    *   `netfilter.c`: Core Netfilter hooks and infrastructure.
    *   `rtnetlink.c`: Netlink interface for routing and network device configuration.
    *   `sysctl_net_core.c`: Sysctl parameters for core networking.
    *   `gro.c`, `gso.c`: Generic Receive Offload and Generic Segmentation Offload.

*   **`ipv4/`**: Implementation of the IPv4 protocol suite.
    *   `af_inet.c`: Address Family INET (IPv4) socket layer.
    *   `ip_input.c`, `ip_output.c`, `ip_forward.c`: IPv4 packet reception, transmission, and forwarding.
    *   `route.c`: IPv4 routing table management.
    *   `tcp_ipv4.c`, `udp.c`, `ping.c` (for raw ICMP sockets): TCP, UDP, and ICMP implementations over IPv4.
    *   `arp.c`: Address Resolution Protocol.
    *   `netfilter/`: IPv4-specific Netfilter modules (iptables).

*   **`ipv6/`**: Implementation of the IPv6 protocol suite.
    *   `af_inet6.c`: Address Family INET6 (IPv6) socket layer.
    *   `ip6_input.c`, `ip6_output.c`, `ip6_forward.c`: IPv6 packet processing.
    *   `route.c`: IPv6 routing.
    *   `tcp_ipv6.c`, `udp.c` (shared with IPv4 but with IPv6 specifics): TCP and UDP over IPv6.
    *   `ndisc.c`: Neighbor Discovery Protocol.
    *   `netfilter/`: IPv6-specific Netfilter modules (ip6tables).

*   **`bridge/`**: Ethernet bridging implementation (software switch).
*   **`dsa/`**: Distributed Switch Architecture, for managing switch hardware.
*   **`ethernet/`**: Ethernet-specific functionalities (`eth.c`).
*   **`llc/`**: Logical Link Control (IEEE 802.2).
*   **`sched/`**: Network traffic scheduling (queueing disciplines, qdiscs).
    *   Contains implementations of various qdiscs like `sch_fq_codel.c`, `sch_htb.c`, `sch_ingress.c`.

*   **`netfilter/`**: Core Netfilter framework and common components.
    *   `nf_conntrack_core.c`: Connection tracking.
    *   `nf_nat_core.c`: Network Address Translation core.
    *   `x_tables.c`: The `xtables` framework used by iptables, ip6tables, arptables, ebtables.
    *   Subdirectories for specific Netfilter modules and helpers.

*   **`nftables/`**: The `nftables` packet filtering framework, which is replacing `iptables`.
*   **`xfrm/`**: IPsec (IP Security) framework for secure network communication.
    *   `xfrm_input.c`, `xfrm_output.c`, `xfrm_policy.c`, `xfrm_state.c`.

*   **`socket.c`**: Generic socket system call implementations (the top-level interface).
*   **`sysctl_net.c`**: Top-level sysctl entries for networking.

*   **Protocol-specific directories**:
    *   `ax25/`: AX.25 packet radio protocol.
    *   `bluetooth/`: Bluetooth networking protocols (e.g., L2CAP, SCO, RFCOMM, BNEP). (Works in conjunction with `drivers/bluetooth/`).
    *   `caif/`: Communication CPU Abstraction Interface Framework (used by some modems).
    *   `can/`: Controller Area Network (CAN bus) protocols.
    *   `dccp/`: Datagram Congestion Control Protocol.
    *   `ieee802154/`: IEEE 802.15.4 and 6LoWPAN.
    *   `lapb/`: Link Access Procedure, Balanced (X.25).
    *   `l2tp/`: Layer 2 Tunneling Protocol.
    *   `mac80211/`: Software MAC for IEEE 802.11 (Wi-Fi). This is a major component for Wi-Fi drivers.
    *   `mptcp/`: Multipath TCP.
    *   `ncsi/`: Network Controller Sideband Interface.
    *   `netlink/`: Netlink socket protocol implementation.
    *   `netrom/`: NET/ROM amateur radio protocol.
    *   `nfc/`: Near Field Communication protocols.
    *   `openvswitch/`: Open vSwitch datapath.
    *   `packet/`: Raw packet sockets (`AF_PACKET`).
    *   `phonet/`: Phonet protocol (used in some Nokia phones).
    *   `rose/`: X.25 PLP Rose.
    *   `rds/`: Reliable Datagram Sockets.
    *   `rxrpc/`: RxRPC protocol (AFS).
    *   `sctp/`: Stream Control Transmission Protocol.
    *   `smc/`: Shared Memory Communications (SMC) over RDMA.
    *   `sunrpc/`: Sun Remote Procedure Call (RPC), used by NFS.
    *   `tipc/`: Transparent Inter-Process Communication.
    *   `unix/`: UNIX domain sockets (`AF_UNIX`).
    *   `vmw_vsock/`: VMware VSockets.
    *   `wireless/`: Wireless extensions (legacy Wi-Fi configuration, cfg80211 is the modern way).
    *   `xdp/`: XDP (eXpress Data Path) for high-performance packet processing.

## Use for Linux OS

The `net/` directory is fundamental to all network communication in Linux:

*   **Internet and Local Networking**: Enables communication over Ethernet, Wi-Fi, and other network media using TCP/IP and related protocols.
*   **Application Communication**: Provides the socket API that most network-aware applications use to send and receive data.
*   **Network Services**: Supports the implementation of network services like web servers, file servers (NFS, Samba), DNS resolvers, etc.
*   **Security**: Implements firewalls (Netfilter/nftables) and secure communication protocols (IPsec).
*   **Virtualization and Containers**: Provides networking for virtual machines and containers, including virtual switches and network namespaces.
*   **Specialized Networking**: Supports a wide array of less common or specialized network protocols used in various domains (e.g., amateur radio, industrial control, automotive).
*   **Network Management**: Offers interfaces for administrators to configure, monitor, and troubleshoot network behavior.

The networking subsystem is a cornerstone of modern computing, and the `net/` directory contains the vast and intricate machinery that makes Linux a powerful networking platform. Its modular design allows for continuous addition and improvement of protocols and features.
