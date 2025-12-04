# Task-3 Documentation
# High Availability AWS Architecture – Terraform Deployment

## Architecture Summary
### 1. VPC and Subnets
- Custom VPC: `10.20.0.0/16`
- Two public subnets (AZ-a, AZ-b)
- Two private subnets (AZ-a, AZ-b)
- Ensures multi‑AZ high availability

### 2. Routing
- Public subnets → IGW for internet access
- Private subnets → NAT Gateway for secure outbound traffic
- Separate route tables for public and private tiers

### 3. Load Balancer
- Application Load Balancer (ALB) deployed in public subnets
- Listener on port 80
- Target Group with health checks (`/`, 200–399)

### 4. Auto Scaling Group (ASG)
- Instances launched in private subnets only
- Controlled by Launch Template (Amazon Linux 2 + Nginx)
- Minimum 1, maximum 2 instances
- Automatically registers instances with ALB Target Group

### 5. Security
- ALB SG: Allows HTTP (80) from anywhere
- App SG: Allows HTTP only from ALB SG
- Instances have no public IPs (protected in private subnets)

## Traffic Flow
1. User requests the ALB DNS name.
2. ALB receives request in public subnet.
3. ALB forwards request to Target Group.
4. Target Group routes to EC2 instances in private subnets.
5. Nginx on EC2 returns HTML response.
6. Response flows back through ALB to the user.
