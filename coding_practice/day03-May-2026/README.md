## Architecture Diagram

```mermaid
graph TD
    subgraph Internet["Public Internet"]
        IGW["Internet Gateway"]
    end

    subgraph AWS_Cloud["AWS Region (us-east-1)"]
        
        TGW["Transit Gateway <br/>(transit_gateway_vpc_a_b_c)"]

        subgraph VPC_A["VPC A (10.0.0.0/16)"]
            Pub_A["Public Subnet A"] --> EC2_A["EC2 Instance A"]
            Priv_A["Private Subnet A"]
            SG_A["Security Group A"]
        end

        subgraph VPC_B["VPC B (10.1.0.0/16)"]
            Pub_B["Public Subnet B"] --> EC2_B["EC2 Instance B"]
            Priv_B["Private Subnet B"]
            SG_B["Security Group B"]
        end

        subgraph VPC_C["VPC C (10.2.0.0/16)"]
            Pub_C["Public Subnet C"] --> EC2_C["EC2 Instance C"]
            Priv_C["Private Subnet C"]
            SG_C["Security Group C"]
        end

        %% Connections to TGW
        VPC_A -- "VPC Attachment" --- TGW
        VPC_B -- "VPC Attachment" --- TGW
        VPC_C -- "VPC Attachment" --- TGW

        %% Routing Logic
        TGW -. "Route: 10.0.0.0/8" .-> VPC_A
        TGW -. "Route: 10.0.0.0/8" .-> VPC_B
        TGW -. "Route: 10.0.0.0/8" .-> VPC_C
        
    end

    %% External Access
    Pub_A --- IGW
    Pub_B --- IGW
    Pub_C --- IGW

    %% Styles
    style TGW fill:#f96,stroke:#333,stroke-width:2px
    style Internet fill:#fff,stroke:#333,stroke-dasharray: 5 5
    style VPC_A fill:#e1f5fe,stroke:#01579b
    style VPC_B fill:#e1f5fe,stroke:#01579b
    style VPC_C fill:#e1f5fe,stroke:#01579b