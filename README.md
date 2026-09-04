# End-to-End Kubernetes Three-Tier DevSecOps Project

An end-to-end **DevSecOps pipeline** that builds, secures, and deploys a three-tier web application (React frontend, Node.js backend, MongoDB database) on **Amazon EKS**, using **Terraform**, **Jenkins**, **SonarQube**, **Trivy**, **OWASP Dependency-Check**, **ArgoCD**, and **Prometheus/Grafana** for GitOps-driven continuous delivery and monitoring.

---

## 📐 Architecture

```
Developer → GitHub → Jenkins CI/CD → SonarQube (code quality)
                                    → OWASP Dependency-Check (SCA)
                                    → Trivy (image/file-system scan)
                                    → Docker Build → Amazon ECR
                                    → ArgoCD (GitOps CD) → Amazon EKS
                                                              ├── Frontend (React)
                                                              ├── Backend (Node.js)
                                                              └── MongoDB
                          AWS ALB Ingress → Application
                          Prometheus + Grafana → Cluster & App Monitoring
```

The infrastructure is provisioned as code, the pipeline shifts security left (SAST/SCA/image scanning before deployment), and delivery to Kubernetes is handled declaratively through ArgoCD rather than direct `kubectl apply` from Jenkins.

---

## 🗂️ Repository Structure

| Folder | Description |
|---|---|
| `Application-Code/` | Source code for the three-tier application (frontend, backend, and database configs) |
| `Jenkins-Pipeline-Code/` | Jenkinsfiles defining the CI/CD pipelines for frontend and backend builds |
| `Jenkins-Server-TF/` | Terraform code to provision the Jenkins EC2 server on AWS along with required tooling |
| `Kubernetes-Manifests-file/` | Kubernetes YAML manifests (Deployments, Services, Ingress, PV/PVC) used by ArgoCD to deploy the app on EKS |
| `assets/` | Architecture diagrams, screenshots, and reference images used in documentation |

---

## 🛠️ Tech Stack

**Cloud & Infra:** AWS (EC2, EKS, ECR, ALB, VPC, IAM), Terraform
**CI/CD:** Jenkins, GitHub Webhooks, ArgoCD (GitOps)
**Security (DevSecOps):** SonarQube (code quality/SAST), OWASP Dependency-Check (SCA), Trivy (container/image scanning)
**Containers & Orchestration:** Docker, Kubernetes (Amazon EKS), Helm
**Monitoring:** Prometheus, Grafana
**Application:** React.js (frontend), Node.js (backend), MongoDB (database)

---

## ✅ Prerequisites

- An AWS account with an IAM user having permissions for EC2, EKS, ECR, IAM, and VPC
- Terraform and AWS CLI installed locally
- A GitHub account/repository with webhook access
- Basic familiarity with Jenkins, Docker, and Kubernetes

---

## 🚀 Deployment Overview

1. **Provision the Jenkins server** — Use the Terraform code in `Jenkins-Server-TF/` to spin up an EC2 instance and install Jenkins, Docker, Terraform, kubectl, AWS CLI, SonarQube, and Trivy.
2. **Provision the EKS cluster** — Deploy an Amazon EKS cluster and configure an AWS Application Load Balancer for ingress.
3. **Set up Amazon ECR** — Create private repositories for the frontend and backend Docker images.
4. **Configure Jenkins** — Install required plugins (Docker, NodeJS, SonarQube Scanner, OWASP Dependency-Check) and set up credentials for AWS, GitHub, and SonarQube.
5. **Run the CI pipelines** — The Jenkinsfiles in `Jenkins-Pipeline-Code/` build the app, run SonarQube analysis, scan dependencies and images, then push Docker images to ECR.
6. **Install ArgoCD** — Deploy ArgoCD on the EKS cluster for GitOps-based continuous delivery.
7. **Deploy via ArgoCD** — Apply the manifests in `Kubernetes-Manifests-file/` (database → backend → frontend → ingress) through ArgoCD applications.
8. **Configure DNS** — Point a custom domain/subdomain to the ALB for external access.
9. **Enable monitoring** — Install Prometheus and Grafana via Helm to monitor cluster and application health.

> Detailed step-by-step commands for each stage can be added here as the project evolves.

---

## 🔐 DevSecOps Highlights

- **Shift-left security:** Code quality, dependency vulnerabilities, and image vulnerabilities are all checked *before* an image reaches the cluster.
- **GitOps delivery:** ArgoCD continuously reconciles the cluster state with the manifests in Git, rather than Jenkins pushing changes directly.
- **Observability:** Prometheus scrapes cluster/application metrics, visualized through Grafana dashboards.

---

## 📸 Screenshots / Diagrams

Reference architecture diagrams and screenshots are available in the [`assets/`](./assets) folder.

---

## 📄 License

This project is licensed under the Apache License 2.0.

---

## 🙋 About

Built as a hands-on DevOps/Cloud portfolio project covering CI/CD, Infrastructure as Code, container orchestration, and DevSecOps practices on AWS.
