---
title: Secure and Automated CI-CD Deployment of a React App with Secrets Management on AWS
date: 2025-06-26
draft: false
cover: /cloud_cover.jpg
---
## Introduction:

Over the past few weeks, I gave myself a personal challenge, to stop just _studying_ DevOps and actually start _doing_ DevOps. That meant real-world projects. Projects that solve a problem, follow best practices, and simulate what I’d be doing in a Cloud DevOps Engineer role. This is one of those projects.

Let me walk you through what I built, why I built it, and what I learned along the way.

## The Goal

I wanted to take a React application (a cappuccino calculator app I had already built) and create a full CI/CD pipeline for it. But it had to meet some key criteria:

- Infrastructure must be created using **Terraform**
    
- The app must be containerized with **Docker**
    
- The pipeline should be built using **GitHub Actions**
    
- The app should be deployed on **AWS EC2** (Free Tier only)
    
- A **webhook** should auto-deploy new changes
    
- Secrets must be handled **securely**
    
- Everything must reflect **cybersecurity best practices** I learned in the Google Cybersecurity certification
    

In short, I wanted this project to feel like something I would build on the job.

## The Problem This Solves

In many small teams and startups, automated deployment is still done manually—or worse, insecurely. You might have someone SSH-ing into the server to `git pull` and restart services.

This project solves that by building a **secure and automated deployment pipeline** that reduces manual steps and makes sure deployments are consistent and safe. It also serves as a reusable, reproducible blueprint that I (or any engineer) can use again in future projects.

## The Architecture at a Glance

```ascii
+-------------+         Push Event          +------------------+
|  Developer  | -------------------------> |  GitHub Actions  |
+-------------+                            +--------+---------+
                                                     |
                                                     | Docker Build & Push
                                                     v
                                           +--------------------------+
                                           |    Docker Hub Registry   |
                                           +------------+-------------+
                                                        |
                      GitHub Webhook (HTTP POST)        |
                                                        v
+------------------+   Trigger Auto-deploy   +--------------------------+
|   EC2 Instance   | <---------------------- |   GitHub Webhook System  |
| (Amazon Linux 2) |                         +--------------------------+
| Docker + Webhook |  Pull Image & Restart
+------------------+
```

## How I Built It

1. **Provisioned infrastructure with Terraform**: I created the VPC, EC2 instance, security groups, IAM roles, and SSH key pair—all modular and scalable.

2. **Dockerized the React app**: A clean `Dockerfile` builds the app and runs it using `serve`.

3.  **Created a GitHub Actions workflow**: On every push to `main`, the image is built and pushed to Docker Hub.

4. **Set up a webhook listener on the EC2 instance**: It watches for Docker pushes and auto-pulls the new image, stopping and restarting the container.

5. **Secured it all**: No hardcoded secrets. Everything sensitive is either in `.gitignore`, Terraform `.tfvars`, or GitHub Secrets.

## Challenges I Faced (and Solved)
### CIDR Confusion

I initially tried using my local IP with a `/24` subnet for SSH access, but Terraform threw errors because the CIDR block was invalid. The fix? I had to get my public IP instead using the website **[What Is My IP](https://www.whatismyip.com/)** and using the  `/32` at the end of the CIDR block.

### Terraform Data Source Mismatch

I mistakenly used `aws_subnet_ids`, which doesn’t exist. Switching to `aws_subnets` resolved the issue. Lesson learned: always double-check Terraform documentation.

### DockerHub Login Fails in GitHub Actions

Even after generating an access token, Docker login kept failing. The issue? I had copied the wrong username. It’s those small details that trip you up.

### Webhook Not Triggering

Everything looked right, but the webhook wasn’t firing. Turned out I hadn’t opened port `9000` on the EC2 instance. Once added to the security group, it worked instantly.

## What I Learned

- **Terraform module structure**: I now understand how to split Terraform logic into maintainable pieces and what should live in `main.tf`.
    
- **Secrets management best practices**: Using `.tfvars`, GitHub Secrets, and `.gitignore` to keep sensitive info out of version control.
    
- **CI/CD workflow design**: From build, to push, to deploy—all automated and observable.
    
- **Security awareness**: I implemented IAM roles, network security groups, and API access controls with care.
    

This wasn’t just “a project” for me—it was a full DevOps lifecycle in action.

## Final Thoughts

This project made me feel like I was _actually doing the job_. It also gave me a ton of confidence in my Terraform, Docker, and CI/CD skills. And it’s 100% reproducible, which means I can re-use this architecture for any frontend project in the future.

If you’re someone trying to break into DevOps, my advice is simple: don’t just watch tutorials—**build something**. Something messy. Something real. You’ll learn way more than you expect.

And if you want to check out the repo and try it yourself, here it is:

👉 [GitHub Repository](https://github.com/JORGEBAYUELO/secure-and-automated-cicd)

Thanks for reading. More projects coming soon—this was only Project #2 out of 50+ on the roadmap.


