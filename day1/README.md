DAY1 mini project: 1-mini-project

project description:
  graph TD
    subgraph Internet
        User[User Browser]
    end

    subgraph VPC ["VPC (10.0.0.0/16)"]
        
        subgraph Public_Subnet ["Public Subnet (10.0.0.0/24)"]
            Bastion[Public EC2 Instance <br/> <b>Nginx Reverse Proxy</b>]
        end

        subgraph Private_Subnets ["Private Subnets (10.0.1.0/24 & 10.0.2.0/24)"]
            direction TB
            ILB[Internal Load Balancer]
            
            subgraph TG ["Target Group (Port 8000)"]
                App1[Private EC2 Instance 1 <br/> <b>Node.js Server</b>]
                App2[Private EC2 Instance 2 <br/> <b>Node.js Server</b>]
            end
            
            DB[(PostgreSQL DB)]
        end
    end

    %% Traffic Flow
    User -- "1. Request to http://[Public-IP]:8000" --> Bastion
    Bastion -- "2. Forward to Internal LB DNS" --> ILB
    ILB -- "3. Load Balance" --> TG
    TG --> App1
    TG --> App2
    App1 -- "4. Internal Query" --> DB
    App2 -- "4. Internal Query" --> DB

    %% Security Labels
    classDef public fill:#f9f,stroke:#333,stroke-width:2px;
    classDef private fill:#bbf,stroke:#333,stroke-width:2px;
    classDef db fill:#fdb,stroke:#333,stroke-width:2px;
    class Bastion public;
    class App1,App2,ILB private;
    class DB db;