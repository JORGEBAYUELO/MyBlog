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
### Code Structure: Keeping It Clean
Instead of dumping everything into a single `.tf` file, I kept things **modular**. My project structure looked like this:
```bash
project-root/
│
├── main.tf                # Root module to call other modules
├── backend.tf             # Backend config for S3 + DynamoDB
├── providers.tf           # AWS provider setup
├── variables.tf           # Input variables
├── .gitignore             # Sensitive file exclusion
│
├── modules/
│   └── s3/                # Module for S3 backend bucket
│   └── ec2/               # EC2 instance and security group
│
└── user_data.sh           # Nginx installation script
```
Keeping modules separate allowed me to easily manage and reuse resources.
### Tools & AWS Services Used
Here’s a quick rundown of what was involved:
- **Terraform** – to define and deploy infrastructure
- **AWS EC2** – to host the web server
- **AWS S3** – to store Terraform remote state
- **DynamoDB** – to manage Terraform state locking
- **SSH Key Pairs** – to connect securely to EC2
- **Security Groups** – to control inbound/outbound traffic
- **User Data Script** – to automatically install Nginx
## Challenges & What I Learned:
No project is complete without a few hiccups. Here are a few bumps I hit along the way:
### AWS Credential Management
**Mistake:** I hardcoded my AWS `access_key` and `secret_key` into `providers.tf` at first.

**Fix:** I learned it’s best practice to use **environment variables** and add `providers.tf` to `.gitignore` to avoid accidentally pushing secrets to GitHub.
### S3 Bucket Conflicts
**Mistake:** I reused an existing S3 bucket name in two places — one for backend state, and another in a module. Since S3 bucket names are global, Terraform threw a `BucketAlreadyExists` error.

**Fix:** I gave each bucket a **unique name** and learned to avoid overlapping resource definitions between backend and module setups.
### Internet Access Inside EC2
**Issue:** After connecting to the EC2 instance via SSH, I couldn’t ping external servers.

**Fix:** I realized I hadn’t configured **outbound rules** in the security group. After adding egress rules to allow outbound internet traffic, it worked fine.
### DynamoDB Table Not Found
**Issue:** Terraform couldn't lock the state file because the **DynamoDB table didn’t exist** yet.

**Fix:** I manually created the DynamoDB table beforehand in AWS Console and retried the deployment.
## Conclusion:
### What Worked Well
- Using modules kept the project clean and maintainable.
- I got to test end-to-end infrastructure deployment.
- The EC2 instance booted up with Nginx, and I saw the welcome page using the public IP. Mission accomplished!
### What I’d Improve Next Time
- Automate DynamoDB table creation within Terraform itself.
- Use Terraform Cloud or AWS Secrets Manager to manage credentials better.
- Try deploying with CI/CD next time (maybe using GitHub Actions).
### Key Takeaways
- **Planning your architecture first** saves you time later.
- Don’t skip `.gitignore` and proper **secret management**.
- Learning comes from **debugging and breaking stuff** — don’t be afraid of it.
- Modular Terraform projects are easier to work with and scale.
Hope this breakdown gives you ideas for your own Terraform practice or AWS exploration. If you’re a career changer like me, just keep building and stay consistent, it clicks faster than you think.

If you’d like to see the entire code process feel free to check it on my GitHub Page  https://github.com/JORGEBAYUELO/Automated-AWS-Terraform-Infrastructure! 👨‍💻