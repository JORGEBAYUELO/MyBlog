---
title: Building a Full DevOps Pipeline on AWS with Terraform, EKS, ECR, ArgoCD, Helm, Prometheus, Grafana, and Vault
date: 2025-10-20
draft: false
cover: /aws-eks-infrastructure-deployment-cover.jpg
---
## Introduction: 

As DevOps engineers, we constantly strive to build systems that are **scalable, automated, and observable**. In this project, I set out to design a **complete DevOps pipeline** from scratch, one that goes beyond a simple CI/CD workflow and brings together the best practices of **infrastructure-as-code (IaC)**, **container orchestration**, **security**, and **monitoring** under one unified architecture.

The result was a fully automated, cloud-native environment that leverages **AWS infrastructure**, **GitOps automation**, and **secure secret management**, all built from scratch with cost efficiency in mind.

This project not only strengthened my technical depth but also simulated what a production-grade DevOps setup might look like in a mid-size cloud-native company.

## Why I Built This Project

The goal was clear: **to create a reproducible, modular DevOps pipeline** that could demonstrate end-to-end automation, from infrastructure provisioning all the way to application deployment and observability.

The project had three main goals:

1. **Infrastructure as Code (IaC)** using Terraform — to ensure every component of the environment could be versioned, reviewed, and reproduced.
2. **GitOps Continuous Delivery** using ArgoCD — so that all deployments would be driven by Git commits, eliminating manual kubectl commands.
3. **Observability and Security** through Prometheus, Grafana, and Vault — providing visibility into metrics and secure handling of sensitive information.

In essence, this project reflects how modern DevOps teams **own the full lifecycle**, from code to cluster to observability.
## How It All Came Together

### 1. Terraform: The Foundation

Terraform was used to provision the entire AWS infrastructure, **VPC, subnets, EKS cluster, IAM roles, security groups, and ECR repository**, all written as reusable modules.

One improvement I implemented was **S3 native locking** for state management instead of DynamoDB. This newer approach leverages **S3’s object lock feature**, ensuring immutability and safe concurrency without an additional DynamoDB dependency, a cleaner and cost-effective solution.

Terraform didn’t just create the cluster; it orchestrated the **network layout**, **security layers**, and **ECR repository** used to store the application’s Docker images.

### 2. ECR & CI/CD Integration

The CI/CD pipeline was built around **GitHub Actions** and **Amazon ECR**.

- Every time a new commit was pushed to the React app’s repository, GitHub Actions would **build the Docker image**, **tag it**, and **push it to ECR**.
- This pipeline ensured the application container was always up to date and ready to be deployed into Kubernetes.

By automating builds and image storage, we eliminated the need for manual container management, a crucial step for scalable DevOps.

### 3. EKS + ArgoCD: GitOps in Action

Once the infrastructure was provisioned, **ArgoCD** became the brain of our delivery process.

- A **separate private Git repository** stored the Kubernetes manifests (`deployment.yaml`, `service.yaml`, and `kustomization.yaml`).
- ArgoCD continuously monitored that repository and automatically synchronized any changes into the **EKS cluster**.

This approach embodies the **GitOps model**, where Git isn’t just the source of truth for code, it’s the source of truth for the entire system state.

Whenever a developer updated configuration files in Git, ArgoCD detected the change and reconciled it in the cluster. No kubectl apply needed. Just commit, push, and watch the deployment roll out in real time.

### 4. Helm: Infrastructure Application Management

While ArgoCD handled app deployments, **Helm** was used to install and manage third-party services like **Prometheus**, **Grafana**, and **Vault**.

Helm’s modular nature allowed us to:

- Install complex applications with a single command.
- Manage configurations declaratively.
- Keep infrastructure tools versioned and easily upgradable.

This separation, **ArgoCD for app delivery** and **Helm for infra tooling**, kept the cluster clean, organized, and maintainable.

### 5. Prometheus & Grafana: Observability Layer

With **Prometheus** deployed via Helm, the cluster gained real-time observability. Prometheus scraped metrics from every node and pod, tracking CPU, memory, and container health.

**Grafana** was then configured to use Prometheus as a data source, visualizing these metrics through custom Kubernetes dashboards.

This combination transformed raw metrics into actionable insights, the kind of visibility DevOps teams rely on to keep systems running smoothly.

### 6. Vault: Secure Secret Management

Security was handled by integrating **HashiCorp Vault**, also installed via Helm. Vault provided a centralized way to manage sensitive credentials and environment secrets securely within Kubernetes.

By using Vault’s **agent injector**, applications could automatically pull their secrets from Vault at runtime without embedding them in the codebase, a professional-grade practice for production-grade deployments.

### 7. Cost Optimization & Cleanup

Since this project was built using AWS, cost control was key. Resources were chosen carefully, most components ran on the **AWS Free Tier**, with **on-demand cleanup via Terraform destroy** at the end of each test cycle.

Any leftover components like **Load Balancers or EBS volumes** were identified and deleted manually to ensure a clean, cost-free environment.
## Lessons Learned

Every project teaches something new, even after years of hands-on DevOps work. These were my key takeaways:

1. **GitOps simplifies everything**, once you set up ArgoCD properly, deployments feel almost magical.
2. **S3 native state locking** is a cleaner and more modern alternative to DynamoDB for Terraform state protection.
3. **Observability is not optional**, integrating Prometheus and Grafana early gives you clarity and confidence during development.
4. **Vault is powerful but needs careful planning**, understanding policies and injection workflows is key for scaling securely.
5. **Helm and ArgoCD complement each other beautifully**, one handles system tools, the other manages application delivery.

## Key Takeaways

This project was designed to simulate a **real-world DevOps workflow**, from infrastructure provisioning to continuous delivery and monitoring. All while keeping costs minimal and security high.

It demonstrates the power of combining **Terraform**, **EKS**, **ECR**, **Helm**, **ArgoCD**, **Prometheus**, **Grafana**, and **Vault** into a unified DevOps ecosystem.

The end result:
[x] A fully automated pipeline from **GitHub to AWS EKS**
[x] Real-time monitoring and metrics visualization
[x] Secure secret management with Vault
[x] Zero manual deployment intervention

## Final Thoughts

Building this environment reinforced a core DevOps truth, automation and visibility are the lifeblood of any scalable system. When your infrastructure, deployments, and monitoring work together seamlessly, developers move faster and operations teams sleep better.

This project wasn’t about building something flashy, it was about building something **solid, secure, and production-ready**.

if you want to check out the repo and try it yourself, here it is:

[GitHub Repository](https://github.com/JORGEBAYUELO/terraform-aws-devops-pipeline-eks-ecr-helm-vault?tab=readme-ov-file)

**Created by Jorge Bayuelo — AWS Certified | Cloud DevOps Engineer**