# 🌐 AWS Hands-On Project: VPC + Auto Scaling + ALB 🚀
End-to-end AWS project showcasing a secure, highly available architecture with VPC, public/private subnets, Bastion Host, NAT Gateway, Auto Scaling Group, Application Load Balancer, and health checks. Includes setup notes, troubleshooting steps, and screenshots for reference.

This project demonstrates how to set up a **highly available web application architecture** on AWS using **VPC, Subnets, Auto Scaling, Bastion Host, NAT Gateway, Load Balancer, and Target Groups**.  
The setup ensures secure access, scalability, and fault tolerance.

---

## 📑 Table of Contents
- [Project Architecture](#project-architecture)
- [Screenshots](#screenshots)
- [How It Works](#how-it-works)
- [Troubleshooting & Fixes](#troubleshooting--fixes)
- [Outcomes](#outcomes)
- [Next Steps](#next-steps)
- [Demo](#project-demo)

---

## 🏗️ Project Architecture

- **VPC** with 2 Availability Zones (AZs)
- **Public Subnets** for Bastion Host & NAT Gateway
- **Private Subnets** for Auto Scaling Group (ASG) instances
- **Internet Gateway (IGW)** for external access
- **NAT Gateway** for private subnet outbound internet access
- **Auto Scaling Group (ASG)** with Min: 2, Max: 4 EC2 instances
- **Application Load Balancer (ALB)** distributing traffic to instances
- **Bastion Host** to securely SSH into private instances
- **Security Groups & NACLs** for traffic control

<img width="649" height="472" alt="Project_architechture" src="https://github.com/user-attachments/assets/13f34206-8136-4322-9d5b-3b8c0fce3f9c" />


---

## 📸 Screenshots

> _Upload screenshots in the `/screenshots` directory and link them below_

1️⃣ **VPC Overview**  
<img width="1882" height="760" alt="Step_1" src="https://github.com/user-attachments/assets/95335cd5-928a-4ed6-bfd1-a0a23a9cd779" />


2️⃣ **Auto Scaling Group**  
<img width="1918" height="856" alt="Step_2" src="https://github.com/user-attachments/assets/390e4187-427e-4b1e-a1d5-5719d2233c06" />
<img width="1918" height="797" alt="Step_6" src="https://github.com/user-attachments/assets/4c8e5600-e77b-4fe7-a448-bf22741cfda1" />
<img width="1908" height="478" alt="Step_8" src="https://github.com/user-attachments/assets/a29cc04a-a76d-408c-9c09-c057dc892027" />


3️⃣ **Bastion Host**  
<img width="952" height="852" alt="Step_10" src="https://github.com/user-attachments/assets/7d77e5fc-a197-4ca7-a45e-553ed55f49d5" />
<img width="1919" height="827" alt="Step_12" src="https://github.com/user-attachments/assets/660a7ff8-771d-4631-95fe-d2eb64e63fa6" />


4️⃣ **Load Balancer**  
<img width="501" height="797" alt="Step_20" src="https://github.com/user-attachments/assets/0ed26fb3-beb4-42c3-a680-474bff05b450" />
<img width="1737" height="733" alt="Step_21" src="https://github.com/user-attachments/assets/fb474747-da35-4cc3-93fe-b24ecb58d995" />
<img width="1917" height="578" alt="Step_23" src="https://github.com/user-attachments/assets/e116c99c-01b9-45e4-808d-cd7ffa3b5e00" />



5️⃣ **SSH Configurations**  
<img width="389" height="91" alt="Step_13" src="https://github.com/user-attachments/assets/11a10154-626e-4491-8c7a-0414043173a7" />
<img width="812" height="514" alt="Step_15" src="https://github.com/user-attachments/assets/e0c5e77a-d240-48e1-bbc3-98c3a796e551" />


---

## ⚡ How It Works

- Deployed Python HTTP server:
  ```bash
  python3 -m http.server 8000
  ```
- Added a simple `index.html` file inside both instances.
- Accessed application using **ALB DNS URL**, which serves HTML page from backend servers.

---

## 🚧 Troubleshooting & Fixes

- **502 Bad Gateway from Load Balancer**
  - *Cause:* Python HTTP server was not running, ALB health checks failed.
  - *Fix:* Created a startup script to auto-run the server on boot:
    ```bash
    #!/bin/bash
    cd /home/ubuntu
    nohup python3 -m http.server 8000 &
    ```
<img width="794" height="209" alt="Faced_issues_learnings_Step_27" src="https://github.com/user-attachments/assets/2bbd1368-3058-44f6-b3b2-344c89e7a2d3" />

- **SSH Timeout**
  - *Cause:* Direct SSH to private EC2s.
  - *Fix:* Used Bastion Host in public subnet, allowed inbound SSH (22), then hopped into private subnet via Bastion.

- **ALB Health Check Failing**
  - *Cause:* Health check path was `/` but HTML file was in subfolder.
  - *Fix:* Placed `index.html` in `/home/ubuntu/` and updated health check path.

---

## ✅ Outcomes

- Set up secure networking (VPC, subnets, NACLs, security groups).
- Built scalable architecture with Auto Scaling and Load Balancer.
- Practiced troubleshooting ALB health check failures & bad gateway errors.

---

## 🚀 Next Steps

- Automate with [Terraform](https://www.terraform.io/) or [CloudFormation](https://aws.amazon.com/cloudformation/)
- Deploy a real web app instead of a static HTML
- Integrate with CI/CD pipeline for zero-downtime deployments

---

## 🔗 Project final outcome

<img width="1919" height="909" alt="Step_26" src="https://github.com/user-attachments/assets/869899f0-b468-4228-82bf-ca192f5d7789" />
<img width="1674" height="371" alt="Step_30" src="https://github.com/user-attachments/assets/eaabe9c6-bef5-41de-9710-c266a8c8b3f8" />


---

## 📝 Setup Checklist

- ✅ Create VPC with public/private subnets across 2 AZs
- ✅ Set up IGW, NAT Gateway, and configure route tables
- ✅ Launch Bastion Host in public subnet, configure security group
- ✅ Create Auto Scaling Group in private subnets
- ✅ Deploy Application Load Balancer and Target Groups
- ✅ Configure health checks and test connectivity
- ✅ Add startup script to EC2 instances for Python server
- ✅ Validate access via Bastion Host and ALB DNS

Repository includes setup notes, screenshots, and troubleshooting steps.
