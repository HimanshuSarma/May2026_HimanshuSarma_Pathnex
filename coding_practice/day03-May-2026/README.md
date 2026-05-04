## Architecture Diagram

```mermaid
graph TD
    subgraph Internet[" "]
        IGW["<b>🌐 INTERNET GATEWAY</b>"]
    end

    subgraph AWS_Cloud["<b>☁️ AWS REGION (US-EAST-1)</b>"]
        
        TGW["<b>🛠️ TRANSIT GATEWAY</b><br/>(Hub - transit_gateway_vpc_a_b_c)"]

        subgraph VPC_A["<b>📦 VPC A (10.0.0.0/16)</b>"]
            direction TB
            EC2_A["<b>🖥️ EC2 INSTANCE A</b><br/>(Public Subnet)"]
            SG_A["<b>🛡️ SECURITY GROUP</b><br/>(22, 80, 443, 8000)"]
        end

        subgraph VPC_B["<b>📦 VPC B (10.1.0.0/16)</b>"]
            direction TB
            EC2_B["<b>🖥️ EC2 INSTANCE B</b><br/>(Public Subnet)"]
            SG_B["<b>🛡️ SECURITY GROUP</b><br/>(22, 80, 443, 8000)"]
        end

        subgraph VPC_C["<b>📦 VPC C (10.2.0.0/16)</b>"]
            direction TB
            EC2_C["<b>🖥️ EC2 INSTANCE C</b><br/>(Public Subnet)"]
            SG_C["<b>🛡️ SECURITY GROUP</b><br/>(22, 80, 443, 8000)"]
        end

        %% Attachments
        VPC_A ===|<b>VPC ATTACHMENT</b>| TGW
        VPC_B ===|<b>VPC ATTACHMENT</b>| TGW
        VPC_C ===|<b>VPC ATTACHMENT</b>| TGW

        %% Supernet Routing
        TGW -.->|<b>ROUTE: 10.0.0.0/8</b>| VPC_A
        TGW -.->|<b>ROUTE: 10.0.0.0/8</b>| VPC_B
        TGW -.->|<b>ROUTE: 10.0.0.0/8</b>| VPC_C
        
    end

    %% External Connectivity
    EC2_A --- IGW
    EC2_B --- IGW
    EC2_C --- IGW

    %% HIGH CONTRAST STYLING
    style TGW fill:#FF9900,stroke:#000,stroke-width:4px,color:#000
    style IGW fill:#FFFFFF,stroke:#000,stroke-width:3px,color:#000
    style AWS_Cloud fill:#F2F2F2,stroke:#000,stroke-width:2px,color:#000
    style Internet fill:#FFF,stroke:#000,stroke-dasharray: 5 5
    
    style VPC_A fill:#FFFFFF,stroke:#FF0000,stroke-width:3px,color:#000
    style VPC_B fill:#FFFFFF,stroke:#00FF00,stroke-width:3px,color:#000
    style VPC_C fill:#FFFFFF,stroke:#0000FF,stroke-width:3px,color:#000
    
    style EC2_A fill:#EEE,stroke:#333,color:#000
    style EC2_B fill:#EEE,stroke:#333,color:#000
    style EC2_C fill:#EEE,stroke:#333,color:#000