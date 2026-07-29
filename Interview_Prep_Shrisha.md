# Interview Preparation — Shrisha K S

---

## 1. Resume Bullet Points

### Capgemini — Senior DevOps Engineer (Jul 2025 – Present)

**Tools:** Terraform, AWS (EKS, VPC, RDS, DynamoDB, ElastiCache, SQS, ECR, ALB, Route53, ACM, CloudWatch, X-Ray), Docker, Kubernetes, GitHub Actions, ArgoCD, Helm, ADOT (OpenTelemetry), Prometheus, Grafana, Karpenter

- Provisioned production-grade **AWS EKS** clusters, multi-AZ VPC with public/private subnets, IAM roles, and EKS add-ons (LBC, EBS CSI Driver, Pod Identity Agent) using **Terraform** modules with S3 remote state and DynamoDB state locking
- Deployed a **5-microservice retail e-commerce application** (Java Spring Boot, Go, Node.js) on EKS backed by AWS managed data services — RDS MySQL, RDS PostgreSQL, DynamoDB, ElastiCache Redis, and SQS — fully provisioned via Terraform
- Containerized microservices using **Docker** multi-stage and multi-platform builds (AMD64/ARM64) with BuildKit; published images to Amazon ECR and deployed via Helm Charts with environment-specific values for dev and production
- Built end-to-end **GitOps CI/CD pipeline**: GitHub Actions (OIDC — no access keys) → ECR → Helm values update → **ArgoCD** auto-sync with self-heal, enabling zero-touch deployments from code commit to production
- Configured **AWS Load Balancer Controller** with Kubernetes Ingress for HTTPS traffic using ACM SSL certificates; automated Route53 DNS record management using **External DNS** add-on
- Implemented **Karpenter** for node autoscaling with On-Demand and Spot NodePools; configured EventBridge → SQS → Karpenter Spot interruption handling with PodDisruptionBudgets for zero-downtime pod eviction
- Configured **Horizontal Pod Autoscaler (HPA)** with Metrics Server for CPU and memory-based pod scaling across all microservices, integrated with Karpenter for full cluster elasticity
- Deployed **AWS Distro for OpenTelemetry (ADOT)** with auto-instrumentation for Java Spring Boot and Node.js — exporting distributed traces to **AWS X-Ray**, logs to **CloudWatch**, and metrics to **Amazon Managed Prometheus (AMP)** with **Grafana** dashboards; reduced trace cost by 85% through health-check filtering
- Secured workloads using **EKS Pod Identity** (IRSA) with AWS Secrets Manager and Secrets Store CSI Driver (ASCP), mounting encrypted secrets as environment variables and files — eliminating all hardcoded credentials

---

### Infosys — AWS DevOps Engineer (Feb 2022 – Jul 2025)

**Tools:** Terraform, AWS (VPC, EC2, ALB, CLB, NLB, Auto Scaling, Route53, ACM, RDS, CloudWatch, SNS, CodePipeline, CodeBuild, S3), Bash, Python, GitLab CI/CD

- Designed and provisioned **AWS 3-tier VPC architecture** using Terraform — public/private subnets, NAT Gateways, Internet Gateway, route tables, and Security Groups across multiple Availability Zones
- Deployed and managed **AWS EC2** workloads with Application Load Balancer (ALB), configuring context-path based routing, host-header routing, and HTTP header/query string redirects for multi-application environments
- Implemented **AWS Auto Scaling Groups** using Launch Templates and Launch Configurations with ALB target group integration, ensuring high availability and automatic capacity adjustment based on demand
- Configured **AWS Network Load Balancer (NLB)** with TCP and TLS listeners for low-latency, high-throughput workloads, and managed **ACM SSL certificates** with Route53 DNS for secure custom domain routing
- Set up **Amazon CloudWatch Alarms** with SNS notifications for ALB, Auto Scaling, and EC2 metrics — enabling proactive monitoring, threshold alerting, and faster incident response
- Built reusable **Terraform modules** (local and public registry) with input variables, output values, for loops, meta-arguments (`count`, `for_each`), and remote state storage using **S3 backend and DynamoDB state locking**
- Automated infrastructure delivery using **AWS CodePipeline and CodeBuild** — implementing IaC DevOps pipelines to provision and update AWS resources on every code commit with full audit trail

