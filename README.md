# Production-Grade Retail Microservices Platform on AWS EKS

End-to-end DevOps implementation of a 5-microservice retail application on AWS — from containerization to a fully automated, cost-optimized, observable GitOps deployment pipeline.

## 🧭 Overview

This project takes a retail store application through the complete production DevOps lifecycle on AWS:

**Docker → Kubernetes (EKS) → Terraform (IaC) → Ingress/DNS/TLS → Helm → Karpenter Autoscaling → Secrets Management → Observability → CI/CD with GitOps**

## 🏗️ Architecture

- **Application:** 5 microservices (retail store)
- **Container Registry:** Amazon ECR
- **Orchestration:** Amazon EKS (Kubernetes)
- **Infrastructure as Code:** Terraform (VPC, EKS, RDS, ElastiCache, DynamoDB, remote state)
- **Autoscaling:** Karpenter (On-Demand + Spot instances)
- **CI/CD:** GitHub Actions + ArgoCD (GitOps)
- **Observability:** OpenTelemetry, AWS X-Ray, CloudWatch, Prometheus, Grafana
- **Secrets:** AWS Secrets Manager, EKS Pod Identity, External Secrets Operator

## 🚀 Implementation Steps

### Phase 1 — Containerization
- Wrote multi-stage Dockerfiles for each of the 5 microservices
- Built multi-platform images (AMD64 + ARM64)
- Pushed images to Amazon ECR

### Phase 2 — Kubernetes Fundamentals
- Deployed services manually to learn core primitives: Deployments, ReplicaSets, Pods
- Configured all 5 Kubernetes Service types (ClusterIP, NodePort, LoadBalancer, ExternalName, Headless)
- Added liveness/readiness probes, resource requests/limits, labels/selectors/annotations
- Deployed stateful components with StatefulSets + AWS EBS CSI driver

### Phase 3 — Infrastructure as Code (Terraform)
- Provisioned VPC, subnets, and route tables
- Provisioned the EKS cluster, cluster IAM role, node group IAM role, and private node groups
- Configured Terraform remote state (S3 backend + state locking)
- Provisioned RDS, ElastiCache, and DynamoDB

### Phase 4 — Networking & Ingress
- Installed an Ingress Controller with AWS Load Balancer (ALB) integration
- Attached SSL/TLS certificates via ACM
- Automated DNS records with External DNS

### Phase 5 — Helm Packaging
- Converted raw manifests into Helm charts with templating
- Created environment-specific values files (dev/staging/prod)

### Phase 6 — Autoscaling & Cost Optimization
- Installed Karpenter for pod-driven node autoscaling
- Configured NodePools for On-Demand and Spot instances
- Reduced compute costs by ~70% using Spot instances
- Handled Spot interruptions with EventBridge → SQS → PodDisruptionBudgets for zero-downtime rescheduling
- Enabled node consolidation to remove underutilized nodes automatically

### Phase 7 — Secrets Management
- Stored sensitive config in AWS Secrets Manager
- Used EKS Pod Identity for secure, keyless IAM role assumption
- Synced secrets into Kubernetes via the External Secrets Operator

### Phase 8 — Observability
- Instrumented services with OpenTelemetry for distributed tracing
- Sent traces to AWS X-Ray
- Shipped logs to CloudWatch
- Built Prometheus + Grafana dashboards for metrics, latency, and error rates

### Phase 9 — CI/CD with GitOps
- Built a GitHub Actions workflow to build and push Docker images to ECR on every commit
- Authenticated GitHub Actions to AWS via OIDC (no static access keys)
- Applied semantic versioning via Git tags
- Installed ArgoCD on EKS
- Created ArgoCD Applications with Helm integration, auto-sync, and self-heal
- Verified the full pipeline: commit → build → push to ECR → Helm values updated → ArgoCD auto-deploys → tested rollback

### Phase 10 — Teardown
- Decommissioned ArgoCD apps, PVCs, and Helm releases
- Destroyed infrastructure with `terraform destroy` to avoid ongoing costs

## 🛠️ Tech Stack

`AWS EKS` `Docker` `Kubernetes` `Terraform` `Karpenter` `Helm` `GitHub Actions` `ArgoCD` `Prometheus` `Grafana` `OpenTelemetry` `AWS X-Ray` `AWS Secrets Manager` `AWS ALB` `Route 53`

## 📚 Credit

Built by following [StackSimplify's Ultimate DevOps Real-World Project](https://www.udemy.com/course/aws-eks-kubernetes-karpenter-devops-production/) course, with hands-on implementation and configuration done independently.
