## Architecture Diagram

```mermaid
graph TD
    subgraph "AWS Region (us-east-1)"
        
        %% VPC A
        subgraph "VPC-A (10.0.0.0/16)"
            direction TB
            subgraph "Public Subnet A (10.0.1.0/24)"
                InstanceA_Pub[Public EC2 A]
            end
            subgraph "Private Subnet A1 (10.0.2.0/24)"
                InstanceA_Priv1[Private EC2 A1]
            end
            subgraph "Private Subnet A2 (10.0.3.0/24)"
                InstanceA_Priv2[Private EC2 A2]
            end
            
            IGW_A((IGW A))
            NAT_A[[NAT Gateway A]]
            
            InstanceA_Pub --> IGW_A
            InstanceA_Priv1 & InstanceA_Priv2 --> NAT_A
            
            %% Routing Rule Note for VPC A
            RouteA[<b>Routing Rule:</b><br/>10.1.0.0/16 → PCX]
        end

        %% Peering Connection
        PCX((VPC Peering Connection))

        %% VPC B
        subgraph "VPC-B (10.1.0.0/16)"
            direction TB
            subgraph "Public Subnet B (10.1.1.0/24)"
                InstanceB_Pub[Public EC2 B]
            end
            subgraph "Private Subnet B1 (10.1.2.0/24)"
                InstanceB_Priv1[Private EC2 B1]
            end
            subgraph "Private Subnet B2 (10.1.3.0/24)"
                InstanceB_Priv2[Private EC2 B2]
            end
            
            IGW_B((IGW B))
            NAT_B[[NAT Gateway B]]
            
            InstanceB_Pub --> IGW_B
            InstanceB_Priv1 & InstanceB_Priv2 --> NAT_B

            %% Routing Rule Note for VPC B
            RouteB[<b>Routing Rule:</b><br/>10.0.0.0/16 → PCX]
        end

        %% Inter-VPC Connectivity
        VPC-A <== PCX ==> VPC-B
    end