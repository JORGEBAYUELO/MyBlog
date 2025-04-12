---
title: "Next-Level DevOps: Deploying My React App via CI/CD on AWS with Docker"
date: 2025-04-11
draft: false
cover: /cicd&docker.jpg
---
## Introduction:
There’s something deeply satisfying about pushing a button (or committing a line of code) and watching your entire deployment pipeline go to work automatically, reliably, and without lifting another finger. That’s the core of what I wanted to build with this project: **a hands-on DevOps pipeline that connects GitHub Actions to AWS, Docker, and a live running application**.

While the React app itself is a cappuccino companion I built for fun (because yes, I love coffee), the main goal of this project wasn’t to launch the next big coffee app, it was to **learn and implement a production-like CI/CD workflow** from scratch, and do it using AWS free tier services.
## The App Behind the Pipeline:
Before we dive into the tech, a little backstory. I’m a big fan of cappuccinos and ratios matter. I couldn’t find a good app that showed espresso-to-milk ratios the way I wanted, so I built one using **React**. It’s a simple, clean single-page app that visualizes cappuccino recipes in a helpful and intuitive way.

But here’s the twist: building the app was just the beginning. I wanted it to live on the cloud. I wanted it deployed using GitHub Actions. And I wanted it containerized. That’s where the real journey began.
## Project Overview: From Code to Container to Cloud
This project was all about building an end-to-end DevOps pipeline with the following stack:
- **React.js** – the frontend app
- **Docker** – to containerize the app
- **GitHub Actions** – for continuous integration and deployment
- **Amazon EC2** – as the deployment target
- **NGINX** – acting as the reverse proxy on the server
- **systemd** – to manage Docker on boot
- **Amazon CloudWatch Agent** – for monitoring system-level metrics

Here’s what the pipeline looks like in action:
1. **Code push to `main` branch on GitHub**.
2. GitHub Actions:
	- Builds the Docker image
	- Pushes it to **Docker Hub**
	- SSHes into an **Amazon EC2 instance**
	- Pulls the latest Docker image
	- Stops and removes the old container (if any)
	- Runs the new container with NGINX as reverse proxy

All without me touching the server. Just commit, push, done.
## Challenges I Faced and How I Solved Them:
This project definitely came with its fair share of learning moments:
### SSH Key Issues
At one point, GitHub Actions kept failing to connect to the EC2 instance. The error? `ssh.ParsePrivateKey: ssh: no key found`. Turns out, it was due to how GitHub Actions was reading the key. Re-generating the PEM file and ensuring the formatting was preserved in GitHub secrets solved it.
### SSH Access from GitHub Actions
By default, my EC2 security group only allowed SSH from my IP. But GitHub Actions runners use dynamic IPs. For testing, I temporarily opened port 22 to `0.0.0.0/0`, but I learned that **for production**, I'd want to [fetch the GitHub Actions IPs](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-githubs-ip-addresses) and restrict access accordingly.
### Docker Permissions
Another tricky one `docker ps` failed at first. EC2's default user didn't have permission to run Docker without `sudo`. The fix? Simply prefix Docker commands in the script with `sudo`.
### Monitoring with CloudWatch Agent
Installing and configuring the CloudWatch Agent gave me visibility into CPU, memory, and disk usage on the EC2 instance. It was a great intro to system monitoring on AWS.
## What I Learned
This project brought a lot of moving pieces together, and each of them deepened my understanding of DevOps in a real world context:
- Writing shell scripts that run remotely
- Debugging SSH and permission issues
- Managing Docker images in CI/CD
- Configuring system services using `systemd`
- Working with AWS EC2, Security Groups, and IAM roles
- Monitoring with CloudWatch Agent

More importantly, it taught me the value of **automation**. Once the pipeline was in place, deployments became painless and repeatable. That’s the power of DevOps.
## Final Thoughts
If you’re breaking into DevOps (like I am), don’t just watch tutorials, **build something**. Use a real app (even if it’s a passion project like my coffee app), and deploy it like you’re going to production. Every problem you hit will be a chance to learn something new.

And if you're looking to build something similar, stay tuned. I'll also publish a **step-by-step guide in my GitHub repository** with every command and configuration I used. Feel free to check it here -> https://github.com/JORGEBAYUELO/CappuccinoCupApp

Until next time, happy automating! 🚀