# 🔐 AWS Project 2 Secure AWS VPC with Public & Private Subnets

A hands-on AWS networking project demonstrating a secure Virtual Private Cloud (VPC) with public and private subnets, EC2 instances, an Internet Gateway, and a NAT Gateway.

## 📌 Project Overview

This project demonstrates:
- 1 VPC
- 1 Public Subnet
- 1 Private Subnet
- 1 Internet Gateway
- 1 NAT Gateway
- 2 EC2 instances (Public and Private)
- Separate public and private route tables
- Security Groups
- Connectivity and security testing

### Architecture

```text
                         🌐 INTERNET
                              |
                    ┌─────────────────┐
                    │ Internet Gateway│
                    │      (IGW)      │
                    └────────┬────────┘
                             |
                    ┌────────▼────────┐
                    │      VPC        │
                    │   10.0.0.0/16   │
                    └────────┬─────────┘
                             |
             ┌───────────────┴────────────────┐
             │                                │
     ┌───────▼────────┐              ┌────────▼────────┐
     │ Public Subnet  │              │ Private Subnet  │
     │ 10.0.1.0/24    │              │ 10.0.2.0/24     │
     └───────┬────────┘              └────────┬────────┘
             │                                │
     ┌───────▼────────┐              ┌────────▼────────┐
     │   Public EC2   │              │   Private EC2   │
     │   Public IP    │              │  No Public IP   │
     └────────────────┘              └────────┬────────┘
                                             │
                                    ┌────────▼────────┐
                                    │   NAT Gateway   │
                                    │  Public Subnet  │
                                    └────────┬────────┘
                                             │
                                             ▼
                                          INTERNET
```

## 🛠️ AWS Resources

| Resource | Name | Configuration |
|---|---|---|
| VPC | `Secure-VPC-Project` | `10.0.0.0/16` |
| Public Subnet | `Public-Subnet` | `10.0.1.0/24` |
| Private Subnet | `Private-Subnet` | `10.0.2.0/24` |
| Internet Gateway | `Secure-VPC-IGW` | Attached to VPC |
| NAT Gateway | `Project-NAT-Gateway` | Public Subnet |
| Public Route Table | `Public-Route-Table` | Internet Gateway |
| Private Route Table | `Private-Route-Table` | NAT Gateway |
| Public EC2 | `Public-EC2` | Public IPv4 enabled |
| Private EC2 | `Private-EC2` | Public IPv4 disabled |
| Public Security Group | `Public-EC2-SG` | SSH from My IP |
| Private Security Group | `Private-EC2-SG` | SSH from Public EC2 SG |

## 🚀 Implementation

### 1. VPC
```text
Name: Secure-VPC-Project
CIDR: 10.0.0.0/16
Region: ap-south-1
```

### 2. Subnets
```text
Public-Subnet  → 10.0.1.0/24
Private-Subnet → 10.0.2.0/24
```

### 3. Internet Gateway
Create `Secure-VPC-IGW` and attach it to `Secure-VPC-Project`.

### 4. Public Route Table

```text
10.0.0.0/16 → local
0.0.0.0/0   → Internet Gateway
```

Associate it with `Public-Subnet`.

### 5. NAT Gateway

Create `Project-NAT-Gateway` in `Public-Subnet` with a new Elastic IP.

> ⚠️ NAT Gateway is a paid AWS resource. Delete it after completing the lab to avoid unnecessary charges.

### 6. Private Route Table

```text
10.0.0.0/16 → local
0.0.0.0/0   → NAT Gateway
```

Associate it with `Private-Subnet`.

### 7. EC2 Instances

**Public EC2**
```text
Name: Public-EC2
AMI: Amazon Linux 2023
Subnet: Public-Subnet
Auto-assign Public IP: Enabled
```

**Private EC2**
```text
Name: Private-EC2
AMI: Amazon Linux 2023
Subnet: Private-Subnet
Auto-assign Public IP: Disabled
```

## 🧪 Connectivity Testing

### Public EC2 → Internet

```bash
curl -I https://example.com
```

Check public IP:

```bash
curl https://checkip.amazonaws.com
```

### Private EC2 → Internet

After accessing the private instance:

