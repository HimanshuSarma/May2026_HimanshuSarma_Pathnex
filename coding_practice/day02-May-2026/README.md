VPC peering:

The "Gotchas" (Critical Rules)
There are two big rules you need to know before you start building:

No Overlapping CIDRs: You cannot peer two VPCs if they have the same or overlapping IP ranges (e.g., both are 10.0.0.0/16). The "bridge" wouldn't know which island a specific IP belongs to.

No Transitive Peering: This is the most famous rule. If VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A cannot talk to VPC C through B. You would have to create a direct peer between A and C.