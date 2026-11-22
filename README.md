# AWS-Week-1-Assignment---Custom-V-PC-Network
AWS Cloud Computing Week 1 Assignment: Built a custom VPC (10.20.0.0/16) with 2 public and 2 private subnets across multiple AZs, Internet Gateway, Route Tables, and Secure Security Groups. Includes connectivity tests and architecture diagram.

# AWS Custom VPC Network Architecture

## 📌 Objective
Design and implement a segmented AWS network using multiple Availability Zones, public/private subnets, and controlled routing/security boundaries.

## 🏗️ Architecture Diagram
![Architecture](architecture.png)

## 🌐 CIDR & Subnet Plan
*   **VPC CIDR:** `10.20.0.0/16`
*   **Public Subnet 1 (ap-south-1a):** `10.20.1.0/24`
*   **Public Subnet 2 (ap-south-1b):** `10.20.2.0/24`
*   **Private Subnet 1 (ap-south-1a):** `10.20.3.0/24`
*   **Private Subnet 2 (ap-south-1b):** `10.20.4.0/24`

## 🚦 Routing Configuration
*   **Public Route Table:** Routes `0.0.0.0/0` to Internet Gateway (IGW) for internet access.
*   **Private Route Table:** No IGW route (ensures isolation). Routes S3 traffic through an S3 Gateway Endpoint.

## 🔒 Security Groups (Least Privilege)
*   **Public-Web-SG:** Allows HTTP (80) and HTTPS (443) from anywhere (`0.0.0.0/0`). Allows SSH (22) strictly from my IP address.
*   **Private-SG:** Allows ICMP and SSH (22) strictly from the VPC CIDR (`10.20.0.0/16`), not from the internet.

## 🧪 Connectivity Tests (Evidence in /screenshots)
1.  **Public Instance Internet Access:** Pinged google.com successfully (0% packet loss).
2.  **Public to Private Connectivity:** Successfully pinged private instance (`10.20.3.213`) from the public instance.
3.  **Private Instance Isolation:** Attempted to ping google.com from the private instance, resulting in **100% packet loss** (intentional block).
4.  **Bastion Host SSH:** Successfully SSH'd into the private instance via the public instance.

## 🚀 Deployment Steps
1. Created VPC with CIDR `10.20.0.0/16`.
2. Created 4 subnets across 2 AZs.
3. Attached Internet Gateway and configured public route table.
4. Configured private route table with S3 Gateway Endpoint.
5. Created Security Groups with least privilege access.
6. Launched Public EC2 (with Apache web server) and Private EC2.

## 🧹 Cleanup Steps
1. Terminated both EC2 instances.
2. Deleted Security Groups.
3. Deleted the VPC (which automatically removed subnets, route tables, IGW, and endpoints).
