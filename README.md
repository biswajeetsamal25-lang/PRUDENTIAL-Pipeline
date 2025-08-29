# PRUDENTIAL-CI/CD Pipeline Documentation

This document explains how our **CI/CD process** works for Prudential Project.

---

## CI Pipeline (Continuous Integration)

The CI pipeline runs whenever new code is pushed or a pull request is created.

### Steps:

1. **Checkout Code** – Gets the latest code from GitHub.
2. **Build Docker Image** – Creates a Docker image of the application.
3. **Run Security Scan** – Scans the Docker image with *Trivy* for vulnerabilities.
4. **Push Image** – Pushes the image to Docker Hub Registry.
5. **Create Change Request (CR)** – Sends details (commit, image, scan results, branch) to the Service Now ticketing system. The CR status is set to **PENDING**.

---

## CD Pipeline (Continuous Deployment)

The CD pipeline is triggered **automatically** when a Change Request is approved. The Service Now sends a **webhook** request to GitHub to start the deployment workflow.

### Steps:

1. **Webhook Trigger** – When CR is approved, the ticketing system calls GitHub’s workflow.
2. **Fetch CR Details** – Pipeline fetches the image and branch info from the CR.
3. **Checkout Helm Charts** – Pulls the Helm repo to prepare for deployment.
4. **Update Image in Helm Values** – Updates the image repo and tag in `values.yaml`.
5. **Deploy to Kubernetes** – Uses Helm to deploy or upgrade the app in the cluster.
6. **Update CR Status** – Sends back status (e.g., `DEPLOYED`) to the ticketing system.

---

## 🔄 Flow Diagram

```mermaid
flowchart TD
    A[Developer pushes code] --> B[CI Pipeline]
    B --> C[Build & Scan Docker Image]
    C --> D[Push Image to Registry]
    D --> E[Create Change Request]
    E -->|Approved| F[Webhook to GitHub]
    F --> G[CD Pipeline]
    G --> H[Update Helm Values]
    H --> I[Deploy to Kubernetes]
    I --> J[Update CR Status]
```

---
# CI/CD Tools - One Liner Overview

Brief descriptions and advantages of tools used in GitHub Actions CI/CD pipeline.

---

## 1. ServiceNow  
**Description:** ITSM platform for automating workflows and approvals.  
**Advantage:** Adds automated change approvals and compliance checks.

---

## 2. Trivy  
**Description:** Vulnerability scanner for containers and code.  
**Advantage:** Fast security scans for images and dependencies.

---

## 3. Docker  
**Description:** Tool to build and run containerized applications.  
**Advantage:** Ensures consistent app environments and easy deployment.

---

## 4. Docker Hub  
**Description:** Cloud registry for Docker images.  
**Advantage:** Stores and shares images with CI/CD automation support.

---

## 5. Helm  
**Description:** Kubernetes package manager using charts.  
**Advantage:** Simplifies app deployment and versioning in K8s.

---


