DAY1 mini project: 1-mini-project

project description:
  # High-Availability Multi-Tier VPC Architecture

  ## Architecture Diagram
  ```mermaid
  graph TD
      subgraph Internet ["External Internet"]
          User((User Browser))
      end

      subgraph VPC ["VPC (10.0.0.0/16)"]
          direction TB

          subgraph Public_Subnet ["Public Subnet (10.0.0.0/24)"]
              Bastion[Public EC2 Instance<br/><b>Nginx Reverse Proxy</b>]
          end

          subgraph Private_Subnets ["Private Subnets (10.0.1.0/24 & 10.0.2.0/24)"]
              direction TB
              ILB[Internal Load Balancer]
              
              subgraph TG ["Target Group (Port 8000)"]
                  App1[Private EC2 Instance 1<br/><b>Node.js Server</b>]
                  App2[Private EC2 Instance 2<br/><b>Node.js Server</b>]
              end
              
              DB[(PostgreSQL DB)]
          end
      end

      %% Traffic Flow
      User -- "1. Request (Port 8000)" --> Bastion
      Bastion -- "2. Reverse Proxy" --> ILB
      ILB -- "3. Forwarding" --> TG
      TG --> App1
      TG --> App2
      App1 -.-> DB
      App2 -.-> DB

      %% Styling with Forced Black Text
      style Bastion fill:#f9f,stroke:#333,stroke-width:2px,color:#000
      style ILB fill:#bbf,stroke:#333,stroke-width:2px,color:#000
      style App1 fill:#bbf,stroke:#333,stroke-width:2px,color:#000
      style App2 fill:#bbf,stroke:#333,stroke-width:2px,color:#000
      style DB fill:#fdb,stroke:#333,stroke-width:2px,color:#000
      
      %% Label and Subgraph Styling
      style User color:#fff
      style Public_Subnet fill:#fff,stroke:#333,stroke-dasharray: 5 5,color:#000
      style Private_Subnets fill:#fff,stroke:#333,stroke-dasharray: 5 5,color:#000
      style TG fill:#444,color:#fff