```bash
curl -I https://example.com
```

The expected behavior is:

```text
Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet
```

## 🔐 Security Behavior

| Resource | Public IP | Outbound Internet | Direct Inbound from Internet |
|---|---|---|---|
| Public EC2 | ✅ Yes | ✅ Yes | Controlled by Security Group |
| Private EC2 | ❌ No | ✅ Via NAT Gateway | ❌ No |

The private EC2 can initiate outbound connections through the NAT Gateway, but it is not directly reachable from the internet because it has no public IP.

## 📸 Screenshots

Recommended repository structure:

```text
screenshots/
├── 01-vpc.png
├── 02-public-subnet.png
├── 03-subnets.png
├── 04-internet-gateway.png
├── 05-public-route-table.png
├── 06-nat-gateway.png
├── 07-private-route-table.png
├── 08-public-ec2.png
├── 09-public-connectivity.png
└── 10-private-ec2.png
```

Add screenshots to this directory and reference them in this README.

## 📋 Project Checklist

- [x] Create VPC
- [x] Create Public Subnet
- [x] Create Private Subnet
- [x] Create Internet Gateway
- [x] Create Public Route Table
- [x] Create NAT Gateway
- [x] Create Private Route Table
- [x] Launch Public EC2
- [x] Launch Private EC2
- [x] Configure Security Groups
- [x] Test Public EC2 internet access
- [x] Test Private EC2 internet access
- [x] Demonstrate security behavior

## 💰 Cost Considerations

NAT Gateway is a paid AWS service. For a learning project:

1. Complete the configuration.
2. Perform the tests.
3. Capture screenshots.
4. Delete the NAT Gateway.
5. Delete the remaining lab resources when finished.

## 🧹 Cleanup

Recommended cleanup order:

```text
EC2 Instances
     ↓
NAT Gateway
     ↓
Elastic IP
     ↓
Route Tables
     ↓
Internet Gateway
     ↓
Subnets
     ↓
VPC
```

## VPC Resource Map

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/36adf8db39eabc4566a412a28ac7c29a47ffeecf/VPC%20map.png)


## Create VPC

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/74fa5add8637ef6df6bd452468cfdb78902e3735/Create%20VPC.png)

## Public Subnet

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/4234e3f72aac8e9429c6ad204c55abb47e5799bd/Create%20Public%20Subnet.png)

## Private Subnet

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/4234e3f72aac8e9429c6ad204c55abb47e5799bd/Create%20Private%20Subnet.png)

## Internet Gateway

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/4234e3f72aac8e9429c6ad204c55abb47e5799bd/Create%20IGW.png)

## Public Route Table

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/4234e3f72aac8e9429c6ad204c55abb47e5799bd/Public-Route-Table.png)

##  NAT Gateway

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/36adf8db39eabc4566a412a28ac7c29a47ffeecf/NAT-Gateway.png)

## Private Route Table

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/36adf8db39eabc4566a412a28ac7c29a47ffeecf/Private-Route-Table.png)

## Public EC2 Instance

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/36adf8db39eabc4566a412a28ac7c29a47ffeecf/Public-EC2-Instances.png)

## Private EC2 Instance

![image alt](https://github.com/nileshpatil182004-design/AWS-Secure-VPC-project/blob/36adf8db39eabc4566a412a28ac7c29a47ffeecf/Private-EC2-Instance.png)
## 🎯 Learning Outcomes

After completing this project, you should understand:

- AWS VPC fundamentals
- Public vs. private subnets
- CIDR addressing
- Route tables
- Internet Gateway
- NAT Gateway
- EC2 networking
- Security Groups
- Public and private IPv4 addressing
- Private subnet outbound connectivity
- Basic AWS network security

## ⭐ Conclusion

This project demonstrates a practical AWS VPC architecture that separates public and private resources while allowing private resources to access the internet securely through a NAT Gateway.

**Project Type:** AWS Cloud / Networking / DevOps Hands-on Project

**Technologies:** `AWS VPC` · `EC2` · `Subnet` · `Route Table` · `Internet Gateway` · `NAT Gateway` · `Security Groups`

## Author :- Nilesh Pradeep Patil
