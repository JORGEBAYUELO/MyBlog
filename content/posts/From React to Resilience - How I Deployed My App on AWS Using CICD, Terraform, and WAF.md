---
title: From React to Resilience - How I Deployed My App on AWS Using CICD, Terraform, and WAF
date: 2025-06-20
draft: false
cover: /image.jpg
---
## Introduction: 
Not long ago, I set out to deploy a simple React app, nothing fancy, just something I built with Tailwind and a bit of JavaScript.

But then I thought:

> _What if I treated this like a real production app — the kind I’d work on as a Cloud DevOps Engineer?_

What started as a personal project quickly evolved into a full-blown DevOps lab with automation, infrastructure as code, and a serious focus on security.

This post walks you through what I built, how I built it, and why it matters, especially if you're trying to break into the DevOps field (just like me).

## What I wanted to Solve
We all know pushing files to a server manually is not sustainable. It’s fragile, inconsistent, and not how real teams deploy production apps.

So I asked myself:
-  How can I automate everything from build to deploy?
-  Can I do this using only the AWS Free Tier?
-  And while I’m at it… can I add cybersecurity best practices to the stack?

Spoiler alert: I did all three ✅

## The Stack I Used (And Why It Matters)
Every tool I picked here was chosen for a reason — to simulate what a real DevOps Engineer would use in production:

|Tool|Why I Used It|
|---|---|
|**Terraform**|Define AWS infrastructure as code (S3, CloudFront, IAM, WAF)|
|**GitHub Actions**|Automate the build and deploy steps|
|**S3**|Host the React app as a static website|
|**CloudFront**|Serve the app globally with caching and HTTPS|
|**AWS WAF + Shield**|Add application-layer protection and security visibility|
|**SSM Parameter Store**|Store secrets securely (no hardcoded credentials!)|
|**React + Tailwind**|My actual frontend app|

And yes, _everything_ was built using Terraform. No clicking around in the AWS Console.

## How the CI/CD Pipeline Works
Here's what happens every time I push to the `main` branch:
1. GitHub Actions checks out the repo and installs dependencies
2. React app is built using `npm run build`
3. The `dist/` folder gets uploaded to S3
4. CloudFront cache is invalidated so users always get the latest version
5. If something breaks? I’ve got versioning enabled and can roll back to a previous S3 version (we even added rollback logic in the workflow!)

This is real CI/CD automated, repeatable, and resilient.

## Security: Not Just an Afterthought
Since I recently completed Google’s Cybersecurity course, I wanted to apply some of that knowledge to this lab.

So I added:
-  ✅ IAM with least privilege: My GitHub Actions user only has access to S3, CloudFront, and SSM
-  ✅ AWS WAF (Web Application Firewall): Protects my CloudFront distribution with managed rules
-  ✅ AWS Shield Standard: Automatically enabled to mitigate DDoS attacks
-  ✅ No hardcoded credentials: Secrets are pulled securely from AWS SSM Parameter Store

Security was baked into the infrastructure, not patched on at the end.

## Lessons Learned
Here are some things that stood out during this build:
-  **CloudFront + WAF Integration Can Be Finicky**: You need to be very precise with ARN formats and wait for propagation. Patience is part of DevOps too 😅
-  **SSM Parameter Store is super underrated**: It’s a clean, scalable way to manage secrets and configs across services.
-  **CI/CD isn't just automation, it's safety**: Every time I deploy, I know it’ll be consistent. And if it fails, I can recover quickly.

## Why This Project Matters
This wasn’t about getting something online, it was about **doing things the right way**.

It showed me how to:
-  Use Terraform to manage cloud infrastructure
-  Automate deployments with GitHub Actions
-  Apply real-world security practices in AWS
-  Build rollback mechanisms and cloud-native pipelines

And best of all: I now have a project in my portfolio that’s **as close to a real job task as it gets**.

## What’s Next?
This was just Project 1 in my plan to complete **50 real-world DevOps projects**. Each one will get more complex, and every one will:

-  Solve an actual DevOps problem
-  Use production-grade tooling
-  Be 100% replicable by anyone following my steps

## Want to Try It Yourself?
I’ve documented everything in my [GitHub repo](https://github.com/JORGEBAYUELO/react-s3-deploy) with a detailed README.

And if you want to see how this pipeline works from start to finish, keep checking this blog, every new project will be broken down just like this one.