---

## 2. Self Introduction

> "Hi, I'm Shrisha. I have around 4 years of experience in DevOps and cloud infrastructure on AWS.
>
> I started my career at Infosys as an AWS DevOps Engineer, where I worked on provisioning and managing AWS infrastructure using Terraform — building VPCs, EC2 workloads, load balancers, Auto Scaling groups, and CI/CD pipelines using AWS CodePipeline and CodeBuild. I also handled CloudWatch monitoring and production incident management.
>
> Currently I'm working at Capgemini as a Senior DevOps Engineer, where I focus on modern cloud-native infrastructure — provisioning AWS EKS clusters with Terraform, building GitOps CI/CD pipelines using GitHub Actions and ArgoCD, deploying microservices with Helm, setting up full observability using OpenTelemetry and AWS X-Ray, and implementing autoscaling with Karpenter.
>
> I also hold the AWS Certified Solutions Architect Associate certification."

---

## 3. Project Architecture

### Capgemini — Retail Store on AWS EKS

**When asked to explain the architecture:**

> "In my current project at Capgemini, I work on a Retail Store e-commerce platform built on AWS EKS.
>
> The application has 5 microservices — a UI service in Java Spring Boot, a Catalog service in Go backed by RDS MySQL, a Cart service in Spring Boot using DynamoDB, a Checkout service in Node.js using ElastiCache Redis, and an Orders service in Spring Boot backed by RDS PostgreSQL and SQS for messaging.
>
> On the infrastructure side, everything is provisioned using Terraform — VPC with public and private subnets across multiple Availability Zones, EKS cluster with managed node groups, and all AWS data services like RDS, DynamoDB, ElastiCache, and SQS.
>
> For CI/CD, we use GitHub Actions to build Docker images and push to ECR, then update Helm chart values, and ArgoCD picks up the change and deploys to EKS automatically — fully GitOps.
>
> For autoscaling, Karpenter handles node provisioning including Spot instances with interruption handling, and HPA handles pod scaling.
>
> For observability, we use AWS Distro for OpenTelemetry — traces go to X-Ray, logs to CloudWatch, and metrics to Amazon Managed Prometheus with Grafana dashboards.
>
> Traffic comes in through an AWS ALB with HTTPS using ACM certificates, and Route53 manages DNS automatically via External DNS."

**Architecture Diagram (Text):**

```
Internet → Route53 → AWS ALB (ACM/HTTPS)
                          ↓
                    EKS Cluster (Private Subnets)
         ┌────────────────────────────────────────┐
         │  UI (Spring Boot)                      │
         │  Catalog (Go)     → RDS MySQL          │
         │  Cart (Spring)    → DynamoDB           │
         │  Checkout (Node)  → ElastiCache Redis  │
         │  Orders (Spring)  → RDS PostgreSQL     │
         │                   → SQS               │
         └────────────────────────────────────────┘

CI/CD:         GitHub Actions → ECR → Helm → ArgoCD → EKS
Observability: ADOT → X-Ray | CloudWatch | Prometheus/Grafana
Autoscaling:   HPA (pods) + Karpenter (nodes + Spot)
Security:      EKS Pod Identity + Secrets Manager CSI Driver
IaC:           Terraform (VPC + EKS + all AWS services)
```

---

### Infosys — AWS 3-Tier Infrastructure

**When asked to explain the architecture:**

