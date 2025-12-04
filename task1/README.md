# TASK-1 Documentation

## Explanation of designing VPC's and Subset

* A single VPC was created to act as the main network boundary.

* The VPC spans two Availability Zones to ensure high availability.

* Four subnets were created inside the VPC:

* Two public subnets (one in each AZ)

* Two private subnets (one in each AZ)

* Public subnets are configured to:

* Auto-assign public IPs

* Route 0.0.0.0/0 traffic to the Internet Gateway (IGW)

* Private subnets are configured to:

* Remain isolated from direct internet traffic

* Route 0.0.0.0/0 traffic to a NAT Gateway for secure outbound access

* An Internet Gateway was attached to the VPC to enable public traffic.

* A NAT Gateway was placed in one public subnet to give private subnets controlled internet access.

* Separate route tables were created for public and private subnets to maintain clear routing boundaries.

* The design ensures:

  * Secure separation between public and private tiers

  * Fault tolerance by spreading resources across two AZs

* Future scalability for application servers, ALB, RDS, etc.

## CIDR ranges used and reason for using them

### VPC CIDR Range

* 10.0.0.0/16

  * Provides a large IP address space (65,536 IPs).

  * Allows easy scaling for future subnets, databases, or application tiers.

  * Commonly used size in production VPC architectures.

### Public Subnet CIDR Ranges

* 10.0.1.0/24 — Public Subnet A

* 10.0.2.0/24 — Public Subnet B

  * /24 provides around 256 usable IPs—ideal for ALB, NAT Gateway, Bastion Hosts, etc.

  * Simple, sequential numbering (1.x, 2.x) keeps the network structure easy to understand.

  * Split across two Availability Zones for high availability and redundancy.

  * Ensures sufficient IP space for scaling public-facing components.

### Private Subnet CIDR Ranges

* 10.0.101.0/24 — Private Subnet A

* 10.0.102.0/24 — Private Subnet B

  * /24 provides enough IPs for EC2 Auto Scaling Groups, RDS, ElastiCache, and internal services.

  * Higher numbering (101.x, 102.x) clearly distinguishes private subnets from public ones.

  * Prevents overlap and ensures clean routing.

  * Subnets are placed across two Availability Zones to support Multi-AZ deployments and fault tolerance.

