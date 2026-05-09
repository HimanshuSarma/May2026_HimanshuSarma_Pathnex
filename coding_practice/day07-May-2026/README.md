## Networking flow
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#2b2b2b', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#f4f4f4'}}}%%
graph TD
    subgraph "Global AWS Fabric (The Hidden Network)"
        direction TB
        MappingService["<b>AWS Mapping Service</b><br/>The Control Plane 'Brain'"]
        PhysicalBackbone["Physical Fiber Underlay"]
    end

    subgraph "Physical Host Machine (AZ-A)"
        direction TB
        NitroCard["<b>AWS Nitro System</b><br/>Hardware Virtualization Controller"]
        
        subgraph "EC2 Guest OS (Your Server)"
            direction LR
            KernelRoute["<b>Kernel Routing Table</b><br/>10.0.0.0/16 -> 'Local'"]
            ENA["Virtual Network Driver (ENA)"]
            NVMe["Virtual Storage Driver (NVMe)"]
        end
    end

    subgraph "Storage & Services"
        EBS_Vol[("<b>EBS Volume</b><br/>Remote Block Storage")]
        EFS_Share{{"<b>EFS Endpoint</b><br/>Regional NFS Share"}}
    end

    %% Networking Trickery Flow
    KernelRoute -- "1. Kernel sees 'Local' route" --> ENA
    ENA -- "2. Sends to .1 Virtual Gateway" --> NitroCard
    NitroCard -- "3. Encapsulates for Fabric" --> MappingService
    MappingService -- "4. Tunnels through Fiber" --> RemoteTarget["Remote Instance/Service"]

    %% Storage Attachment Logic
    NVMe -- "Hardware-level Cmds" --> NitroCard
    NitroCard -- "Dedicated Storage Channel" --> EBS_Vol
    
    ENA -- "Network-level NFS Traffic" --> EFS_Share

    %% Styling for Dark/Light Mode Compatibility
    style MappingService fill:#e67e22,stroke:#333,stroke-width:2px,color:#fff
    style NitroCard fill:#d35400,stroke:#333,stroke-width:2px,color:#fff
    style KernelRoute fill:#34495e,stroke:#fff,color:#fff
    style PhysicalBackbone fill:#7f8c8d,color:#fff