> "At Infosys, I worked on provisioning and managing AWS infrastructure for application workloads using Terraform.
>
> The architecture was a 3-tier VPC — public subnets for load balancers, private subnets for application EC2 instances, and private subnets for RDS databases. NAT Gateways handled outbound internet access from private subnets.
>
> For load balancing, we used Application Load Balancers with path-based and host-header based routing for multiple applications, and Network Load Balancers for TCP/TLS workloads requiring low latency.
>
> Auto Scaling Groups with Launch Templates handled EC2 scaling based on CloudWatch alarms, with SNS notifications for threshold breaches.
>
> For CI/CD, we used AWS CodePipeline and CodeBuild to automate Terraform deployments — so any infrastructure change went through a pipeline with plan, review, and apply stages.
>
> Route53 handled DNS, ACM managed SSL certificates, and CloudWatch provided monitoring across the entire stack."

**Architecture Diagram (Text):**

```
Internet → Route53 → ALB / NLB (Public Subnet)
                          ↓
                EC2 Auto Scaling Group (Private Subnet)
                          ↓
                    RDS Database (Private Subnet)

Monitoring:  CloudWatch Alarms → SNS notifications
CI/CD:       CodePipeline → CodeBuild → Terraform apply
IaC:         Terraform modules with S3 remote state + DynamoDB locking
SSL/DNS:     ACM certificates + Route53
```

---

## 4. Interview Tips

### Before the Interview

**Know your resume line by line**
Every bullet you wrote — be ready to explain *how* you did it, not just *that* you did it.
- "You mentioned Karpenter — walk me through Spot interruption handling"
- "How does ArgoCD self-heal work exactly?"
- "What happens if Terraform state gets locked?"

**Prepare 2 failure/challenge stories**
One for each company — format: *What happened → what you did → what you learned*
- Infosys: CloudWatch alarm misconfiguration / Terraform state issue / Auto Scaling not triggering
- Capgemini: Pod crash / ArgoCD sync failure / Karpenter not provisioning nodes

---

### During the Interview

**Never say "I don't know" and stop**
Say: *"I haven't worked on that directly, but based on how X works, I would approach it by..."*

**Clarify before answering**
If asked *"How do you handle secrets in Kubernetes?"* ask:
*"Are you asking about native Kubernetes secrets or integration with AWS Secrets Manager?"*
Shows depth and buys you thinking time.

**Use STAR format for scenario questions**
- **S**ituation — context
- **T**ask — your responsibility
- **A**ction — what you did
- **R**esult — outcome

---

### Top 5 Technical Topics That Always Come Up

| Topic | What to prepare |
|---|---|
| `kubectl` troubleshooting | `describe`, `logs`, `exec`, `top`, `get events` |
| Terraform state issues | state lock, `terraform state rm`, `import` |
| Docker image optimization | multi-stage builds, `.dockerignore` |
| Pod not starting | OOMKilled, CrashLoopBackOff, ImagePullBackOff |
| CI/CD security | OIDC vs access keys, secret scanning, least privilege |

---

### Know the "Why" Not Just the "What"

| Question | Answer |
|---|---|
| Why Karpenter over Cluster Autoscaler? | Faster (seconds vs minutes), pod-driven, Spot-native |
| Why ArgoCD over Jenkins CD? | GitOps, drift detection, declarative, audit trail in Git |
| Why Helm over plain kubectl? | Templating, versioning, rollback, environment management |
| Why OIDC over access keys in GitHub Actions? | No long-lived credentials, auto-rotated, more secure |
| Why remote state in Terraform? | Team collaboration, state locking, no local state loss |

---

### Salary & Offer

**Don't give a number first**
When asked expected CTC say:
*"I'm open to the right opportunity — what is the budgeted range for this role?"*

**Questions to ask at the end of the interview:**
- *"What does the on-call rotation look like for this team?"*
- *"What is the current biggest infrastructure challenge the team is solving?"*
- *"What does the tech stack look like 1-2 years from now?"*

---

### Mindset

After completing both courses hands-on, you will have:
- Built a production EKS cluster from scratch with Terraform
- Set up GitOps, observability, autoscaling, and secrets management
- Provisioned full AWS 3-tier infrastructure with CodePipeline CI/CD

**That is more than most candidates who claim 4+ years. Be confident.**
