---
title: React App on AWS EKS with ArgoCD (GitOps + CI/CD)
date: 2025-10-10
draft: false
cover: /AWS_EKS.jpg
---
## Introduction:

Kubernetes is everywhere right now. If you look at the job market, “Kubernetes” and “cloud-native” skills are some of the most in demand keywords for DevOps engineers. That’s why I wanted to take one of my own projects, a simple React app and deploy it the way real world companies do: on AWS EKS with GitOps, powered by ArgoCD and CI/CD pipelines.

This post walks through **why I built this project**, **how the infrastructure was set up**, and the **lessons learned along the way**.

## Why This Project?

The goal was simple:

- Take a React app I built.
- Containerize it with Docker.
- Deploy it on AWS EKS (Elastic Kubernetes Service).
- Automate deployments using ArgoCD and GitHub Actions.

Why? Because Kubernetes is the de facto standard for modern infrastructure, and pairing it with GitOps (via ArgoCD) and CI/CD (via GitHub Actions) is exactly how real-world DevOps pipelines work today.

Instead of just running `kubectl apply` manually, I wanted to prove that I could set up a **self-updating system**: code changes → new Docker image → ArgoCD syncs → app updated in the cluster.

## Setting Up the Infrastructure

The backbone of this project is an **EKS cluster on AWS**. I spun it up using `eksctl` because it’s quick and reliable for demos:

```bash
eksctl create cluster --name portfolio-eks --region us-east-1 --nodes 2 --node-type t3.medium
```

This gave me:

- A managed Kubernetes control plane.
- Two EC2 worker nodes where my pods would run.

On top of that cluster, I installed:

- **NGINX** → to serve the React app.
- **ArgoCD** → for GitOps continuous delivery.
- **Metrics server** → for basic monitoring.

With those components in place, the cluster was ready to host the app.

## From Code to Kubernetes

Here’s the flow I implemented:

1. **React App → Docker**
    
    - I wrote a Dockerfile that builds my React app and serves it via NGINX.
    - The image gets pushed to Docker Hub every time I update my repo.
        
2. **GitHub Actions → CI/CD**
    
    - GitHub Actions builds and pushes the Docker image automatically when I push to `main`.
    - No manual Docker pushes. Everything is automated.
        
3. **Kubernetes Manifests → GitOps**
    
    - I stored `deployment.yaml` and `service.yaml` in a `k8s/` directory inside my repo.
    - These define how Kubernetes runs my app.
        
4. **ArgoCD → Automated Sync**
    
    - ArgoCD watches my GitHub repo.
    - When new changes are detected in the `k8s/` folder, it automatically syncs and updates the cluster.

End result? My app redeploys itself whenever I push code.

## Challenges & Lessons Learned

Like any real-world project, not everything went smoothly. Here are a few things I ran into:

- **Pods stuck in Pending** → At first, I tried running the cluster on `t3.micro` nodes. That didn’t work because the pods needed more resources than the tiny instance could provide. Switching to `t3.medium` nodes solved the issue.
- **Cluster upgrades** → When experimenting with EKS version upgrades, I learned that the control plane and node groups need to be upgraded carefully. Otherwise, workloads can get disrupted.
- **CI/CD with `latest` tag** → Using `:latest` and `imagePullPolicy: Always` works fine for demos, but in production, versioned tags are the way to go.
- **GitOps clarity** → Once ArgoCD was set up, deployments became hands-off. I didn’t need `kubectl apply` anymore, which really shows the power of GitOps.

## Final Thoughts

This project gave me hands-on experience with one of the most relevant DevOps stacks today: **React + Docker + Kubernetes + GitHub Actions + ArgoCD + AWS EKS**.

The main takeaway? **Kubernetes can be tricky to set up, but once it’s running with GitOps, deployments become effortless.**

For anyone preparing for a DevOps career, I highly recommend building a project like this. It not only shows you understand the tools, but also proves you can solve real-world deployment challenges.

And if you want to check out the repo and try it yourself, here it is:

[GitHub Repository](https://github.com/JORGEBAYUELO/Catppuccino_App)
