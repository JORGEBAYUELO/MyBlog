---
title: How I Built a Fully Automated AWS Infrastructure Using Terraform
date: 2025-04-04
draft: false
cover: /terraform_cover.jpg
---
## Introduction:
Building real world projects is one of the best ways to learn, and that’s exactly what I wanted to do with this one. This project was my way of testing everything I’ve been learning about **DevOps, AWS, and Infrastructure as Code** in a practical and hands-on way.

So, what’s the project about?  
I created a **fully automated cloud infrastructure on AWS using Terraform**, where I deployed a web server (Nginx) inside an EC2 instance. The infrastructure is managed entirely through code and includes everything from networking and security group configuration to remote state management using S3 and DynamoDB.

## Why I Built It:
As someone who is **self-taught** and **switching careers into DevOps**, I’ve been putting a lot of time into learning tools like Terraform. But reading docs and watching videos only go so far. I needed to _build_ something real to solidify the concepts and test myself.
This project helped me:
- Practice writing and organizing Terraform code.
- Understand AWS services and how they work together.
- Improve my ability to troubleshoot real-world issues.
## What You'll Learn From This Blog:
By the end of this post, you’ll have a clear idea of:
- How to set up a simple but complete AWS infrastructure using Terraform.
- The challenges I faced and how I debugged them.
- Best practices I followed and some that I learned the hard way.
## Project Breakdown: How I Built It
### Designing the Architecture
I started by outlining what I wanted the infrastructure to include:
- A single **EC2 instance** running Nginx
- A **Security Group** allowing inbound SSH and HTTP access
- Remote **Terraform state** stored in **S3** with a **DynamoDB** table to manage state locks
- All resources defined and deployed using **Terraform**
This setup mimics real-world cloud architecture in a simplified way, this is enough to learn without getting overwhelmed.
### Why I Chose Terraform
Terraform is one of the most popular Infrastructure as Code tools in the industry, and it integrates really well with AWS. I chose it because:
- It’s **cloud-agnostic**
- It’s **declarative**, meaning you describe what you want, not how to do it
- It has **great community support**
- And frankly, it’s everywhere in job postings 😅
## Conclusion
This section is to express our final thoughts and takeaways