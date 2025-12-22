# AWSAssignments

# AWS Assignment 1 – Custom VPC with Public and Private Subnets

## 📌 Objective
The objective of this assignment is to design and deploy a custom Amazon VPC with proper network segmentation, internet access, and EC2 instances deployed across public and private subnets following AWS best practices.

---

## 🏗 Architecture Overview

The architecture consists of:

- A custom VPC (`10.0.0.0/16`)
- One public subnet
- One private subnet
- An Internet Gateway (IGW)
- A NAT Gateway for outbound internet access from the private subnet
- Separate route tables for public and private subnets
- EC2 instances deployed in each subnet
- Security groups enforcing least-privilege access

---

## 🛠 Resources Created

### Networking
- **VPC:** Custom VPC (10.0.0.0/16)
- **Public Subnet:** Internet-facing subnet
- **Private Subnet:** No direct internet access
- **Internet Gateway:** Attached to VPC
- **Elastic IP:** Allocated for NAT Gateway
- **NAT Gateway:** Deployed in public subnet

### Routing
- **Public Route Table**
  - Route: `0.0.0.0/0 → Internet Gateway`
- **Private Route Table**
  - Route: `0.0.0.0/0 → NAT Gateway`

### Compute
- **Public EC2 Instance**
  - Deployed in public subnet
  - Assigned a public IPv4 address
  - Used for external access and administration
- **Private EC2 Instance**
  - Deployed in private subnet
  - No public IP
  - Internet access only via NAT Gateway

---

## 🔐 Security Groups

### Public EC2 Security Group
- Allows:
  - SSH (port 22) from my public IP
  - HTTP (port 80) from my public IP
- Denies all other inbound traffic

### Private EC2 Security Group
- Allows:
  - Internal traffic from within the VPC / public EC2
- No inbound access from the internet

---

## 🧪 Validation & Testing

- Verified public EC2 has internet access via IGW
- Verified private EC2 can access the internet via NAT Gateway
- Confirmed private EC2 is not reachable directly from the internet
- Confirmed routing tables are correctly associated

---

## 📸 Screenshots

Screenshots are included to demonstrate:
- VPC and subnet configuration
- Route tables
- Internet Gateway and NAT Gateway
- EC2 instance details
- Security group rules

(All screenshots are located in the `/screenshots` folder)

---

## 🧹 Cost Management

All billable resources (EC2 instances, NAT Gateway, Elastic IP) were terminated and deleted after completion of the assignment to avoid ongoing charges.

---

## ✅ Conclusion

This assignment demonstrates a solid understanding of AWS networking fundamentals, including VPC design, subnet isolation, routing, internet access patterns, and security group configuration following best practices.

------------------------------------------------------------------------------------------------------------//////////////////////


# Assignment 2 – Application Load Balancer (ALB)

## Overview

This assignment demonstrates a common DevOps architecture pattern: deploying multiple EC2 instances behind an Application Load Balancer (ALB). The goal is to ensure traffic is evenly distributed, instances are isolated from direct public access, and health checks determine which instances receive traffic.

---

## Architecture

* **VPC**: Custom VPC
* **Subnets**:

  * 2 Public Subnets (different Availability Zones) – used by the ALB
  * 2 Private Subnets (different Availability Zones) – used by EC2 instances
* **Load Balancer**: Application Load Balancer (Internet-facing)
* **Compute**: 2 EC2 instances
* **Web Server**: Apache (installed via user-data)

All inbound traffic is handled by the ALB. EC2 instances are not publicly accessible.

---

## Objectives Achieved

* Deployed two EC2 instances across different Availability Zones
* Installed and configured a web server using EC2 user-data
* Created an Application Load Balancer with HTTP listener (port 80)
* Configured a target group with health checks
* Implemented proper security group isolation
* Verified round-robin traffic distribution

---

## Step-by-Step Implementation

### 1. EC2 Instances

* Launched **two EC2 instances** in the same VPC
* Instances placed in **different Availability Zones**
* Instances deployed in **private subnets** (no public IPs)
* Apache installed using user-data
* Each instance serves different content for testing

Example user-data:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "Hello from Instance 1" > /var/www/html/index.html
```

---

### 2. Application Load Balancer

* Created an **Internet-facing Application Load Balancer**
* Attached ALB to **two public subnets** (different AZs)
* Configured **HTTP listener on port 80**
* Created a **target group** using HTTP protocol
* Registered both EC2 instances
* Configured health check:

  * Protocol: HTTP
  * Path: `/`
  * Port: traffic port

---

### 3. Security Groups

#### ALB Security Group

* Inbound:

  * HTTP (80) from `0.0.0.0/0`
* Outbound:

  * All traffic allowed

#### EC2 Security Group

* Inbound:

  * HTTP (80) **only from ALB Security Group**
* Outbound:

  * All traffic allowed

This ensures EC2 instances cannot be accessed directly from the internet.

---

## Testing & Validation

### Load Balancer Test

* Accessed the ALB DNS name in a browser
* Observed successful page load
* Refreshed multiple times to confirm **round-robin load balancing**
* Verified responses alternated between instances

### Health Checks

* Checked Target Group health status
* Confirmed both EC2 instances were marked **Healthy**

### Local Instance Validation

* Connected to EC2 instances
* Verified Apache was running
* Used `curl localhost` to confirm local web server response

---

## Screenshots

The following screenshots are included to document the deployment:

* VPC and subnet configuration
* ALB configuration and listeners
* Target group health status
* Security group rules
* Browser test showing load balancing

---

## Key Learnings

* How Application Load Balancers distribute traffic
* Importance of health checks in high availability systems
* Proper use of security groups for isolation
* Difference between public and private subnets
* Real-world DevOps infrastructure patterns

---

## Conclusion

This assignment successfully demonstrates a production-style AWS architecture where EC2 instances are protected behind an Application Load Balancer. The setup ensures scalability, availability, and security while following AWS best practices.

