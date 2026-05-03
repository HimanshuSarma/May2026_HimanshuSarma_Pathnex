graph TD
    subgraph "AWS Region (us-east-1)"
        subgraph "VPC-A (10.0.0.0/16)"
            direction TB
            PubSubA[Public Subnet A] --> IGWA[Internet Gateway A]
            PrivSubA[Private Subnets A] --> NATA[NAT Gateway A]
            InstanceA[VPC-A-Instance] --- PubSubA
            SGA[Security Group: public_instance_sg] -.-> InstanceA
        end

        subgraph "VPC-B (10.1.0.0/16)"
            direction TB
            PubSubB[Public Subnet B] --> IGWB[Internet Gateway B]
            PrivSubB[Private Subnets B] --> NATB[NAT Gateway B]
            InstanceB[VPC-B-Instance] --- PubSubB
            SGB[Security Group: public_instance_sg] -.-> InstanceB
        end

        %% Peering Connection
        VPC_Peering{{"VPC Peering Connection<br>(vpc_a_to_b)"}}
        VPC-A (10.0.0.0/16) <==> VPC_Peering
        VPC_Peering <==> VPC-B (10.1.0.0/16)

        %% Routing Logic
        RouteA[Route: 10.1.0.0/16 via PCX] --- VPC-A (10.0.0.0/16)
        RouteB[Route: 10.0.0.0/16 via PCX] --- VPC-B (10.1.0.0/16)
    end