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

