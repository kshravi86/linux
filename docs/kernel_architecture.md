``` mermaid
graph TD
    subgraph "🔧 USER SPACE APPLICATIONS"
        Tools["🛠️ **System Tools & Utilities**<br/>📋 Commands, Scripts, Apps"]
    end
    
    subgraph "⚡ LINUX KERNEL CORE"
        Core["💎 **KERNEL CORE**<br/>🧠 Process Management<br/>💾 Memory Management<br/>📞 IPC & Scheduling"]
        Arch["🏗️ **ARCHITECTURE LAYER**<br/>⚙️ Platform-Specific Code<br/>🔧 Hardware Abstraction"]
        
        subgraph "🚀 CORE SUBSYSTEMS"
            Drivers["🎯 **DEVICE DRIVERS**<br/>🖥️ Hardware Controllers<br/>🔌 Device Management"]
            Net["🌐 **NETWORKING STACK**<br/>📡 TCP/IP, Protocols<br/>🔗 Network Interfaces"]
            FS["📁 **FILE SYSTEMS**<br/>💿 VFS, ext4, NTFS<br/>📂 Storage Management"]
            Block["💽 **BLOCK LAYER**<br/>🗄️ Disk I/O<br/>📚 Block Device Mgmt"]
            Security["🛡️ **SECURITY FRAMEWORK**<br/>🔐 SELinux, AppArmor<br/>🔒 Access Control"]
        end
        
        subgraph "⭐ SUPPORTING SYSTEMS"
            Virt["☁️ **VIRTUALIZATION**<br/>🖥️ KVM, Containers<br/>🎭 Virtual Machines"]
            Sound["🔊 **AUDIO SUBSYSTEM**<br/>🎵 ALSA, PulseAudio<br/>🎧 Sound Drivers"]
            Crypto["🔐 **CRYPTOGRAPHY**<br/>🔑 Encryption APIs<br/>🛡️ Security Algorithms"]
            Firmware["⚙️ **FIRMWARE LAYER**<br/>🔧 Hardware Init<br/>💾 Device Firmware"]
            Lib["📚 **KERNEL LIBRARIES**<br/>🧰 Shared Functions<br/>🔧 Utility Code"]
        end
        
        Include["📋 **HEADER FILES**<br/>🔗 System Includes<br/>📝 API Definitions"]
        
        subgraph "🏗️ BUILD & METADATA"
            BuildSystem["🔨 **BUILD SYSTEM**<br/>⚡ Kbuild & Kconfig<br/>📜 Compilation Scripts"]
            Metadata["📋 **PROJECT METADATA**<br/>📄 Documentation<br/>🏷️ Licenses & Samples"]
        end
    end
    
    HW["🖥️ **PHYSICAL HARDWARE**<br/>💻 CPU, RAM, Storage<br/>🔌 Peripherals & Devices"]
    
    %% Core & Architecture Connections
    Core <==> Arch
    Arch <==> HW
    Core <==> Lib
    Include -.-> Core
    Include -.-> Drivers
    Include -.-> Net
    Include -.-> FS
    
    %% Core Subsystems Interactions
    Drivers <==> Core
    Drivers <==> Arch
    Drivers <==> HW
    Net <==> Core
    Net <==> Drivers
    Net <==> Crypto
    FS <==> Core
    FS <==> Block
    FS <==> Crypto
    Block <==> Drivers
    Security <==> Core
    Security <==> FS
    Security <==> Net
    
    %% Supporting Systems Interactions
    Virt <==> Core
    Virt <==> Arch
    Sound <==> Core
    Sound <==> Drivers
    Crypto <==> Core
    Crypto <==> Lib
    Firmware <==> Drivers
    Firmware <==> Core
    Lib <==> Core
    
    %% User Space and Build/Meta
    Tools <==> Core
    BuildSystem -.-> Core
    BuildSystem -.-> Drivers
    BuildSystem -.-> Net
    Metadata -.-> Core
    
    %% Enhanced Styling with Compatible Colors
    classDef core fill:#667eea,stroke:#2d3748,stroke-width:4px,color:#fff
    classDef arch fill:#f093fb,stroke:#2d3748,stroke-width:4px,color:#fff
    classDef core_subsystem fill:#4facfe,stroke:#2d3748,stroke-width:3px,color:#fff
    classDef supporting_subsystem fill:#43e97b,stroke:#2d3748,stroke-width:3px,color:#fff
    classDef buildmeta fill:#fa709a,stroke:#2d3748,stroke-width:2px,color:#fff
    classDef hardware fill:#ff6b6b,stroke:#2d3748,stroke-width:4px,color:#fff
    classDef userspace fill:#a8edea,stroke:#2d3748,stroke-width:3px,color:#2d3748
    classDef includes fill:#d299c2,stroke:#2d3748,stroke-width:2px,color:#2d3748
    
    class Core,Lib core
    class Arch arch
    class Drivers,Net,FS,Block,Security core_subsystem
    class Virt,Sound,Crypto,Firmware supporting_subsystem
    class BuildSystem,Metadata buildmeta
    class HW hardware
    class Tools userspace
    class Include includes
```
