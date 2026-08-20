# Accenture Interview Prep — DevOps Engineer (COMPLETE)
**Job No.:** ATCI-5548029-S2055951 | **Location:** Chennai | **Level:** Packaged Application Development Senior Analyst  
**Must Have:** AWS Containerization | **Experience Required:** 3–5 years

---

## 1. JD vs Resume — Skill Alignment

### 1.1 Skill-by-Skill Match

| JD Requirement | Shrisha's Exact Match | Strength |
|---|---|---|
| **AWS Containerization** | EKS (production clusters), ECR (OIDC-based push), Docker multi-stage + multi-platform builds, Helm | ✅ Strong — core skill |
| **CI/CD Pipelines** | GitHub Actions → ECR → ArgoCD GitOps (Capgemini); AWS CodePipeline + CodeBuild (Infosys) | ✅ Strong |
| **Linux + Config Management** | EC2 management, Bash scripting, user data, `systemctl`, log analysis; Terraform as IaC config management | ✅ Strong |
| **Git / Version Control** | GitHub (Capgemini — GitOps repo + app repos), GitLab CI/CD (Infosys) | ✅ Strong |
| **Kubernetes** | EKS, Helm charts, ArgoCD, Karpenter, HPA, Pod Identity, Ingress, NetworkPolicy, PVC | ✅ Strong |
| **HashiCorp Terraform** | Terraform modules, S3 remote state, DynamoDB state locking, `count`/`for_each`, `lifecycle` rules | ✅ Strong |
| **Grafana + Prometheus** | Amazon Managed Prometheus (AMP) + Grafana dashboards (cluster health, service latency, error rates) | ✅ Strong |
| **Monitoring / Observability** | ADOT, AWS X-Ray (traces), CloudWatch (logs), AMP + Grafana (metrics) — full 3-pillar stack | ✅ Strong |
| **Agile / DevOps culture** | GitOps mindset, blameless post-mortems, shift-left security, self-service pipelines | ✅ Strong |
| **HashiCorp Vault** | No direct Vault — used AWS Secrets Manager + Secrets Store CSI Driver (ASCP) — equivalent | ⚠️ Bridge needed |
| **Alert Manager** | No direct Alert Manager — used CloudWatch Alarms + SNS; Grafana alerting for app metrics | ⚠️ Bridge needed |
| **On-prem infrastructure** | AWS-only (no on-prem experience) — can bridge with general Linux + infra skills | ⚠️ Bridge needed |

**Overall Match: Very Strong — 9/12 direct matches. 3 gaps are bridgeable with AWS equivalents.**

---

### 1.2 How to Bridge the Gaps in the Interview

**HashiCorp Vault → Say this:**
> "I haven't used Vault directly, but I've implemented the same security model using AWS-native tools — AWS Secrets Manager for centralized storage and rotation, Secrets Store CSI Driver (ASCP) to mount secrets as pod volumes, and EKS Pod Identity for least-privilege IAM access per service account. The concepts are identical: centralized secret store, access policies, audit trail, automatic rotation. I'm confident I can pick up Vault quickly."

**Alert Manager → Say this:**
> "I used CloudWatch Alarms with SNS for infrastructure-level alerting and Grafana alerting for application-level thresholds. Alert Manager sits between Prometheus and notification channels — it does routing, deduplication, grouping, and silencing, which is what Grafana alerting also does. The configuration format is different but the problem it solves is the same. I can adapt quickly."

**On-prem → Say this:**
> "My experience has been on AWS, but the underlying skills — Linux administration, Terraform for infrastructure provisioning, Ansible-style configuration management, Docker, Kubernetes — apply equally to on-prem or hybrid environments. The network topology differs (no VPC, physical switches) but the tooling and mindset are the same."

---

### 1.3 Your Strongest Talking Points for This JD

| JD Priority | Your Evidence |
|---|---|
| AWS Containerization (must-have) | Deployed 5-microservice app on EKS — full stack from VPC to GitOps |
| CI/CD pipelines | GitHub Actions + ArgoCD — zero-touch code commit to production |
| Kubernetes | EKS + Karpenter + HPA + Pod Identity + Helm — production-grade |
| Terraform | Modules + remote state + DynamoDB locking — team-ready IaC |
| Monitoring | ADOT → X-Ray + CloudWatch + AMP + Grafana — full observability |
| Security | OIDC (no access keys), Pod Identity, Secrets Manager — no hardcoded credentials anywhere |
| Business impact | 85% trace cost reduction, 60–70% Spot savings, zero-touch deployments |

---

## 2. Self Introduction (Accenture-tailored)

### 2.1 Full Version (2–3 minutes)

> "Hi, I'm Shrisha. I have around 4 years of experience in DevOps and cloud infrastructure, with a strong focus on AWS containerization and Kubernetes.
>
> I started my career at Infosys as an AWS DevOps Engineer, where I designed and provisioned AWS infrastructure using Terraform — 3-tier VPCs, EC2 workloads with ALB and Auto Scaling, RDS, CloudWatch monitoring, and CI/CD pipelines using AWS CodePipeline and CodeBuild. That's where I built my foundation in IaC and cloud infrastructure management.
>
> Currently at Capgemini as a Senior DevOps Engineer, I work on production-grade AWS containerization. I provision EKS clusters with Terraform, build GitOps CI/CD pipelines using GitHub Actions and ArgoCD, and deploy a 5-microservice retail application — Java Spring Boot, Go, and Node.js — using Helm. I manage autoscaling with Karpenter and HPA, handle Spot instance interruptions automatically via EventBridge and SQS, and run full observability using OpenTelemetry, AWS X-Ray, Amazon Managed Prometheus, and Grafana.
>
> On the security side, I use OIDC in GitHub Actions — no static AWS credentials anywhere — and EKS Pod Identity with Secrets Manager to eliminate all hardcoded secrets.
>
> Some outcomes I'm proud of: I reduced our observability costs by 85% through trace filtering in ADOT, and we save 60 to 70% on compute costs using Karpenter Spot instances.
>
> I also hold the AWS Certified Solutions Architect Associate certification.
>
> This role at Accenture is a great fit — the core requirements of AWS containerization, Kubernetes, Terraform, CI/CD pipelines, and monitoring are exactly what I do every day. I'm excited about the opportunity to bring that experience to Accenture's clients."

---

### 2.2 Short Version (under 1 minute — if asked "tell me briefly about yourself")

> "I'm Shrisha, a Senior DevOps Engineer with 4 years of experience. I started at Infosys working on AWS infrastructure with Terraform, and I'm currently at Capgemini focused on AWS containerization — EKS, Helm, GitOps with ArgoCD, and full observability with Prometheus and Grafana. I've reduced observability costs by 85% and use Karpenter Spot instances for 60–70% compute savings. I'm AWS certified and very hands-on with the exact stack this role needs."

---

### 2.3 If Asked "Why Accenture?"

> "Accenture works at a scale and client variety that I find exciting — the opportunity to apply the same containerization and DevOps practices I've built at Capgemini to multiple clients and domains is something I'm genuinely looking forward to. I also want to expand my exposure to hybrid and on-prem infrastructure, which I know Accenture works with extensively. It feels like the right next step."

---

### 2.4 If Asked "Why are you looking to move?"

> "I've had a great experience at Capgemini and learned a lot. I feel I've built solid depth in AWS containerization and GitOps, and I'm now looking for a role where I can apply that at a larger scale, work with a wider variety of clients, and continue growing — ideally into more architecture and platform-level work. This role at Accenture offers that."

---

## 3. AWS Containerization — Q&A (Must Have Skill)

---

**Q: What does AWS containerization mean to you? What container services have you worked with?**

> AWS containerization means packaging application code and its runtime dependencies into portable, isolated containers and running them reliably on AWS-managed infrastructure — so the same image works in dev, staging, and production without environment differences.
>
> Full AWS container stack I've worked with:
> - **Docker** — multi-stage, multi-platform builds (AMD64/ARM64) using BuildKit
> - **Amazon ECR** — private image registry with OIDC-based push from GitHub Actions (no static access keys)
> - **Amazon EKS** — managed Kubernetes with managed node groups + Karpenter for autoscaling, ArgoCD for GitOps
> - **Helm** — Kubernetes package manager for environment-specific deployments
>
> At Capgemini I containerized a 5-microservice retail application — Java Spring Boot, Go, Node.js — and deployed it on EKS using Helm charts with full GitOps.

---

**Q: What is EKS? How is it different from self-managed Kubernetes?**

> EKS is AWS's managed Kubernetes control plane. AWS manages the API server, etcd, scheduler, and controller manager — across multiple AZs, with automatic upgrades, patching, and HA built in.

| Aspect | EKS | Self-Managed |
|---|---|---|
| Control plane | Managed by AWS — you cannot SSH into master nodes | You manage and patch master nodes yourself |
| High Availability | Multi-AZ built-in | You configure it manually |
| IAM integration | Pod Identity, IRSA — native AWS auth per pod | Manual webhook/OIDC setup |
| Add-ons | EBS CSI Driver, LBC, CoreDNS as managed add-ons | Manual install and upgrade |
| Cost | ~$0.10/hr for control plane | EC2 cost for master nodes |

> The trade-off is the control plane fee — but for production, the operational simplicity is worth it. You never touch master nodes and AWS handles HA automatically.

---

**Q: Walk me through your complete EKS cluster setup.**

> **Infrastructure — Terraform:**
> - Multi-AZ VPC: public subnets (ALB), private subnets (EKS nodes, RDS, ElastiCache, SQS)
> - EKS cluster + managed node groups for system workloads (CoreDNS, Karpenter, ArgoCD)
> - EKS add-ons via Terraform: EBS CSI Driver, Pod Identity Agent, VPC CNI, CoreDNS, kube-proxy
> - Karpenter NodePools for application workload provisioning (On-Demand + Spot)
> - S3 remote state + DynamoDB locking for team-safe Terraform
>
> **Security:**
> - EKS Pod Identity per service account — least-privilege IAM per microservice
> - Secrets Manager + Secrets Store CSI Driver (ASCP) — no hardcoded credentials
> - ECR image scanning on push — block CRITICAL CVEs in CI
> - Non-root containers, private subnets, ALB-only ingress
>
> **CI/CD:**
> - GitHub Actions → ECR → Helm values update → ArgoCD auto-sync (zero-touch)
>
> **Observability:**
> - ADOT → X-Ray (traces), CloudWatch (logs), AMP + Grafana (metrics)

---

**Q: What IAM roles are required for EKS to work?**

| Role | Policy | Purpose |
|---|---|---|
| EKS Cluster Role | `AmazonEKSClusterPolicy` | Control plane manages AWS resources (VPC, ENIs) |
| Node Group Role | `AmazonEKSWorkerNodePolicy` | Worker nodes join and register with the cluster |
| Node Group Role | `AmazonEC2ContainerRegistryReadOnly` | Nodes pull images from ECR |
| Node Group Role | `AmazonEKS_CNI_Policy` | VPC CNI plugin manages pod-level ENIs |
| Pod Identity Role | Custom least-privilege | Per-service-account AWS access (Secrets Manager, S3, DynamoDB) |

---

**Q: How do you configure kubectl to connect to your EKS cluster?**

```bash
aws eks update-kubeconfig --region us-east-1 --name my-cluster
```

> This updates `~/.kube/config` with the cluster endpoint, CA certificate, and auth token command.
> Authentication uses `aws eks get-token` under the hood — no static credentials in kubeconfig.
> IAM permissions control who can run kubectl commands — mapped via `aws-auth` ConfigMap or EKS access entries.

---

**Q: What is EKS Pod Identity? How is it different from IRSA?**

| Feature | IRSA (legacy) | Pod Identity (recommended) |
|---|---|---|
| Setup | Annotate service account + configure OIDC provider | Pod Identity Agent DaemonSet handles the token exchange |
| Token delivery | OIDC JWT projected as a volume mount into the pod | Agent intercepts and exchanges tokens transparently |
| AWS recommendation | Legacy — still works | Yes — preferred for new EKS clusters |
| Cross-account | Complex trust policy setup | Simpler association API |

> At Capgemini I used **Pod Identity**. Each microservice (Cart, Catalog, Orders, etc.) has its own Kubernetes service account, each associated with a dedicated IAM role — so the Cart service can only access DynamoDB, Orders can only access RDS PostgreSQL and SQS. No shared node-level IAM roles with broad permissions.

---

**Q: What is Karpenter and how does it differ from Cluster Autoscaler?**

| Feature | Cluster Autoscaler | Karpenter |
|---|---|---|
| How it provisions | Scales existing ASG node groups | Calls EC2 API directly — any instance type/family |
| Speed | 3–5 minutes for a new node | Under 60 seconds |
| Flexibility | Limited to pre-defined node group instance types | Dynamically picks the best fit instance type for pending pods |
| Bin-packing | Limited | Efficient consolidation — removes underutilized nodes |
| Spot handling | Basic | Full interruption handling via EventBridge → SQS |

---

**Q: Walk me through how Karpenter handles Spot interruptions.**

> 1. AWS sends a **2-minute Spot interruption warning** → **EventBridge** rule captures it
> 2. EventBridge sends event to a dedicated **SQS queue**
> 3. **Karpenter** polls SQS and receives the interruption notice
> 4. Karpenter **cordons** the node — no new pods scheduled on it
> 5. Karpenter **drains** the node gracefully — evicts pods respecting **PodDisruptionBudget**
> 6. Pods are rescheduled on available nodes or Karpenter provisions a new On-Demand node
> 7. The Spot instance is terminated — zero application downtime
>
> This is why I configured PodDisruptionBudget (`minAvailable: 2`) for every microservice — Karpenter cannot evict pods if it would violate the PDB.

---

**Q: What is a NodePool in Karpenter? What does it define?**

```yaml
apiVersion: karpenter.sh/v1beta1
kind: NodePool
spec:
  template:
    spec:
      requirements:
        - key: karpenter.sh/capacity-type
          operator: In
          values: ["spot", "on-demand"]
        - key: node.kubernetes.io/instance-type
          operator: In
          values: ["m5.large", "m5.xlarge", "m4.large", "m5.2xlarge"]
  limits:
    cpu: "1000"
  disruption:
    consolidationPolicy: WhenUnderutilized
    consolidateAfter: 30s
```

> - Defines what kind of nodes Karpenter can provision — instance types, capacity types (Spot/On-Demand), AZs
> - `EC2NodeClass` defines the AMI, subnets, security groups, and instance profile
> - `limits.cpu` caps total CPU Karpenter can provision — prevents unbounded scaling
> - `consolidationPolicy: WhenUnderutilized` — Karpenter removes underutilized nodes automatically (cost saving)

---

**Q: How do HPA and Karpenter work together?**

```
High traffic arrives
  → ALB distributes to running pods
  → CPU rises above HPA threshold (e.g., 70%)
  → HPA adds pod replicas
  → New pods go Pending (no node has capacity)
  → Karpenter sees Pending pods
  → Karpenter provisions exactly-right EC2 node in < 60 seconds
  → Pods schedule and serve traffic

Traffic drops
  → HPA scales pods down
  → Nodes become underutilized
  → Karpenter consolidates — terminates underutilized nodes
  → Cost goes down automatically
```

> HPA scales **pods**. Karpenter scales **nodes**. Together they give full cluster elasticity with no manual intervention.

---

**Q: How does the AWS Load Balancer Controller work with Kubernetes Ingress?**

> The LBC controller watches for `Ingress` resources with the ALB annotation. When it sees one, it automatically provisions an ALB in AWS, configures listeners, path-based routing rules, and target groups — all from the Kubernetes Ingress spec.
>
> **IP mode vs Instance mode:**
> - **IP mode** (preferred): routes directly to pod IPs — lower latency, no double-hop via NodePort
> - **Instance mode**: routes to node NodePort — simpler but adds a network hop

```yaml
annotations:
  kubernetes.io/ingress.class: alb
  alb.ingress.kubernetes.io/scheme: internet-facing
  alb.ingress.kubernetes.io/target-type: ip
  alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:123:certificate/abc
  alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
  alb.ingress.kubernetes.io/ssl-redirect: '443'
```

> The ALB handles HTTPS termination using the ACM certificate — pods receive plain HTTP. Route53 DNS is managed automatically by External DNS.

---

**Q: What is External DNS and how did you use it?**

> External DNS is a Kubernetes controller that watches Ingress and Service resources and automatically manages DNS records in Route53.
>
> - When an ALB Ingress is created with a hostname annotation, External DNS creates a Route53 A record pointing to the ALB DNS name
> - When the Ingress is deleted, External DNS removes the DNS record
> - Eliminates all manual DNS management — fully automated
> - Requires IAM permission: `route53:ChangeResourceRecordSets`, `route53:ListHostedZones`, `route53:ListResourceRecordSets`
>
> In my project, I gave External DNS its own IAM role via Pod Identity — scoped only to our hosted zone.

---

**Q: How do you ensure zero-downtime deployments on EKS?**

> Multiple layers working together:
>
> ```yaml
> strategy:
>   type: RollingUpdate
>   rollingUpdate:
>     maxUnavailable: 0   # never reduce below desired count
>     maxSurge: 1         # allow one extra pod during rollout
> ```
>
> - **`readinessProbe`** — new pod only receives traffic once the probe passes; unhealthy pods never get traffic
> - **`PodDisruptionBudget`** — `minAvailable: 2` prevents too many pods being down simultaneously during node drain or rollout
> - **Pre-upgrade Helm hooks** — DB migrations run as a Kubernetes Job before pods roll out
> - **ArgoCD progressive sync** — can pause between rollout stages and check health before continuing
> - **Feature flags** — decouple deployment from feature release for risky changes

---

**Q: How do you secure containers on EKS?**

| Layer | What I Did |
|---|---|
| **Image** | Multi-stage builds (no build tools in runtime), Alpine base, ECR scan on push, Trivy in CI |
| **Pod** | Non-root USER, `readOnlyRootFilesystem: true`, `allowPrivilegeEscalation: false` |
| **Secrets** | Pod Identity + Secrets Manager + ASCP CSI Driver — no secrets in env vars or code |
| **Network** | Private subnets for pods, Security Groups for Pods, ALB-only public ingress |
| **IAM** | Pod Identity per service account — least-privilege, scoped to specific resources |
| **CI/CD** | OIDC in GitHub Actions — no static AWS access keys ever stored anywhere |
| **Audit** | CloudTrail logs every API call; Secrets Manager logs every `GetSecretValue` |

---

**Q: What EKS add-ons did you install and what does each do?**

| Add-On | Purpose |
|---|---|
| `aws-ebs-csi-driver` | Dynamic EBS volume provisioning for PersistentVolumeClaims |
| `eks-pod-identity-agent` | Runs on every node; handles Pod Identity token exchange with STS |
| `aws-load-balancer-controller` | Provisions ALB/NLB from Kubernetes Ingress/Service resources |
| `secrets-store-csi-driver` + ASCP | Mounts AWS Secrets Manager secrets as pod files/env vars |
| `external-dns` | Syncs Ingress hostnames to Route53 DNS records automatically |
| `vpc-cni` | Assigns VPC IPs directly to pods (native VPC networking) |
| `coredns` | Internal DNS for service discovery inside the cluster |
| `kube-proxy` | Manages iptables rules for Kubernetes Service routing |

---

**Q: How do you upgrade EKS from version 1.28 to 1.30?**

> EKS supports only **one minor version at a time** — you must do 1.28 → 1.29 → 1.30.
>
> **Process per version step:**
> 1. Review EKS release notes — breaking API changes between versions
> 2. Check add-on compatibility matrix — upgrade add-ons to versions compatible with new K8s version
> 3. Raise a Normal CR in ServiceNow (planned change, CAB approval)
> 4. Update `cluster_version` in Terraform → `terraform apply` → AWS upgrades control plane (~15–20 min, zero downtime — AWS manages master nodes, you never touch them)
> 5. Update node group AMI release version in Terraform → rolling node replacement (old nodes drained, new nodes join)
> 6. Verify: `kubectl version`, `kubectl get nodes`, all pods running, app accessible
> 7. Repeat for next minor version
>
> **Key rule:** AWS manages master nodes completely — you never SSH into or patch control plane nodes.

---

**Q: What is kubectl cordon and kubectl drain? When do you use each?**

> - `kubectl cordon <node>` — marks node as **unschedulable**. New pods won't land on it. Existing pods are untouched.
> - `kubectl drain <node>` — **evicts all pods** from the node gracefully (respects PodDisruptionBudget), then cordons it.
>
> Used before: patching a node OS, manually replacing a node, decommissioning capacity.
>
> ```bash
> kubectl cordon ip-10-0-1-100.ec2.internal
> kubectl drain ip-10-0-1-100.ec2.internal --ignore-daemonsets --delete-emptydir-data
> ```
>
> Karpenter does cordon + drain automatically on Spot interruption or consolidation — you don't need to do it manually for autoscaling events.

---

**Q: Scenario — A new Karpenter node provisioned but pod is still Pending. What do you check?**

> 1. `kubectl describe pod <pod>` — read the **Events** section — exact failure reason is there
> 2. Common causes:
>
> | Cause | How to Diagnose |
> |---|---|
> | Taints/tolerations mismatch | Node has taint pod doesn't tolerate — check `kubectl describe node` |
> | Node selector / affinity | Pod requires labels node doesn't have |
> | Resource request too high | Pod requests 8Gi but node only has 6Gi allocatable |
> | PVC not bound | Pod waiting on PVC in Pending state — `kubectl get pvc` |
> | Image pull failure | ECR auth issue — check node IAM role has `ecr:GetAuthorizationToken` |
>
> 3. `kubectl get nodeclaim -A` — check Karpenter NodeClaim status to see if provisioning succeeded

---

**Q: Scenario — You tried to scale up nodes and it's failing. How do you troubleshoot?**

```bash
kubectl get nodes                                       # node status
kubectl describe node <node-name>                      # events and conditions
kubectl get events -A --sort-by='.lastTimestamp'       # cluster-wide events

# Check EC2 bootstrap log via SSM:
aws ssm start-session --target i-xxxxxxxx
sudo cat /var/log/cloud-init-output.log
```

| Symptom | Likely Cause |
|---|---|
| EC2 not launching | Spot unavailable / ASG at max — check ASG Activity tab |
| Nodes `NotReady` | kubelet startup issue — check `cloud-init-output.log` |
| Nodes not joining cluster | IAM role missing policy — check node group role has all 4 required policies |
| Subnet IP exhaustion | Too many pods, `/24` subnet too small — check available IPs in VPC console |
| Security group blocking | Nodes can't reach API server — SG must allow port 443 from node SG to cluster SG |

---

## 4. Docker — Q&A

---

**Q: What is a Docker image vs a Docker container?**

> - **Image** — immutable, read-only template built layer by layer from a Dockerfile; stored in a registry (ECR, DockerHub). Cannot change once built.
> - **Container** — a running instance of an image; has a thin writable layer on top; isolated process on the host OS sharing the kernel.
>
> Analogy: image = class definition, container = object instance.

---

**Q: What is containerization vs virtualisation?**

| Feature | Container | Virtual Machine |
|---|---|---|
| Startup time | Seconds | Minutes |
| Size | MBs | GBs |
| Isolation | Process-level — shares host OS kernel | Full OS-level — guest OS per VM |
| Overhead | Very low | High (hypervisor + full OS) |
| Portability | High — same image runs anywhere | Medium — tied to hypervisor type |
| Use case | Microservices, CI/CD, cloud-native apps | Full OS isolation, legacy apps |

---

**Q: What is a multi-stage Docker build? Why did you use it?**

```dockerfile
# Stage 1 — build (large, contains JDK + Maven + source)
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline          # cache dependencies as a separate layer
COPY src ./src
RUN mvn package -DskipTests

# Stage 2 — runtime only (small, contains only JRE + JAR)
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/app.jar .
USER 1001                               # non-root user
ENTRYPOINT ["java", "-jar", "app.jar"]
```

> - Stage 1 has JDK, Maven, source code — ~800MB
> - Stage 2 only has JRE + the compiled JAR — ~180MB
> - Build tools, source code, Maven cache are **never in the final image**
> - Result: smaller image, smaller attack surface, faster ECR pull, lower storage cost

---

**Q: What is CMD vs ENTRYPOINT? What happens when both are set?**

> - `ENTRYPOINT` — the **fixed executable**; always runs; arguments from `docker run` are **appended** to it
> - `CMD` — **default arguments**; easily overridden at `docker run`; when both are set, CMD becomes the default args to ENTRYPOINT

```dockerfile
ENTRYPOINT ["java", "-jar", "app.jar"]
CMD ["--spring.profiles.active=prod"]
```

> - `docker run myimage` → runs `java -jar app.jar --spring.profiles.active=prod`
> - `docker run myimage --spring.profiles.active=dev` → CMD is overridden → runs `java -jar app.jar --spring.profiles.active=dev`
> - ENTRYPOINT is **never** replaced by `docker run` arguments (unless `--entrypoint` flag is used)

**Tricky question — what does this Dockerfile output?**
```dockerfile
FROM ubuntu
ENTRYPOINT ["ls"]
CMD ["echo", "test"]
```
> Runs `ls echo test` — lists files named `echo` and `test`, which don't exist → error: `No such file or directory`. CMD here is not a command — it's arguments to `ls`.

---

**Q: What security best practices did you apply in your Dockerfiles?**

| Practice | Why |
|---|---|
| `USER 1001` (non-root) | Prevents container escape via privilege escalation |
| Alpine / distroless base | Smaller attack surface — fewer packages, fewer CVEs |
| Multi-stage builds | Build tools (`mvn`, `gcc`, `npm`) never appear in runtime image |
| `.dockerignore` | Prevents `.git`, `.env`, `node_modules`, test files entering build context |
| `HEALTHCHECK` | Kubernetes/Docker can detect unhealthy containers |
| Pinned base image versions | No surprise breaking changes from `:latest` |
| `readOnlyRootFilesystem: true` | Set in K8s pod spec — container cannot write to its own filesystem |
| `allowPrivilegeEscalation: false` | Prevents processes gaining more privileges than the container started with |

---

**Q: What is HEALTHCHECK in Dockerfile? How does it interact with Kubernetes probes?**

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1
```

> - `HEALTHCHECK` is **Docker-native** — used by Docker Engine and `docker-compose`
> - **Kubernetes does NOT use Docker's HEALTHCHECK** — it has its own probes defined in pod spec

| Kubernetes Probe | Purpose | Failure Action |
|---|---|---|
| `livenessProbe` | Is the container alive? | Kubernetes **restarts** the container |
| `readinessProbe` | Is the container ready to serve traffic? | Pod **removed from Service endpoints** — no traffic sent; NOT restarted |
| `startupProbe` | For slow-starting apps — gives time before liveness kicks in | Kubernetes restarts if startup probe never passes |

> Best practice: define all three in pod spec. `startupProbe` with high `failureThreshold` for slow Spring Boot apps, then `livenessProbe` + `readinessProbe` take over.

---

**Q: What is Docker BuildKit and why should you use it over the classic builder?**

> BuildKit is Docker's next-generation build engine — enabled by default in Docker 23+.

| Feature | Classic Builder | BuildKit |
|---|---|---|
| Stage execution | Sequential | Parallel — independent stages build simultaneously |
| Cache | Layer-level only | Granular — can cache specific mounts |
| Secret mounting | Not supported | `RUN --mount=type=secret` — secrets at build time, not in layers |
| Package cache | Re-downloaded every build | `RUN --mount=type=cache` — caches `~/.m2`, `node_modules` across builds |
| Multi-platform | Not supported natively | `docker buildx` — build AMD64 + ARM64 in one command |

```dockerfile
# Cache Maven dependencies across builds — not re-downloaded every time
RUN --mount=type=cache,target=/root/.m2 mvn package -DskipTests

# Mount a secret at build time without it appearing in any layer
RUN --mount=type=secret,id=npm_token npm install
```

---

**Q: How did you build multi-platform images? Why does it matter?**

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t 123456789.dkr.ecr.us-east-1.amazonaws.com/catalog:v1.2.3 \
  --push .
```

> - EKS Graviton nodes (ARM64) are **40% cheaper** than x86 EC2 — same image runs on both without rebuilding
> - Mac M-series developers (ARM64) can run the same image locally that runs in production (x86)
> - One ECR push — Docker image manifest contains layers for both architectures; Kubernetes pulls the right one automatically

---

**Q: What is a distroless image? When would you use it?**

> Distroless images (by Google) contain only the application runtime and its direct dependencies — **no shell, no package manager, no Linux utilities**.
>
> - Drastically smaller attack surface — no `bash`, `sh`, `curl`, `apt` for attackers to exploit
> - Significantly fewer CVEs vs even Alpine
> - Trade-off: **cannot exec into the container** (no shell) — use an ephemeral debug container in Kubernetes:

```bash
kubectl debug -it <pod> --image=busybox --target=<container>
```

---

**Q: What is the difference between `COPY` and `ADD` in a Dockerfile?**

> - `COPY` — copies files/directories from build context into image. Simple, predictable. **Always prefer this.**
> - `ADD` — same as COPY but also: auto-extracts `.tar.gz` archives, can fetch from URLs
> - Best practice: use `COPY` always unless you specifically need tar extraction or URL fetching

---

**Q: What is `.dockerignore` and why is it important?**

> `.dockerignore` excludes files and directories from the Docker build context before it's sent to the Docker daemon.
>
> Without it, every file in the directory is sent — including `.git`, `node_modules`, `.env`, test files, and credentials.
>
> ```
> .git
> node_modules
> .env
> *.pem
> target/
> **/*.test.js
> ```
>
> Benefits: smaller build context (faster builds), prevents accidental inclusion of secrets or unnecessary files in the image.

---

**Q: What is the difference between `docker stop` and `docker kill`?**

> - `docker stop` — sends **SIGTERM** first (graceful shutdown signal), waits 10 seconds, then sends SIGKILL
> - `docker kill` — sends **SIGKILL** immediately — forceful termination, no cleanup, no in-flight request completion
> - Always prefer `docker stop` in production — gives the app time to drain connections and shut down cleanly

---

**Q: How do you reduce Docker image size?**

> 1. **Multi-stage build** — discard build tools; only copy the final artifact
> 2. **Smaller base image** — Alpine (~5MB) or distroless vs Ubuntu (~70MB) vs full JDK (~500MB)
> 3. **Fewer layers** — chain `RUN` commands with `&&`:
>    ```dockerfile
>    RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
>    ```
> 4. **`.dockerignore`** — exclude `node_modules`, `.git`, test files from build context
> 5. **BuildKit cache mounts** — don't copy package manager caches into the image
> 6. **Pin specific versions** — `FROM eclipse-temurin:17-jre-alpine` not `FROM java:latest`

---

**Q: What is Docker Compose and how is it different from Kubernetes?**

| Feature | Docker Compose | Kubernetes |
|---|---|---|
| Scope | Single machine (local dev) | Cluster of machines (production) |
| Orchestration | Basic (start/stop/restart) | Full (scheduling, scaling, self-healing, rolling updates) |
| Networking | Auto bridge network per compose file | CNI-based pod networking, Services, Ingress |
| Scaling | `deploy.replicas` (limited) | HPA, Karpenter — auto-scaling |
| Use case | Local development, integration testing | Production workloads |

> I used Docker Compose to run the 5-microservice retail app locally during development — with health checks, named volumes for MySQL and Redis, and profiles to selectively start monitoring (`docker compose --profile monitoring up`). In production, everything runs on EKS.

---

**Q: How do you scan a Docker image for vulnerabilities?**

> **In CI (GitHub Actions):**
> ```yaml
> - name: Scan image with Trivy
>   uses: aquasecurity/trivy-action@master
>   with:
>     image-ref: '123456789.dkr.ecr.us-east-1.amazonaws.com/catalog:${{ github.sha }}'
>     severity: 'CRITICAL,HIGH'
>     exit-code: '1'   # fail the build if CRITICAL CVEs found
> ```
>
> **ECR native scanning:**
> - Enable ECR enhanced scanning (uses AWS Inspector) — scans on push and continuously
> - Results visible in ECR console and via EventBridge for automation
>
> **Locally:**
> ```bash
> trivy image myapp:latest
> ```

---

## 5. CI/CD Pipelines — Q&A

---

**Q: What is the difference between CI and CD?**

> - **CI (Continuous Integration)** — automatically build, test, lint, and validate code on every commit or PR. Goal: catch bugs early, before merge.
> - **CD (Continuous Delivery)** — automatically deliver the validated build to staging; production deployment requires a manual approval gate.
> - **CD (Continuous Deployment)** — fully automated push to production on every successful build — no manual gate.
>
> In my project: **CI = GitHub Actions** (build + test + image scan); **CD = ArgoCD** (GitOps sync to EKS — auto in dev/staging, manual approval gate before prod).

---

**Q: Design a full CI/CD pipeline for containerized microservices on AWS.**

```
Developer pushes code / opens PR
  ↓
GitHub Actions — CI Workflow:
  ├── Checkout code
  ├── Run unit tests + lint
  ├── Build Docker image (multi-stage, BuildKit)
  ├── Trivy scan — fail if CRITICAL CVEs found
  ├── Authenticate to AWS via OIDC (no static access keys)
  ├── Push image to Amazon ECR with :commit-sha tag
  └── Update Helm chart image.tag in GitOps repo (git commit)
  ↓
ArgoCD — CD (GitOps):
  ├── Detects git change (webhook or 3-min poll)
  ├── Compares desired state (git) vs live cluster state
  ├── Applies diff → rolling update in EKS
  ├── Self-heals if manual changes detected
  └── Sends Slack notification — success / failure
  ↓
EKS → ALB → Route53 → End User
```

> For Infosys: **AWS CodePipeline → CodeBuild** for Terraform IaC changes — every commit to the IaC repo triggers a plan + apply pipeline.

---

**Q: How did you avoid storing AWS credentials in GitHub Actions? Explain OIDC.**

> GitHub Actions supports **OIDC (OpenID Connect)** — zero static access keys needed anywhere.
>
> **How it works:**
> 1. GitHub Actions requests a short-lived JWT token from **GitHub's OIDC provider** (`token.actions.githubusercontent.com`)
> 2. The workflow calls `aws-actions/configure-aws-credentials` — this sends the JWT to **AWS STS**
> 3. STS validates the JWT against the trusted OIDC provider and calls `AssumeRoleWithWebIdentity`
> 4. STS returns **temporary credentials** valid only for that workflow run (auto-expire)
>
> **IAM Trust Policy (restricts to specific repo + branch):**
> ```json
> {
>   "Effect": "Allow",
>   "Principal": {
>     "Federated": "arn:aws:iam::123456789:oidc-provider/token.actions.githubusercontent.com"
>   },
>   "Action": "sts:AssumeRoleWithWebIdentity",
>   "Condition": {
>     "StringEquals": {
>       "token.actions.githubusercontent.com:sub": "repo:myorg/myrepo:ref:refs/heads/main",
>       "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
>     }
>   }
> }
> ```
>
> **GitHub Actions workflow snippet:**
> ```yaml
> permissions:
>   id-token: write   # required to request the JWT
>   contents: read
>
> steps:
>   - uses: aws-actions/configure-aws-credentials@v4
>     with:
>       role-to-assume: arn:aws:iam::123456789:role/github-actions-role
>       aws-region: us-east-1
> ```
>
> Result: **no AWS access keys ever stored** in GitHub secrets, repo, or environment. Credentials expire after each run.

---

**Q: What GitHub Actions do you use in your project?**

| Action | Purpose |
|---|---|
| `actions/checkout@v4` | Clone the repo into the runner |
| `actions/setup-java@v4` | Set up Java runtime for build |
| `docker/setup-buildx-action@v3` | Enable BuildKit for multi-platform builds |
| `docker/build-push-action@v5` | Build and push Docker image to ECR |
| `aws-actions/configure-aws-credentials@v4` | Authenticate to AWS via OIDC |
| `aws-actions/amazon-ecr-login@v2` | Login to Amazon ECR |
| `aquasecurity/trivy-action` | Scan image for CVEs — fail on CRITICAL |
| `hashicorp/setup-terraform@v3` | Install Terraform CLI for IaC pipeline |
| `actions/cache@v4` | Cache Maven `.m2` / npm `node_modules` — faster builds |
| `slackapi/slack-github-action@v1` | Slack notification on deploy success/failure |

---

**Q: What is GitOps? How does ArgoCD implement it?**

> GitOps: Git is the **single source of truth** for desired cluster state. No `kubectl apply` in CI — CI only writes to git; ArgoCD reads from git and applies.

| GitOps Principle | ArgoCD Implementation |
|---|---|
| Declarative | Helm charts + values files stored in git |
| Versioned | Every deployment = a git commit with author + timestamp |
| Automated | Auto-sync when git changes detected |
| Reconciled | Self-heal — reverts manual `kubectl` changes back to git state |

> **Security benefit**: ArgoCD running inside the cluster pulls from git. No external system needs `kubectl` access or AWS credentials.

---

**Q: What is ArgoCD's sync policy? Difference between manual and auto sync?**

| Setting | Behaviour |
|---|---|
| **Manual sync** | ArgoCD shows drift but waits for a human to click Sync in the UI |
| **Auto sync** | ArgoCD syncs automatically when a git change is detected |
| **Self-heal** | If someone manually `kubectl apply`s a change, ArgoCD reverts it to git state |
| **Prune** | Deletes Kubernetes resources that were removed from git — **disabled by default** (dangerous if misconfigured) |

> In my project: dev and staging use **auto sync + self-heal**. Production uses **auto sync + self-heal** but guarded by a GitHub Actions manual approval gate before the image tag is committed to the GitOps repo.

---

**Q: How do you do a rollback in your GitOps setup?**

> **Option 1 — Git revert (cleanest, full audit trail):**
> ```bash
> git revert <commit-sha>   # creates a new commit that undoes the image tag change
> git push
> # ArgoCD detects the new commit → auto-syncs → deploys the previous image
> ```
>
> **Option 2 — Helm rollback:**
> ```bash
> helm history retail-ui              # list all revisions
> helm rollback retail-ui 3           # revert to revision 3
> ```
>
> **Option 3 — Kubernetes rollback:**
> ```bash
> kubectl rollout undo deployment/retail-ui
> kubectl rollout undo deployment/retail-ui --to-revision=2
> kubectl rollout status deployment/retail-ui   # watch progress
> ```
>
> **Option 4 — ArgoCD UI rollback:**
> Select a previous git commit hash in the ArgoCD UI → sync to that specific revision.
>
> Git revert is preferred — it maintains the audit trail and keeps git as the source of truth.

---

**Q: The CI/CD pipeline passed but the app is down. Where do you start?**

> Pipeline passing = build and deploy **succeeded**. It does NOT guarantee the app is healthy at runtime.
>
> **Investigation order:**
> 1. `kubectl get pods -n <namespace>` — CrashLoopBackOff? OOMKilled? Pending?
> 2. `kubectl describe pod <pod>` — read Events for exact failure reason
> 3. `kubectl logs <pod> --previous` — logs from the crashed container
> 4. `kubectl get events -n <ns> --sort-by='.lastTimestamp'` — cluster events
> 5. ArgoCD — is the app Synced and Healthy?
> 6. Check resource limits — OOMKilled? CPU throttled (check `kubectl top pod`)?
> 7. Check config/secrets — missing env var? Wrong DB connection string? Secret not synced?
> 8. Check readiness probe — app process running but failing health check → gets no traffic

---

**Q: How do you handle secrets in a CI/CD pipeline?**

> **In GitHub Actions (CI):**
> - **OIDC** — no AWS access keys at all; temporary credentials per run
> - Non-AWS secrets (e.g., Slack webhook, SonarQube token) stored as **GitHub Actions secrets** (encrypted at rest, never visible in logs), referenced as `${{ secrets.SLACK_WEBHOOK }}`
>
> **In EKS pods (runtime):**
> - **EKS Pod Identity** — assigns IAM role to the Kubernetes service account
> - **Secrets Store CSI Driver (ASCP)** — fetches from AWS Secrets Manager and mounts as file or env var inside the pod
> - Pod gets the secret at start time; Secrets Manager handles rotation; ASCP re-mounts on next pod restart
>
> Result: **zero hardcoded credentials** at every stage — not in code, not in pipeline, not in pod specs.

---

**Q: What is a reusable workflow in GitHub Actions? When do you use it?**

> A reusable workflow is defined with `workflow_call` trigger — it can be called from any repo like a function.
>
> ```yaml
> # .github/workflows/build-and-push.yml (in a shared workflows repo)
> on:
>   workflow_call:
>     inputs:
>       image-name:
>         required: true
>         type: string
>     secrets:
>       ECR_ROLE_ARN:
>         required: true
> ```
>
> ```yaml
> # Any microservice repo calling the shared workflow
> jobs:
>   build:
>     uses: myorg/shared-workflows/.github/workflows/build-and-push.yml@main
>     with:
>       image-name: catalog-service
>     secrets:
>       ECR_ROLE_ARN: ${{ secrets.ECR_ROLE_ARN }}
> ```
>
> Use when: you have 5+ microservice repos all needing the same build-test-scan-push pipeline. Define once, call everywhere — changes propagate to all repos automatically.

---

**Q: How do you implement a manual approval gate in GitHub Actions?**

> Use **GitHub Environments** with required reviewers:
> ```yaml
> jobs:
>   deploy-prod:
>     runs-on: ubuntu-latest
>     environment:
>       name: production          # environment with required reviewers configured in GitHub UI
>     steps:
>       - name: Update GitOps repo image tag
>         run: |
>           # commit new image tag → ArgoCD auto-syncs to prod
> ```
>
> In GitHub → Settings → Environments → production → add required reviewers. The job pauses and sends a notification — deployment only proceeds after an approved reviewer clicks "Approve".

---

**Q: How do you cache dependencies in GitHub Actions to speed up builds?**

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.m2/repository        # Maven local cache
    key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
    restore-keys: |
      ${{ runner.os }}-maven-
```

> - Cache key includes a hash of `pom.xml` — cache invalidates automatically when dependencies change
> - On cache hit: Maven skips downloading; build time drops from ~4 min to ~90 seconds
> - Same pattern for npm (`node_modules`), Go modules (`~/go/pkg/mod`), pip

---

**Q: What is a blue-green deployment?**

> - Run two identical environments: **blue** (current live) and **green** (new version)
> - Deploy new version to green — test it completely with no live traffic
> - Switch traffic from blue to green instantly — ALB listener rule update or DNS change
> - **Rollback**: switch back to blue instantly — zero code change
>
> | Advantage | Disadvantage |
> |---|---|
> | Zero-downtime cutover | Double infrastructure cost during switchover |
> | Instant rollback | DB schema changes are tricky — both envs must be compatible |
> | Easy to test before going live | More complex orchestration |

---

**Q: What is a canary deployment?**

> Gradually shift a percentage of live traffic to the new version — monitor metrics — increase percentage if healthy.
>
> ```
> 5% → green (new version)    95% → blue (current)
>         ↓ metrics look good
> 25% → green                 75% → blue
>         ↓ metrics look good
> 100% → green (full cutover)  blue decommissioned
> ```
>
> Tools: **Argo Rollouts**, AWS ALB weighted target groups, Istio traffic splitting.
>
> Advantage over blue-green: limits blast radius — only 5% of users see a bad deployment before rollback.

---

**Q: What is the difference between GitHub Actions and Jenkins?**

| Feature | GitHub Actions | Jenkins |
|---|---|---|
| Hosting | Cloud-native — GitHub-hosted runners | Self-hosted — you manage the server |
| Setup | Zero setup — YAML in `.github/workflows/` | Requires Jenkins server install, plugins, config |
| OIDC / AWS auth | Built-in OIDC support | Plugin-dependent, more complex |
| Marketplace | 15,000+ community actions | Plugin ecosystem (older) |
| Cost | Free for public repos; minutes-based for private | EC2/server cost + maintenance |
| Best for | Modern cloud-native teams, GitHub repos | Legacy enterprise, complex multi-step pipelines |

---

**Q: What is a matrix strategy in GitHub Actions?**

```yaml
jobs:
  test:
    strategy:
      matrix:
        java: [17, 21]
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-java@v4
        with:
          java-version: ${{ matrix.java }}
```

> Runs the job for every combination — 4 parallel jobs (2 Java versions × 2 OSes). Used for: multi-version testing, multi-platform builds, multi-region deployments.

---

**Q: How do you pass data between jobs in GitHub Actions?**

```yaml
jobs:
  build:
    outputs:
      image-tag: ${{ steps.set-tag.outputs.tag }}
    steps:
      - id: set-tag
        run: echo "tag=${{ github.sha }}" >> $GITHUB_OUTPUT

  deploy:
    needs: build
    steps:
      - run: echo "Deploying image tag ${{ needs.build.outputs.image-tag }}"
```

> Use `GITHUB_OUTPUT` to set step outputs → expose as job outputs → consume in downstream jobs with `needs.<job>.outputs.<key>`.

---

**Q: What is ArgoCD ApplicationSet and when do you use it?**

> ApplicationSet is an ArgoCD controller that generates multiple ArgoCD `Application` CRs from a single template — used when managing many clusters or environments.
>
> ```yaml
> apiVersion: argoproj.io/v1alpha1
> kind: ApplicationSet
> spec:
>   generators:
>     - list:
>         elements:
>           - cluster: us-east-1-prod
>             env: prod
>           - cluster: eu-west-1-prod
>             env: prod
>   template:
>     spec:
>       source:
>         helm:
>           valueFiles:
>             - values.yaml
>             - values-{{env}}.yaml
>             - values-{{cluster}}.yaml
>       destination:
>         server: 'https://{{cluster}}.k8s.example.com'
>       syncPolicy:
>         automated:
>           selfHeal: true
> ```
>
> Adding a new cluster = add one entry to `elements`. One push to git → all clusters auto-sync. No manual YAML duplication.

---

## 6. Kubernetes — Q&A

---

**Q: What is a Pod? What is the difference between a Pod and a Container?**

> - **Container** — a running instance of a Docker image; isolated process on the host OS
> - **Pod** — the smallest deployable unit in Kubernetes; wraps one or more containers that share the same **network namespace** (same IP and ports) and **storage volumes**

```
Pod (one IP address)
 ├── Main container  (app — port 8080)
 └── Sidecar container (log shipper / envoy proxy)
       └── Both communicate via localhost — they share the same network
```

> Kubernetes manages Pods, not containers directly. If a container crashes inside a Pod, Kubernetes restarts it within the same Pod. Different Pods communicate via Kubernetes Services.

---

**Q: What is a Deployment? When do you use it?**

> A Deployment manages a ReplicaSet — it ensures the desired number of Pod replicas are always running and handles rolling updates and rollbacks.
>
> Use for: **stateless applications** — web servers, APIs, microservices — anything where pods are interchangeable.
>
> ```yaml
> apiVersion: apps/v1
> kind: Deployment
> metadata:
>   name: catalog
> spec:
>   replicas: 3
>   selector:
>     matchLabels:
>       app: catalog
>   strategy:
>     type: RollingUpdate
>     rollingUpdate:
>       maxUnavailable: 0
>       maxSurge: 1
>   template:
>     spec:
>       containers:
>       - name: catalog
>         image: 123.dkr.ecr.us-east-1.amazonaws.com/catalog:v1.2.3
>         resources:
>           requests:
>             cpu: "250m"
>             memory: "512Mi"
>           limits:
>             cpu: "500m"
>             memory: "1Gi"
> ```

---

**Q: What is the difference between a Deployment and a StatefulSet?**

| Feature | Deployment | StatefulSet |
|---|---|---|
| Pod identity | Random names (`catalog-abc123`) | Stable ordinal names (`mysql-0`, `mysql-1`) |
| Pod storage | Shared or ephemeral | Each pod gets its **own PVC** — persistent and unique |
| Scaling order | All pods start/stop in parallel | Sequential — `app-0` before `app-1` before `app-2` |
| DNS | Common Service — any pod | Each pod gets a stable DNS: `mysql-0.mysql.default.svc.cluster.local` |
| Use case | Stateless apps (APIs, UI, microservices) | Databases, Kafka, ZooKeeper, Elasticsearch |

> In my retail project: all 5 microservices use **Deployments** (stateless). The databases (RDS, DynamoDB, ElastiCache) are AWS managed services — not run inside Kubernetes.

---

**Q: What is a DaemonSet? When do you use one?**

> A DaemonSet ensures **one pod runs on every node** (or a selected subset) in the cluster — automatically. When a new node joins, the DaemonSet pod is scheduled on it; when the node leaves, the pod is removed.
>
> Use cases:
> - **Log collectors** — Fluentd, Fluent Bit shipping logs from every node
> - **Monitoring agents** — Prometheus Node Exporter collecting node-level metrics
> - **Security agents** — Falco, runtime threat detection on every node
> - **CNI plugins** — VPC CNI, Calico run as DaemonSets
> - **Karpenter node agent** — runs on every node for interruption handling

---

**Q: What is a sidecar container? Give a real example from your project.**

> A sidecar is a secondary container in the same Pod as the main app — they share network and volumes.
>
> Real example from Capgemini:
> - **ADOT Collector sidecar** — collects telemetry from the app container (same localhost), exports to X-Ray and CloudWatch
> - **Envoy proxy (service mesh)** — Istio injects an Envoy sidecar for mTLS, traffic management, and observability without changing app code
> - **Init containers** — run to completion before the main container starts. I used them for: waiting for DB to be ready, fetching config from S3 before app starts

---

**Q: What is Kubelet? What does it do?**

> Kubelet is the **node agent** — runs on every worker node. It:
> - Watches the API server for Pod specs assigned to its node
> - Instructs the container runtime (containerd) to start/stop containers
> - Runs liveness/readiness/startup probes
> - Reports pod and node status back to the control plane
> - Restarts containers that crash (respects `restartPolicy`)
>
> You cannot SSH into EKS master nodes — Kubelet only runs on **worker nodes**.

---

**Q: What is the difference between liveness, readiness, and startup probes?**

| Probe | Question it asks | Failure action |
|---|---|---|
| `livenessProbe` | Is the container still alive / not stuck? | Kubernetes **restarts** the container |
| `readinessProbe` | Is the container ready to serve traffic? | Pod **removed from Service endpoints** — no traffic; NOT restarted |
| `startupProbe` | Has the app finished starting up? | Kubernetes restarts if app never passes startup; prevents liveness from firing too early |

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
  failureThreshold: 3

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5

startupProbe:
  httpGet:
    path: /health
    port: 8080
  failureThreshold: 30    # 30 × 10s = 5 minutes for slow Spring Boot startup
  periodSeconds: 10
```

> Startup probe prevents liveness from killing a slow-starting Spring Boot app during its 2–3 minute JVM warm-up.

---

**Q: What happens when a pod is in CrashLoopBackOff?**

> The container keeps crashing and Kubernetes restarts it with **exponential backoff** (10s → 20s → 40s → ... → max 5 min).
>
> **Debug steps:**
> ```bash
> kubectl logs <pod> --previous          # logs from the crashed container
> kubectl describe pod <pod>             # look at Exit Code in Last State + Events
> ```
>
> | Exit Code | Meaning |
> |---|---|
> | `1` | Application error — check logs |
> | `137` | OOMKilled — out of memory, increase `resources.limits.memory` |
> | `139` | Segmentation fault — native code crash |
> | `2` | Misuse of shell command |
>
> **Common causes:** bad env var / missing secret, DB connection refused, port conflict, missing config file.

---

**Q: What happens when a pod exceeds its memory limit?**

> Container is **OOMKilled** (Out of Memory Killed) by the Linux kernel — process is forcefully terminated.
> - `kubectl describe pod` → Last State shows `OOMKilled`, Exit Code `137`
> - Kubernetes restarts the container automatically
> - `resources.requests` → affects **scheduling** (which node the pod lands on)
> - `resources.limits` → affects **runtime enforcement** (hard ceiling — exceed it → OOMKilled)
>
> Fix: increase `resources.limits.memory`, profile the app for memory leaks, or enable VPA to auto-tune limits.

---

**Q: What is the difference between ClusterIP, NodePort, and LoadBalancer service types?**

| Type | Accessible From | Use Case |
|---|---|---|
| **ClusterIP** (default) | Inside cluster only | Internal service-to-service communication |
| **NodePort** | Outside via `NodeIP:Port` (30000–32767) | Dev/testing, legacy setups |
| **LoadBalancer** | External via cloud LB (AWS NLB auto-provisioned) | Production external TCP traffic |
| **ExternalName** | DNS alias to external service | Point to RDS endpoint or external API |

> For HTTP/HTTPS production traffic on EKS: use **ALB Ingress** via the Load Balancer Controller instead of LoadBalancer-type Services — better SSL termination, path/host-based routing, cheaper (one ALB for many services).

---

**Q: What is a ConfigMap vs a Secret in Kubernetes?**

> - **ConfigMap** — stores non-sensitive configuration: env vars, config files, feature flags
> - **Secret** — stores sensitive data: passwords, API keys, TLS certs — stored as **base64 encoded** (NOT encrypted by default in etcd)
>
> **Problem with native Secrets:**
> ```bash
> kubectl get secret db-secret -o jsonpath='{.data.password}' | base64 -d
> # Anyone with kubectl get secret can decode it instantly
> ```
>
> **What I use instead:**
> - **Secrets Store CSI Driver (ASCP)** + AWS Secrets Manager — secret never stored in etcd; mounted directly into pod from Secrets Manager at runtime
> - If you must use native Secrets: enable **KMS envelope encryption** for etcd at rest

---

**Q: What is RBAC in Kubernetes?**

> Role-Based Access Control — controls **who can do what** to **which resources** in which **namespace or cluster**.

| Object | Scope | Purpose |
|---|---|---|
| `Role` | Namespace | Permissions within one namespace |
| `ClusterRole` | Cluster-wide | Permissions across all namespaces |
| `RoleBinding` | Namespace | Grants a Role to a user/service account |
| `ClusterRoleBinding` | Cluster-wide | Grants a ClusterRole cluster-wide |

```yaml
# Give a service account read-only pod access in one namespace
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: retail
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list", "watch"]
---
kind: RoleBinding
metadata:
  name: read-pods
  namespace: retail
subjects:
- kind: ServiceAccount
  name: monitoring-agent
roleRef:
  kind: Role
  name: pod-reader
```

---

**Q: What is a Kubernetes NetworkPolicy and why is it important?**

> NetworkPolicy controls which pods can talk to which other pods (and external endpoints). **Default in Kubernetes: all pods can communicate freely with all other pods** — no restriction.
>
> **Real scenario — restrict UI from reaching Database directly:**
> ```
> Goal:  UI → API → Database  ✅
>        UI → Database         ❌
> ```
>
> ```yaml
> # Step 1: Default deny all — start with zero trust
> apiVersion: networking.k8s.io/v1
> kind: NetworkPolicy
> metadata:
>   name: default-deny-all
> spec:
>   podSelector: {}        # applies to ALL pods in namespace
>   policyTypes: [Ingress, Egress]
> ---
> # Step 2: Allow only API pod to reach Database
> kind: NetworkPolicy
> metadata:
>   name: allow-api-to-db
> spec:
>   podSelector:
>     matchLabels:
>       app: database
>   ingress:
>   - from:
>     - podSelector:
>         matchLabels:
>           app: api
> ---
> # Step 3: Allow only UI to reach API
> kind: NetworkPolicy
> metadata:
>   name: allow-ui-to-api
> spec:
>   podSelector:
>     matchLabels:
>       app: api
>   ingress:
>   - from:
>     - podSelector:
>         matchLabels:
>           app: ui
> ```
>
> **Important:** NetworkPolicy requires a CNI that enforces it — **Calico**, **Cilium**, or **Weave**. The default VPC CNI does not enforce NetworkPolicy.

---

**Q: What is HPA and how does it work?**

> Horizontal Pod Autoscaler scales pod **replicas** up or down based on observed metrics vs a target threshold.
>
> ```yaml
> apiVersion: autoscaling/v2
> kind: HorizontalPodAutoscaler
> spec:
>   scaleTargetRef:
>     kind: Deployment
>     name: catalog
>   minReplicas: 2
>   maxReplicas: 20
>   metrics:
>   - type: Resource
>     resource:
>       name: cpu
>       target:
>         type: Utilization
>         averageUtilization: 70   # scale up if CPU > 70%
> ```
>
> **Scale-up chain with Karpenter:**
> ```
> CPU > 70% → HPA adds replicas → pods Pending (no node capacity)
>   → Karpenter provisions new EC2 node in < 60s → pods schedule
> ```
>
> **Beyond CPU/Memory — KEDA (Kubernetes Event-Driven Autoscaling):**
> - Scale on SQS queue depth, Kafka consumer lag, HTTP request rate, custom CloudWatch metrics
> - KEDA extends HPA — same HPA mechanism, different metric source

---

**Q: What is the relationship between PV, PVC, and StorageClass?**

```
StorageClass (defines HOW to provision — AWS EBS gp3, encryption, IOPS)
    ↓ dynamic provisioning — PV created automatically when PVC submitted
PersistentVolume (actual EBS volume in AWS — 20Gi gp3)
    ↑ bound to
PersistentVolumeClaim (app's request: "I need 20Gi ReadWriteOnce")
    ↑ mounted by
Pod (mounts PVC at /data)
```

> - **Without StorageClass**: PV must be manually pre-created (static provisioning)
> - **With StorageClass**: PV auto-created on PVC submission (dynamic provisioning) — what I use
> - **EBS access modes**: `ReadWriteOnce` only — block storage, one node at a time
> - **Need `ReadWriteMany`** (shared across nodes)? → Use **EFS** (elastic file system)

---

**Q: What is the EBS CSI Driver? Why is it needed?**

> EBS CSI Driver implements the **Container Storage Interface** spec — runs as a DaemonSet on worker nodes.
>
> Without it: Kubernetes cannot dynamically create, attach, detach, or resize AWS EBS volumes from PVCs.
> With it: submitting a PVC automatically provisions an EBS volume, attaches it to the scheduled node, and mounts it into the pod.
>
> I installed it as an EKS managed add-on via Terraform:
> ```hcl
> resource "aws_eks_addon" "ebs_csi" {
>   cluster_name             = aws_eks_cluster.main.name
>   addon_name               = "aws-ebs-csi-driver"
>   service_account_role_arn = aws_iam_role.ebs_csi.arn
> }
> ```

---

**Q: What is the difference between `kubectl apply` and `kubectl create`?**

| Command | Behaviour |
|---|---|
| `kubectl create` | Creates the resource — **fails** if it already exists |
| `kubectl apply` | Creates if not exists, **updates** if it does — idempotent |
| `kubectl replace` | Deletes and recreates — must exist; not safe for live objects |

> Always use `kubectl apply` in CI/CD pipelines — idempotent, safe to run repeatedly.

---

**Q: What is a Job vs a CronJob in Kubernetes?**

> - **Job** — runs a pod to completion once; retries on failure; used for one-off tasks (DB migration, batch processing, data import)
> - **CronJob** — runs a Job on a schedule (cron expression); used for periodic tasks (nightly reports, cache warming, cleanup)
>
> In my project: DB schema migrations run as a **pre-upgrade Helm hook Job** before the application pods roll out — ensuring the DB is migrated before any new pod starts.

---

**Q: What is a mutating webhook? Give a real example.**

> A mutating webhook intercepts incoming Kubernetes API requests and **modifies the object** before it's persisted to etcd.
>
> Real example from Capgemini — **ADOT auto-instrumentation**:
> - ADOT Operator installs a mutating admission webhook
> - When a pod with annotation `instrumentation.opentelemetry.io/inject-java: "true"` is created, the webhook automatically injects a Java agent as an init container
> - The agent instruments the app for traces, metrics, and logs — **zero application code changes**
>
> Other examples: Istio injects an Envoy sidecar into every pod; Karpenter's webhook patches node selectors.

---

**Q: What is the difference between HPA and VPA?**

| Feature | HPA | VPA |
|---|---|---|
| What it scales | Number of pod **replicas** | Pod **CPU/memory requests and limits** |
| When it acts | While pod is running — adds/removes replicas | On pod restart — updates resource requests |
| Best for | Stateless services with variable load | Services where resource needs change but replica count is fixed |
| Together | Can be used together — HPA for scale-out, VPA for right-sizing | Don't use both on CPU simultaneously — conflict |

---

**Q: What is `kubectl cordon` vs `kubectl drain`?**

> - `kubectl cordon <node>` — marks node **unschedulable**. No new pods placed on it. Existing pods keep running.
> - `kubectl drain <node>` — evicts all pods gracefully (respects PodDisruptionBudget), then cordons the node.
>
> ```bash
> kubectl cordon ip-10-0-1-100.ec2.internal          # mark unschedulable
> kubectl drain ip-10-0-1-100.ec2.internal \
>   --ignore-daemonsets \                             # don't evict DaemonSet pods
>   --delete-emptydir-data                            # evict pods using emptyDir volumes
> ```
>
> Karpenter does this automatically on Spot interruption or consolidation — you only run it manually for planned node maintenance.

---

**Q: How does Kubernetes self-healing work?**

> Kubernetes continuously reconciles **desired state** (what you defined) vs **actual state** (what's running).
>
> | Failure | Kubernetes Response |
> |---|---|
> | Container crashes | Kubelet restarts it (respects restartPolicy) |
> | Node goes down | Pods rescheduled on healthy nodes |
> | Pod deleted manually | ReplicaSet creates a replacement |
> | Deployment image updated | Rolling update — new pods, old pods terminated after ready |
> | Manual `kubectl apply` changes cluster state | ArgoCD self-heal reverts to git (GitOps layer) |

---

## 7. Monitoring & Observability — Q&A

---

**Q: What are the three pillars of observability? Which AWS services did you use for each?**

| Pillar | What it tells you | Service I used |
|---|---|---|
| **Traces** | Request flow across microservices — which service was slow, which hop failed, latency per step | AWS X-Ray via ADOT auto-instrumentation |
| **Logs** | Detailed event records from each service — errors, warnings, app output at a specific moment | Amazon CloudWatch Logs via ADOT collector |
| **Metrics** | Aggregate numbers over time — CPU, request rate, error rate, p99 latency | Amazon Managed Prometheus (AMP) + Grafana dashboards |

> Together they give full visibility: metrics tell you **something is wrong**, traces tell you **where** it went wrong, logs tell you **why**.

---

**Q: What is OpenTelemetry (OTEL)?**

> OpenTelemetry is a **vendor-neutral, open-source observability framework** — APIs, SDKs, and tools to instrument, generate, collect, and export telemetry (traces, metrics, logs) from applications.
>
> Key benefit: **write instrumentation once → export to any backend** (X-Ray, Jaeger, Datadog, Prometheus, Loki) without changing application code.
>
> Components:
> - **OTEL SDK** — language-specific library that instruments your app (Java, Go, Node.js)
> - **OTEL Collector** — central pipeline: receives telemetry, processes (filters, enriches, samples), exports to backends
> - **Auto-instrumentation** — zero code changes; agent injects itself and instruments common frameworks automatically

---

**Q: What is ADOT? How is it different from vanilla OpenTelemetry?**

> **ADOT (AWS Distro for OpenTelemetry)** = AWS's supported, production-tested distribution of OpenTelemetry, pre-configured for AWS backends.
>
> | Feature | Vanilla OTEL | ADOT |
> |---|---|---|
> | AWS X-Ray exporter | Community-maintained | AWS-maintained, officially supported |
> | Amazon Managed Prometheus | Manual config | Native remote write support |
> | CloudWatch | Plugin | First-class support |
> | EKS integration | Manual setup | ADOT Operator — Helm chart, CRDs, auto-instrumentation webhook |
> | Support | Community | AWS Support covers it |
>
> I deployed the **ADOT Operator** on EKS via Helm — it installed the mutating webhook for auto-instrumentation and the collector DaemonSet.

---

**Q: How did ADOT auto-instrumentation work for Java Spring Boot without code changes?**

> 1. Install ADOT Operator on EKS (Helm chart)
> 2. Create an `Instrumentation` CRD defining the Java agent config and export endpoints
> 3. Annotate the pod or namespace:
>    ```yaml
>    annotations:
>      instrumentation.opentelemetry.io/inject-java: "true"
>    ```
> 4. The ADOT **mutating webhook** intercepts pod creation and automatically injects a Java agent as an **init container**
> 5. The Java agent instruments Spring Boot HTTP calls, DB queries, and messaging — publishes spans to the OTEL Collector running as a sidecar or DaemonSet
> 6. Collector exports: **traces → X-Ray**, **logs → CloudWatch**, **metrics → AMP**
>
> **Zero application code changes.** The dev team had no idea observability was enabled.

---

**Q: How did you reduce observability costs by 85%?**

> Health check endpoints (`/health`, `/actuator/health`) run every 30 seconds per pod. With 5 microservices × multiple replicas, this generated **thousands of traces per hour** — high volume, zero diagnostic value.
>
> I added a **filter processor** in the ADOT Collector pipeline config:
> ```yaml
> processors:
>   filter/drop_health_checks:
>     traces:
>       span:
>         - 'attributes["http.target"] == "/health"'
>         - 'attributes["http.target"] == "/actuator/health"'
>         - 'attributes["http.target"] == "/ready"'
>
> service:
>   pipelines:
>     traces:
>       processors: [filter/drop_health_checks, batch]
>       exporters: [awsxray]
> ```
>
> Additionally used **tail-based sampling** — always keep error traces and traces > 500ms; sample 10% of fast successful traces.
>
> Result: **85% reduction in X-Ray ingestion cost** with zero loss in diagnostic observability for real user requests.

---

**Q: What is the OTEL Collector? Why use it instead of sending data directly to backends?**

> The OTEL Collector is a **central telemetry pipeline** — receives data from apps, processes it, and exports to one or more backends.
>
> ```
> App (OTEL SDK) → OTEL Collector → X-Ray
>                               → CloudWatch
>                               → Amazon Managed Prometheus
> ```
>
> **Why use it instead of direct export from the app:**
> - **Vendor-neutral** — switch from X-Ray to Jaeger without any code change (just update collector config)
> - **Batching** — collector buffers and sends in batches → fewer API calls → lower cost
> - **Filtering & sampling** — drop health check traces, sample slow requests — app code doesn't do this
> - **Data enrichment** — add cluster name, region, pod name as attributes to every span
> - **Decoupling** — app doesn't need to know the backend; only the collector does

---

**Q: What is Prometheus? How does it collect metrics?**

> Prometheus is a **pull-based metrics system** — it scrapes `/metrics` HTTP endpoints exposed by applications and exporters at a configured interval (default 15s).
>
> **Components:**
> - **Scrape configs** — define targets to scrape (pods, nodes, services by label selector)
> - **ServiceMonitor / PodMonitor** — Kubernetes CRDs (kube-prometheus-stack) for declarative scrape target config
> - **TSDB** — time-series database for local storage (15-day default retention)
> - **PromQL** — query language for aggregating and alerting on metrics
> - **Alertmanager** — handles alert routing, grouping, deduplication, silencing
>
> **In my project:** I used **Amazon Managed Prometheus (AMP)** — fully managed Prometheus-compatible backend. ADOT Collector remote-writes metrics to AMP, so I don't manage Prometheus storage, TSDB compaction, or retention.

---

**Q: Write a PromQL query. What does it mean?**

```promql
# HTTP error rate for the catalog service (last 5 minutes)
rate(http_server_requests_seconds_count{app="catalog", status=~"5.."}[5m])
/
rate(http_server_requests_seconds_count{app="catalog"}[5m])
```

> - `rate(...)` — per-second rate of increase over the time window
> - `{app="catalog"}` — label selector filtering to catalog service only
> - `status=~"5.."` — regex match for all 5xx status codes
> - Division = error rate as a fraction (multiply by 100 for percentage)

```promql
# p99 latency for catalog service
histogram_quantile(0.99,
  rate(http_server_requests_seconds_bucket{app="catalog"}[5m])
)
```

---

**Q: What is Grafana and what dashboards did you build?**

> Grafana is a **visualization platform** that connects to data sources (Prometheus, CloudWatch, Loki, X-Ray) and renders dashboards with time-series graphs, heatmaps, tables, and alerts.
>
> **Dashboards I built at Capgemini:**
>
> | Dashboard | Key Panels |
> |---|---|
> | EKS Cluster Health | Node CPU/memory, pod restart count, node count (On-Demand vs Spot), Karpenter events |
> | Service-Level (per microservice) | Request rate (RPS), p50/p95/p99 latency, error rate (%), active connections |
> | Karpenter | Node provisioning events, Spot interruptions, consolidation activity, savings vs On-Demand |
> | Observability Cost | X-Ray trace ingestion by service, CloudWatch log volume, AMP metric cardinality |
>
> **Data source:** Amazon Managed Prometheus (AMP) — authenticated via AWS SigV4 signing.

---

**Q: What is Alertmanager? Have you used it? How do you bridge the gap?**

> Alertmanager is the **alerting component of the Prometheus stack**. It receives alerts fired by Prometheus alert rules and handles:
> - **Routing** — send different alerts to different receivers (Slack, PagerDuty, email)
> - **Grouping** — batch multiple related alerts into one notification
> - **Deduplication** — suppress duplicate alerts during an ongoing incident
> - **Silencing** — mute known alerts during planned maintenance windows
> - **Inhibition** — suppress low-severity alerts if a high-severity alert is already firing
>
> **I haven't used Alertmanager directly — here's my bridge answer:**
> > "In my projects I used **CloudWatch Alarms → SNS → Email/Slack** for infrastructure-level alerting (EC2, ALB, RDS metrics), and **Grafana alerting** for application-level thresholds on Prometheus metrics. Grafana alerting does the same job as Alertmanager — routing rules, contact points (Slack, PagerDuty), grouping, and silencing. The configuration format differs (Grafana UI vs YAML) but the problem is identical. If this role uses Alertmanager, I can configure it quickly — the routing rule YAML is straightforward given my strong Prometheus/Grafana foundation."

---

**Q: What is the difference between head-based and tail-based sampling?**

> - **Head-based sampling** — decision made at the **start** of a trace (e.g., sample 10% of all incoming requests randomly). Fast, low overhead, but may discard the 10% that contains the actual error.
> - **Tail-based sampling** — decision made **after the trace completes**, based on outcome (e.g., always keep errors and slow traces > 500ms; drop fast successful ones).
>
> I used **tail-based filtering** via the OTEL Collector filter processor — always keep error traces and slow traces, drop health check traces entirely. This is more intelligent than random sampling.

---

**Q: What is AWS X-Ray? What problem does it solve?**

> X-Ray is AWS's **distributed tracing service** — it tracks requests as they flow across multiple microservices, showing exactly where latency occurs and which service caused an error.
>
> **Without tracing:** an end user gets a 500 error. You check logs across 5 microservices manually to find where it failed.
>
> **With X-Ray:** you open the trace for that request and see the full call chain — UI → Catalog → RDS MySQL — with latency for each hop. The RDS query took 8 seconds instead of 20ms → immediately obvious that the DB is the bottleneck.
>
> **Service Map:** X-Ray auto-generates a visual dependency graph showing all services, error rates, and latency percentiles between them.

---

**Q: What is Amazon Managed Prometheus (AMP)?**

> AMP is a **fully managed, Prometheus-compatible monitoring service** — you get all of Prometheus (scraping, PromQL, alert rules) without managing Prometheus servers, TSDB compaction, storage, or HA.
>
> - ADOT Collector `remote_write` to AMP endpoint → metrics stored and queryable
> - Grafana connects to AMP as a Prometheus data source (authenticated via AWS SigV4)
> - Alert rules written in PromQL → SNS for notifications
> - Pay per metric sample ingested + query

---

**Q: How do you correlate logs and traces for a single request?**

> Every distributed trace has a **Trace ID**. With ADOT auto-instrumentation:
> - The Trace ID is automatically injected into every log line produced during that request
> - **CloudWatch Logs Insights** — query by Trace ID to find all log lines for a specific failing request
> - **X-Ray** — view the full service map and span timeline for that Trace ID
>
> ```sql
> -- CloudWatch Logs Insights query to find all errors with their trace ID
> fields @timestamp, @message, traceId
> | filter level = "ERROR"
> | sort @timestamp desc
> | limit 50
> ```
>
> This is the core value of OpenTelemetry — logs, metrics, and traces are all correlated by the same Trace ID, making root cause analysis fast.

---

**Q: What CloudWatch metrics or alarms did you set up?**

> **At Infosys (EC2/ALB workloads):**
> - ALB `HTTPCode_Target_5XX_Count` > threshold → SNS → email alert
> - ALB `TargetResponseTime` p99 > 2s → SNS alert
> - EC2 `CPUUtilization` > 80% → Auto Scaling policy
> - RDS `FreeStorageSpace` < 10GB → SNS critical alert
> - Auto Scaling Group `GroupInServiceInstances` < minimum → SNS alert
>
> **At Capgemini (EKS):**
> - AMP alert rules in PromQL → SNS: pod restart count > 5 in 10 minutes, p99 latency > 1s, error rate > 1%
> - CloudWatch: EKS node CPU > 90%, Karpenter NodeClaim failure events
> - X-Ray: service map error rate > 5% threshold

---

**Q: Scenario — Your Catalog service is returning 500 errors. Walk through your investigation using your observability stack.**

> **Step 1 — X-Ray Service Map**
> - Open X-Ray console → Service Map → find Catalog service highlighted in red (high error rate)
> - Click → see which downstream dependency is failing: is it the RDS MySQL call or the app logic?
>
> **Step 2 — Drill into a failing trace**
> - Find a specific 500 error trace → open span timeline
> - See: UI → Catalog (2ms) → RDS Query (8,432ms!) → response
> - Immediately clear: the DB query is the bottleneck
>
> **Step 3 — CloudWatch Logs Insights**
> ```sql
> fields @timestamp, @message
> | filter traceId = "1-abc123-def456"
> | sort @timestamp asc
> ```
> - Find the exact SQL query that timed out, the exception stacktrace
>
> **Step 4 — Grafana**
> - Check RDS `DatabaseConnections` — at max_connections limit?
> - Check EKS HPA — did pod scale-up double DB connections without warning?
>
> **Step 5 — Fix and verify**
> - Increase `max_connections` or add PgBouncer connection pooling
> - CloudWatch alarm on `DatabaseConnections` > 80% of limit going forward

---

## 8. Terraform — Q&A

---

**Q: What are the core Terraform CLI commands and what does each do?**

| Command | Purpose |
|---|---|
| `terraform init` | Downloads provider plugins, sets up backend, initializes modules — run first, always |
| `terraform validate` | Checks HCL syntax and config structure — offline, no credentials needed, fast |
| `terraform plan` | Compares desired state vs real state — shows what will change; nothing is modified |
| `terraform apply` | Executes the plan — creates/modifies/destroys real resources |
| `terraform destroy` | Destroys all resources managed by the config — use with extreme caution |
| `terraform fmt` | Auto-formats `.tf` files to canonical style — run before committing |
| `terraform output` | Prints output values from state |
| `terraform show` | Human-readable view of state or a saved plan file |
| `terraform state list` | Lists all resources tracked in state |
| `terraform state mv` | Moves a resource to a new address in state — used when renaming resources |
| `terraform state rm` | Removes a resource from state without destroying it in the cloud |
| `terraform import` | Imports existing cloud resource into Terraform state without recreating it |
| `terraform taint` / `apply -replace` | Forces destroy + recreate of a specific resource on next apply |
| `terraform workspace` | Manages multiple state workspaces (dev, staging, prod) |

---

**Q: What is the difference between `terraform validate` and `terraform plan`?**

> - `validate` — checks **syntax and config structure only**; works offline; no AWS credentials needed; runs in seconds
> - `plan` — calls **cloud APIs** to compare desired vs real state; requires credentials and network access; takes longer
>
> Always run `validate` first (fast, cheap) → then `plan` (slower, needs auth) → then `apply`.

---

**Q: What is the Terraform Settings Block? What goes inside it?**

```hcl
terraform {
  required_version = ">= 1.6.0"        # pins Terraform CLI version

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"               # >= 5.0, < 6.0
    }
  }

  backend "s3" {                        # remote state configuration
    bucket         = "my-tf-state"
    key            = "prod/eks/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "tf-state-lock"
    encrypt        = true
  }
}
```

> `~> 5.0` means: allow `>= 5.0` and `< 6.0` — patches and minor versions OK, no breaking major version jumps.

---

**Q: How do you manage Terraform state in a team environment?**

```hcl
terraform {
  backend "s3" {
    bucket         = "my-tfstate-bucket"
    key            = "prod/eks/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-state-lock"
    encrypt        = true              # KMS encryption for state at rest
  }
}
```

> - **S3** — stores the state file; enable versioning for rollback to previous state
> - **DynamoDB** — state locking; prevents two engineers running `apply` simultaneously (would corrupt state)
> - **KMS encryption** — state file contains sensitive resource attributes (RDS passwords, etc.)
> - Split state by layer (networking, EKS cluster, apps) to reduce blast radius of a failed apply

---

**Q: What is infrastructure drift and how does Terraform detect it?**

> Drift = real infrastructure differs from what Terraform state records (someone manually changed a resource).

| Scenario | Terraform Behaviour |
|---|---|
| Resource deleted outside Terraform | Recreates it on next `apply` |
| Resource attribute changed outside Terraform | Reverts the change on next `apply` |
| `terraform plan -refresh-only` | Shows drift without changing anything — safe to run |
| `terraform refresh` | Updates state to match reality — no apply |

> **Prevention:** Restrict IAM permissions (no console changes allowed), enable AWS Config, use CloudTrail to audit who changed what.

---

**Q: What are Terraform meta-arguments? List them all.**

> Meta-arguments are special arguments supported by **every** `resource` block — not provider-specific:

| Meta-Argument | Purpose |
|---|---|
| `depends_on` | Explicit dependency — force ordering when Terraform can't detect it automatically |
| `count` | Create N identical copies of a resource |
| `for_each` | Create one resource per map key or set element |
| `provider` | Use a non-default provider alias (multi-region, cross-account) |
| `lifecycle` | Control create/destroy behaviour (`create_before_destroy`, `prevent_destroy`, `ignore_changes`) |

---

**Q: When do you use `depends_on` vs relying on implicit dependency?**

> Terraform auto-detects dependencies when you **reference one resource's attribute in another** — e.g., `aws_subnet.id` in an EC2 resource.
>
> Use `depends_on` when the dependency exists at the **API level** but not in HCL — the EC2 needs an IAM policy attached before it can start, but the EC2 resource doesn't reference the policy ARN directly:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abc123"
  instance_type = "t3.micro"
  depends_on    = [aws_iam_role_policy_attachment.web_policy]
}
```

---

**Q: What is the difference between `count` and `for_each`? Which do you prefer?**

| Feature | `count` | `for_each` |
|---|---|---|
| Input type | Number | Map or Set of strings |
| Resource address | `aws_subnet.public[0]`, `[1]`, `[2]` | `aws_subnet.public["us-east-1a"]` |
| Deletion problem | Deleting index `0` shifts all indexes → mass destroy/recreate | Removing one key only affects that one resource |
| Best for | Truly identical N copies | Resources with unique names or configs |

```hcl
# count — 3 identical subnets (fragile if order changes)
resource "aws_subnet" "public" {
  count             = 3
  cidr_block        = cidrsubnet(var.vpc_cidr, 8, count.index)
  availability_zone = var.azs[count.index]
}

# for_each — one subnet per AZ (stable, safe to delete any one)
resource "aws_subnet" "public" {
  for_each          = toset(["us-east-1a", "us-east-1b", "us-east-1c"])
  availability_zone = each.value
  cidr_block        = cidrsubnet(var.vpc_cidr, 8, index(var.azs, each.value))
}
```

> Always prefer `for_each` over `count` for resources with unique identifiers — safer deletions.

---

**Q: What are `lifecycle` rules in Terraform?**

```hcl
resource "aws_db_instance" "main" {
  # ...
  lifecycle {
    create_before_destroy = true    # create replacement before destroying old one — avoids downtime
    prevent_destroy       = true    # terraform destroy will ERROR if this is set — protects critical resources
    ignore_changes        = [tags, password]  # ignore external changes to these attributes
  }
}
```

> Use `prevent_destroy = true` on: RDS instances, S3 buckets with data, EKS clusters in production — prevents accidental `terraform destroy`.
>
> Use `ignore_changes` when an external system modifies an attribute you don't want Terraform to revert — e.g., auto-scaling group's `desired_capacity` (managed by ASG, not Terraform).

---

**Q: What is variable precedence in Terraform? Lowest to highest.**

| Priority | Source | Example |
|---|---|---|
| 1 (lowest) | Default in `variable {}` block | `default = "t3.micro"` |
| 2 | `terraform.tfvars` file | `instance_type = "t3.small"` |
| 3 | `*.auto.tfvars` files | auto-loaded alphabetically |
| 4 | `-var-file` flag | `terraform apply -var-file=prod.tfvars` |
| 5 | `-var` flag | `terraform apply -var="instance_type=t3.large"` |
| 6 | Interactive prompt | no default set → Terraform asks at runtime |
| 7 (highest) | Environment variable | `export TF_VAR_instance_type=t3.xlarge` |

---

**Q: What is the difference between `terraform.tfvars` and `*.auto.tfvars`?**

> - Both are **automatically loaded** — no flag needed
> - `terraform.tfvars` — single well-known file, always loaded first
> - `*.auto.tfvars` — any matching file auto-loaded alphabetically after `terraform.tfvars`; allows splitting vars by feature: `network.auto.tfvars`, `compute.auto.tfvars`
> - Non-auto `.tfvars` files (e.g., `prod.tfvars`) must be passed explicitly with `-var-file=prod.tfvars`

---

**Q: What are sensitive variables in Terraform? What do they protect?**

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

> - Terraform **redacts** the value in `plan` and `apply` terminal output — shows `(sensitive value)` instead
> - Value is still stored in the **state file in plain text** — encrypt state (S3 + KMS) and restrict access
> - Does NOT prevent the value from being used in resources — only hides it from logs/output

---

**Q: What is a Terraform data source?**

> Reads existing infrastructure **without managing it** — read-only reference to something that already exists.

```hcl
# Fetch latest Amazon Linux 2023 AMI without hardcoding the ID
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["al2023-ami-*-x86_64"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id    # always uses the latest AMI
  instance_type = "t3.micro"
}
```

> Other common data sources: `aws_vpc`, `aws_subnets`, `aws_caller_identity`, `aws_region`, `aws_eks_cluster`.

---

**Q: What is `terraform import`? What are its limitations?**

> Imports an existing real cloud resource into Terraform state — without destroying and recreating it.
> ```bash
> terraform import aws_s3_bucket.my_bucket my-existing-bucket-name
> terraform import aws_instance.web i-0abc123def456
> ```
>
> **Limitations:**
> - Does **not** write `.tf` config — you must write the resource block manually to match the imported state
> - Running `plan` after import will show changes until your `.tf` config matches exactly
> - Not all resources support import
> - Complex resources (EKS cluster, RDS with many attributes) are tedious to import accurately

---

**Q: What is `terraform state rm`? When do you use it?**

> Removes a resource from Terraform state **without destroying it in the cloud**. The real resource continues to exist but Terraform no longer manages it.
>
> Use when: you want to stop managing a resource with Terraform (hand it off to another team), or when a resource is so corrupted in state you need to re-import it fresh.

```bash
terraform state rm aws_instance.old_server
```

---

**Q: What is `terraform apply -replace`? When do you use it?**

> Forces **destroy + recreate** of a specific resource on the next apply — even if Terraform doesn't detect any configuration change.
>
> ```bash
> terraform apply -replace="aws_instance.web"
> ```
>
> Use when: an EC2 instance is in a bad state (corrupted user data, failed bootstrap), EBS volume has silent corruption, or a resource needs a fresh start but config hasn't changed.

---

**Q: A colleague ran `terraform destroy` on production. How do you recover?**

> 1. **Check S3 state versioning** — if enabled, restore the previous state file from S3 console (versioned bucket)
> 2. `terraform plan` — see what needs to be recreated
> 3. `terraform apply` — recreate all resources
> 4. **Databases**: restore from RDS automated backup or snapshot (point-in-time restore)
> 5. **Data in S3**: check S3 versioning / bucket policies
>
> **Post-incident actions:**
> - Add `lifecycle { prevent_destroy = true }` to all critical resources (RDS, EKS, S3)
> - Restrict IAM — engineers should not have `terraform:apply` + `terraform:destroy` in production; use pipeline-only roles
> - Require `terraform plan` review before `apply` in a PR process

---

**Q: What are Terraform modules?**

```hcl
# modules/vpc/main.tf — reusable module
resource "aws_vpc" "this" {
  cidr_block = var.cidr
  tags       = var.tags
}
output "vpc_id" { value = aws_vpc.this.id }

# prod/main.tf — calling the module
module "vpc" {
  source = "../modules/vpc"
  cidr   = "10.0.0.0/16"
  tags   = { Environment = "prod" }
}

# Pass module output to another resource
resource "aws_subnet" "public" {
  vpc_id = module.vpc.vpc_id
}
```

> - Avoids code duplication (DRY) — same module called with different variables for dev/prod
> - Can also use **public registry modules**: `source = "terraform-aws-modules/vpc/aws"` — community-maintained, production-tested

---

**Q: What is the difference between a public registry module and a local module?**

| Feature | Public Registry Module | Local Module |
|---|---|---|
| Source | `terraform-aws-modules/vpc/aws` | `../modules/vpc` |
| Versioning | Yes — `version = "5.1.2"` | No — tied to file system |
| Maintenance | Community/HashiCorp maintained | You own and maintain it |
| Upgrade command | `terraform init -upgrade` → updates `.terraform.lock.hcl` | Edit files directly |
| Trust | Widely tested, open source | Custom, internal |

---

**Q: What is the `.terraform.lock.hcl` file? Should it be committed to git?**

> Records exact provider and module versions used — a "lock file" like `package-lock.json` in Node.js.
>
> **Yes, always commit it to git.** It ensures every team member and every CI pipeline uses the exact same provider version — no surprise breaking changes from a provider auto-upgrade.
>
> To upgrade providers/modules: edit version constraints → `terraform init -upgrade` → new lock file → commit.

---

**Q: What are Terraform for loops and splat operators?**

```hcl
# For loop — returns list
output "upper_azs" {
  value = [for az in var.availability_zones : upper(az)]
  # ["US-EAST-1A", "US-EAST-1B"]
}

# For loop with filter
output "prod_types" {
  value = [for env, type in var.instance_types : type if env == "prod"]
}

# Splat operator — collect one attribute from all instances
output "all_private_ips" {
  value = aws_instance.web[*].private_ip   # [*] is modern; .* is legacy
}
```

---

**Q: What are `toset()`, `tomap()`, `keys()` functions used for?**

| Function | Purpose | Example |
|---|---|---|
| `toset(list)` | Converts list to set (removes duplicates) — required for `for_each` | `for_each = toset(var.azs)` |
| `tomap(object)` | Converts object to map type — for `for_each` compatibility | |
| `keys(map)` | Returns sorted list of map keys | `keys({a=1,b=2})` → `["a","b"]` |
| `values(map)` | Returns list of map values | |
| `lookup(map, key, default)` | Safe map access with fallback | `lookup(var.types, "prod", "t3.micro")` |
| `file(path)` | Reads file content as string | `public_key = file("~/.ssh/id_rsa.pub")` |

---

**Q: What is the `random` provider used for?**

```hcl
resource "random_id" "suffix" {
  byte_length = 4
}

resource "aws_s3_bucket" "state" {
  bucket = "my-tf-state-${random_id.suffix.hex}"  # "my-tf-state-a3f2b1c4"
}
```

> S3 bucket names must be globally unique — appending a random hex suffix prevents naming conflicts.
> `random_id` value is stable across plans — only regenerates if the resource is destroyed.

---

**Q: What is a Terraform provisioner? Why are they a last resort?**

> Provisioners run scripts on resources after creation — `local-exec` (on your machine), `remote-exec` (on the remote resource via SSH), `file` (copy a file).
>
> **Why last resort:**
> - Break **idempotency** — running `plan` again shows no changes even if the script ran incorrectly
> - Failures mid-provisioner leave resources in partial state
> - Can't be tested with `plan`
>
> **Prefer instead:** EC2 `user_data` for bootstrapping, AWS Systems Manager for configuration, Docker images for app config.

---

**Q: Have you used HashiCorp Vault? How do you handle secrets without it?**

> I haven't used Vault directly — but I've implemented the same security model with AWS-native tools:

| Vault Concept | My AWS Equivalent |
|---|---|
| Secret storage | AWS Secrets Manager |
| Access policies | IAM policies (least-privilege per service account) |
| Vault agent (sidecar) | Secrets Store CSI Driver (ASCP) |
| Secret injection into pods | ASCP mounts secrets as files/env vars |
| Secret rotation | Secrets Manager automatic rotation with Lambda |
| Audit log | CloudTrail — every `GetSecretValue` logged |

> The concepts are identical — centralized storage, access control, auditing, rotation. I can pick up Vault quickly given this strong foundation.

---

## 9. Linux & Shell — Q&A

---

**Q: What Linux administration tasks do you perform in your DevOps work?**

> Day-to-day Linux tasks:
> - **EC2 management** — SSH access, user data scripts for bootstrapping, `systemctl` to manage services
> - **Log analysis** — `tail -f`, `journalctl -u`, `grep`/`awk` for parsing; CloudWatch agent for log forwarding
> - **File permissions** — `chmod 400` for PEM keys, `600` for SSH keys, `755` for scripts
> - **Process management** — `ps aux`, `kill`, `top`/`htop` for CPU/memory spikes
> - **Disk management** — `df -h`, `du -sh *`, `docker system prune` to free space
> - **Networking** — `curl`, `telnet`, `netstat`/`ss`, `nslookup` for connectivity and DNS checks
> - **Bash scripting** — automating deployments, health checks, cron jobs

---

**Q: What are the most used Linux commands in your DevOps work?**

| Command | Purpose | Key Flags / Example |
|---|---|---|
| `grep` | Search patterns in files/output | `-i` case-insensitive, `-r` recursive, `-v` exclude, `-n` line number |
| `awk` | Column extraction, text processing | `awk '{print $1}'` first col, `awk '{print $NF}'` last col |
| `sed` | Stream edit — find and replace | `sed 's/old/new/g' file.txt` |
| `tail` | View end of file / live logs | `tail -f /var/log/app.log` (follow live), `tail -100` last 100 lines |
| `find` | Find files by name, size, time | `find / -name "*.log" -mtime -7 -size +100M` |
| `ps aux` | List all running processes | `ps aux \| grep nginx` |
| `kill` | Send signal to process | `kill -9 <pid>` (SIGKILL), `kill -15 <pid>` (SIGTERM graceful) |
| `df -h` | Disk space at filesystem level | Shows mount points and usage % |
| `du -sh` | Disk usage of a directory | `du -sh *` → size of each item in current dir |
| `netstat` / `ss` | Network connections and listeners | `ss -tlnp` → active TCP listeners with process |
| `curl` | HTTP requests from CLI | `curl -I https://url` (headers only), `curl -o file url` |
| `chmod` | File permissions | `chmod 755 script.sh`, `chmod 400 key.pem` |
| `chown` | File ownership | `chown ec2-user:ec2-user /app` |
| `systemctl` | Manage systemd services | `start`, `stop`, `restart`, `status`, `enable`, `disable` |
| `journalctl` | View systemd service logs | `journalctl -u nginx -f` (follow), `-n 100` last 100 lines |
| `crontab` | Schedule recurring tasks | `crontab -e` → `0 2 * * * /scripts/backup.sh` |
| `scp` | Secure file copy over SSH | `scp -i key.pem file ec2-user@ip:/path` |

---

**Q: What does `chmod 755`, `644`, `600`, `400` mean?**

> Octal notation: **Owner | Group | Others** — 4=read, 2=write, 1=execute

| chmod | Owner | Group | Others | Symbolic | Use Case |
|---|---|---|---|---|---|
| `755` | rwx (7) | r-x (5) | r-x (5) | `-rwxr-xr-x` | Scripts, directories |
| `644` | rw- (6) | r-- (4) | r-- (4) | `-rw-r--r--` | Regular config files |
| `600` | rw- (6) | --- (0) | --- (0) | `-rw-------` | SSH private keys, secrets |
| `400` | r-- (4) | --- (0) | --- (0) | `-r--------` | AWS `.pem` key files — read-only even for owner |
| `700` | rwx (7) | --- (0) | --- (0) | `-rwx------` | Private scripts/directories |
| `777` | rwx (7) | rwx (7) | rwx (7) | `-rwxrwxrwx` | ⚠️ Never use — full access to everyone |

---

**Q: What does `2>&1` mean in shell commands?**

> Redirects **stderr** (file descriptor 2) to wherever **stdout** (file descriptor 1) is going.
>
> ```bash
> terraform apply > output.log 2>&1   # both stdout and stderr written to output.log
> command > /dev/null 2>&1            # discard all output — stdout + stderr to null
> ```
>
> File descriptors: `0` = stdin, `1` = stdout, `2` = stderr.

---

**Q: What is `systemctl` and how is it different from `service`?**

> - `systemctl` — the modern **systemd**-based service manager (RHEL 7+, Ubuntu 16+)
> - `service` — older SysV init compatibility wrapper — calls systemctl under the hood on modern systems
>
> ```bash
> systemctl start nginx          # start the service
> systemctl stop nginx           # stop the service
> systemctl restart nginx        # stop + start
> systemctl status nginx         # show status, last logs, PID
> systemctl enable nginx         # start on boot automatically
> systemctl disable nginx        # remove from boot
> journalctl -u nginx -f         # follow live logs for nginx
> journalctl -u nginx --since "1 hour ago"
> ```

---

**Q: How do you find all files modified in the last 7 days?**

```bash
find /var/log -type f -mtime -7
find /app -name "*.log" -mtime -7 -size +100M    # logs > 100MB, modified last 7 days
find / -type f -size +500M 2>/dev/null            # files larger than 500MB, suppress permission errors
```

---

**Q: How do you search for a string recursively across all files?**

```bash
grep -r "db_password" /etc/                        # recursive, case-sensitive
grep -ri "error" /var/log/                         # recursive, case-insensitive
grep -rn "FAILED" . --include="*.log"              # with line numbers, only .log files
grep -v "health" /var/log/app.log                  # exclude lines containing "health"
```

---

**Q: How do you print specific columns from a file using `awk`?**

```bash
awk '{print $1}' file.txt          # first column
awk '{print $NF}' file.txt         # last column (NF = number of fields)
awk '{print $1, $3}' file.txt      # first and third columns
awk -F',' '{print $2}' file.csv    # second column from CSV (comma separator)
awk '/ERROR/ {print $0}' app.log   # print full lines containing ERROR
```

---

**Q: How do you kill a process by name (not PID)?**

```bash
pkill nginx                         # send SIGTERM to all processes named nginx
pkill -9 nginx                      # force kill (SIGKILL)
killall nginx                       # kill all instances by name
kill -9 $(pgrep nginx)              # get PID first, then kill
kill -9 $(lsof -t -i:8080)         # kill process listening on port 8080
```

---

**Q: You lost your EC2 PEM file. Can you recover it? How do you still connect?**

> **No — you cannot recover the private key.** AWS only stores the public key. The private key is given once at creation and never stored by AWS.
>
> **Option 1 — AWS Session Manager (recommended — no PEM, no port 22):**
> ```bash
> aws ssm start-session --target i-0abc123def456
> ```
> Requirements: SSM Agent running on EC2 + `AmazonSSMManagedInstanceCore` IAM role attached.
>
> **Option 2 — EC2 Instance Connect (Amazon Linux / Ubuntu only):**
> AWS Console → EC2 → select instance → Connect → EC2 Instance Connect → browser-based shell. AWS injects a temporary key for 60 seconds.
>
> **Option 3 — Generate a new SSH key pair (permanent fix):**
> ```bash
> # On your local machine
> ssh-keygen -t rsa -b 4096 -f ~/.ssh/new_ec2_key
> cat ~/.ssh/new_ec2_key.pub          # copy this public key
>
> # Inside the instance (via Session Manager or Instance Connect):
> echo "ssh-rsa AAAA..." >> ~/.ssh/authorized_keys
> chmod 600 ~/.ssh/authorized_keys
> chmod 700 ~/.ssh
>
> # Back on local machine
> chmod 400 ~/.ssh/new_ec2_key
> ssh -i ~/.ssh/new_ec2_key ec2-user@<public-ip>
> ```

| Method | Requires PEM | Requires Port 22 | Best For |
|---|---|---|---|
| Session Manager | No | No | Quick access, any instance with SSM agent |
| EC2 Instance Connect | No | Yes | Amazon Linux / Ubuntu only |
| New key + authorized_keys | No | Yes | Permanent fix via Session Manager first |

---

**Q: What is `~/.ssh/authorized_keys` and how does SSH use it?**

> `authorized_keys` is a file on the **server** that lists all public keys that are allowed to SSH in.
>
> When you SSH:
> 1. Client presents its **public key** to the server
> 2. Server checks if that public key exists in `~/.ssh/authorized_keys`
> 3. Server sends a challenge encrypted with your public key
> 4. Client proves it has the matching **private key** by decrypting and responding
> 5. Authentication succeeds — no password needed
>
> **Required permissions:**
> ```bash
> chmod 700 ~/.ssh                    # only owner can read/write/execute
> chmod 600 ~/.ssh/authorized_keys    # only owner can read/write
> ```
> SSH **refuses to authenticate** if these permissions are too open.

---

**Q: What is the difference between `df -h` and `du -sh`?**

> - `df -h` — shows **filesystem-level disk space** — how much of each mounted partition is used/free
>   ```bash
>   df -h                   # all filesystems
>   df -h /var/log          # just the filesystem containing /var/log
>   ```
> - `du -sh` — shows **actual disk usage of a directory/file**
>   ```bash
>   du -sh /var/log         # total size of /var/log directory
>   du -sh *                # size of each item in current directory
>   du -sh * | sort -rh | head -10  # top 10 largest items
>   ```
>
> Use `df` to check if a disk is full. Use `du` to find what is filling the disk.

---

**Q: How do you follow live logs from a service?**

```bash
tail -f /var/log/nginx/access.log                # follow live — any log file
journalctl -u nginx -f                           # follow systemd service logs
kubectl logs -f <pod> -n <namespace>             # follow Kubernetes pod logs
kubectl logs -f <pod> --previous                 # logs from crashed container
kubectl logs -l app=catalog -f --tail=100        # all catalog pods, last 100 lines
```

---

**Q: How do you copy files securely between two Linux machines?**

```bash
# scp — simple, single file or directory
scp -i ~/.ssh/key.pem file.txt ec2-user@10.0.1.5:/home/ec2-user/

# rsync — better for directories, only syncs changed files
rsync -avz -e "ssh -i key.pem" ./local-dir ec2-user@10.0.1.5:/remote-dir/

# sftp — interactive secure file transfer
sftp -i key.pem ec2-user@10.0.1.5
```

---

**Q: What is the difference between Terraform and Ansible for configuration management?**

| Aspect | Terraform | Ansible |
|---|---|---|
| Primary use | Infrastructure **provisioning** — create VMs, VPCs, K8s clusters | **Configuration management** — install packages, configure OS, deploy files |
| Approach | Declarative — define desired state | Declarative playbooks / imperative tasks |
| State | Maintains state file | Stateless — checks state on each run |
| Agent | Agentless | Agentless (SSH-based) |
| Target | Cloud resources (AWS API) | OS-level (files, packages, services) |

> They complement each other: **Terraform creates the EC2 instance**, **Ansible configures it** (installs Nginx, deploys app config). In my projects I used Terraform for all provisioning and kept configuration management inside **Docker images** (Dockerfiles) rather than Ansible — containers are the config.

---

**Q: How do you check what is listening on a specific port?**

```bash
ss -tlnp | grep 8080          # modern — show TCP listeners, with process name
netstat -tlnp | grep 8080     # older equivalent
lsof -i :8080                  # show process using port 8080
fuser 8080/tcp                 # show PID using the port
```

---

**Q: How do you add a cron job? Write one that runs a backup script every day at 2 AM.**

```bash
crontab -e                     # open cron editor for current user
```

```
# m  h  dom  mon  dow  command
  0  2   *    *    *   /scripts/backup.sh >> /var/log/backup.log 2>&1
```

> Cron format: `minute hour day-of-month month day-of-week command`
> `>> /var/log/backup.log 2>&1` — append both stdout and stderr to log file

---

## 10. Git — Q&A

---

**Q: What is the difference between `git merge` and `git rebase`?**

> Both integrate changes from one branch into another — but they do it differently:

| Aspect | `git merge` | `git rebase` |
|---|---|---|
| History | Non-linear — creates a **merge commit** | Linear — rewrites commits on top of target branch |
| Commit integrity | Preserves all original commits | Replays commits as new commits (different SHA) |
| Safety on shared branches | ✅ Safe — never rewrites history | ❌ Unsafe — never rebase a branch others are working on |
| Use case | Merging feature into `main` via PR | Cleaning up a feature branch before raising PR |

```bash
# merge — creates a merge commit, preserves full history
git checkout main
git merge feature/catalog-fix

# rebase — replays feature commits on top of main (linear history)
git checkout feature/catalog-fix
git rebase main
git checkout main
git merge feature/catalog-fix   # now a fast-forward — no merge commit
```

> **Golden rule:** Never rebase a branch that others are working on — it rewrites commit SHAs and causes conflicts for everyone else. Rebase only your own local/private branches.

---

**Q: What is `git stash` and when do you use it?**

> Temporarily saves **uncommitted changes** (both staged and unstaged) without making a commit — lets you switch context cleanly.

```bash
git stash                        # save current changes with auto-generated message
git stash save "WIP: catalog fix"  # save with a descriptive message
git stash list                   # see all stashes
git stash pop                    # apply most recent stash AND remove it from stash list
git stash apply stash@{1}        # apply a specific stash but KEEP it in the list
git stash drop stash@{0}         # delete a specific stash
git stash clear                  # delete all stashes
```

> **Real use case:** You're mid-way through a feature and your team lead asks you to urgently check a production bug on `main`. Stash your changes → switch to `main` → investigate → come back → `git stash pop`.

---

**Q: How do you revert a commit that has already been pushed to remote?**

```bash
# SAFE — creates a new commit that undoes the changes (preserves history)
git revert <commit-sha>
git push

# NEVER DO THIS on shared/main branches — rewrites history
git reset --hard <commit-sha>
git push --force   # ❌ dangerous on shared branches — destroys others' work
```

> Always use `git revert` on shared branches — it's safe because it adds a new commit rather than rewriting history. Use `git reset` only on your own local private branches.

---

**Q: What is the difference between `git reset --soft`, `--mixed`, and `--hard`?**

| Flag | Commits | Staging Area (Index) | Working Directory | Changes Lost? |
|---|---|---|---|---|
| `--soft` | Undone | Unchanged — changes remain staged | Unchanged | No |
| `--mixed` (default) | Undone | Cleared — changes moved back to working dir | Unchanged | No |
| `--hard` | Undone | Cleared | Cleared — **changes deleted** | **Yes — irreversible** |

```bash
git reset --soft HEAD~1     # undo last commit, keep changes staged — ready to re-commit
git reset --mixed HEAD~1    # undo last commit, keep changes in working dir — need to re-add
git reset --hard HEAD~1     # undo last commit, DELETE all changes — use with caution
```

> `--hard` is destructive. Only use it when you intentionally want to discard changes entirely. Never on commits already pushed to shared branches.

---

**Q: What is `git cherry-pick`? When do you use it?**

> Applies a **specific commit** from one branch onto the current branch — without merging the entire branch.

```bash
git log feature/orders --oneline        # find the commit SHA you want
git checkout main
git cherry-pick a1b2c3d                 # apply that single commit to main
git cherry-pick a1b2c3d..e5f6g7h       # apply a range of commits
```

> **Real use cases:**
> - **Hotfix cherry-pick**: a bug fix is committed on `main` — cherry-pick it to the `release/1.2` branch so the release gets the fix without taking all of `main`
> - **Wrong branch commit**: you committed on `main` instead of `feature/catalog` — cherry-pick to the feature branch, then `git reset --hard` on `main`

---

**Q: What is a detached HEAD state?**

> `HEAD` normally points to a **branch**. In detached HEAD state, `HEAD` points directly to a **commit** — not a branch.
>
> You enter detached HEAD when you `git checkout <commit-sha>` or check out a tag.
>
> ```bash
> git checkout a1b2c3d     # detached HEAD — viewing history
> git log --oneline        # no branch name shown next to HEAD
> ```
>
> **Risk:** any commits made in detached HEAD state will be lost when you switch branches — they're unreachable.
>
> **Fix:** create a branch immediately to save your work:
> ```bash
> git checkout -b hotfix/from-old-commit    # create branch from current detached HEAD position
> ```

---

**Q: What is the difference between `git clone` and `git fork`?**

| | `git clone` | `git fork` |
|---|---|---|
| Where copy lives | Your **local machine** | Your **GitHub account** (server-side copy) |
| Purpose | Work on any repo locally | Contribute to someone else's repo |
| Command | `git clone <url>` | GitHub UI — "Fork" button |
| Push access | Depends on repo permissions | You always can — it's your copy |

> **Open-source contribution workflow:**
> ```bash
> # 1. Fork repo on GitHub → github.com/shrisha/upstream-repo
> # 2. Clone your fork locally
> git clone https://github.com/shrisha/upstream-repo.git
> # 3. Make changes, push to your fork
> git push origin feature-branch
> # 4. Open Pull Request → your fork → original repo
> ```

---

**Q: How do you keep your fork in sync with the upstream repository?**

```bash
# Add upstream remote once
git remote add upstream https://github.com/original-owner/repo.git

# Sync your fork with upstream changes
git fetch upstream
git checkout main
git merge upstream/main        # or: git rebase upstream/main for clean history
git push origin main           # update your fork on GitHub
```

---

**Q: What is `git bisect` and when do you use it?**

> `git bisect` uses **binary search** to find the exact commit that introduced a bug — without manually checking each commit.

```bash
git bisect start
git bisect bad                  # current commit is broken
git bisect good v1.2.0          # last known good commit/tag

# Git checks out a middle commit — you test it:
# If broken:
git bisect bad
# If working:
git bisect good
# Git narrows down — repeat until it identifies the exact culprit commit

git bisect reset                # return to HEAD when done
```

> Especially useful when the bug was introduced somewhere in the last 100 commits — bisect finds it in ~7 steps (log₂100 ≈ 7).

---

**Q: What is the difference between `git fetch` and `git pull`?**

> - `git fetch` — downloads changes from remote but does **not** merge them into your local branch. Safe — lets you inspect before merging.
> - `git pull` — `git fetch` + `git merge` (or rebase) in one step. Immediately integrates remote changes.

```bash
git fetch origin                    # download — inspect with git log origin/main
git merge origin/main               # then manually merge when ready

git pull origin main                # fetch + merge in one step
git pull --rebase origin main       # fetch + rebase (cleaner linear history)
```

> Best practice in team environments: `git fetch` first → `git log origin/main` → review changes → then merge. Avoids surprise conflicts.

---

**Q: What is the difference between `git reset` and `git revert`?**

| | `git reset` | `git revert` |
|---|---|---|
| What it does | Moves HEAD backward — **removes commits** from history | Creates a **new commit** that undoes the changes |
| History | Rewrites history — SHAs change | Preserves all history — adds a new commit |
| Safe for shared branches | ❌ No — forces others to re-sync | ✅ Yes — non-destructive |
| Use case | Clean up local branch before pushing | Undo a commit already pushed to main/remote |

---

**Q: What is a Git hook? Give a real use case.**

> Git hooks are scripts that run automatically at specific points in the Git workflow — stored in `.git/hooks/`.

| Hook | When it runs | Use case |
|---|---|---|
| `pre-commit` | Before a commit is created | Run linting, `terraform fmt`, detect-secrets scan |
| `commit-msg` | After commit message is written | Enforce Jira ticket format (`PROJ-123: message`) |
| `pre-push` | Before `git push` | Run unit tests locally — prevent pushing broken code |
| `post-merge` | After a merge completes | Auto-run `npm install` or `terraform init` |

> I use `pre-commit` hooks with **detect-secrets** to block commits containing hardcoded API keys or passwords before they ever reach the remote repo.

---

**Q: What is `git squash`? When do you use it?**

> Squash combines multiple commits into a single clean commit — used to clean up messy "WIP" commits before merging a PR.

```bash
# Interactive rebase — squash last 3 commits into one
git rebase -i HEAD~3
# In the editor: change 'pick' to 'squash' (or 's') for commits to combine
# → prompts for a new combined commit message

# Or: squash and merge in GitHub PR UI — "Squash and merge" button
```

> Only squash on **your own feature branch** before merging — never squash commits that are already on `main` or shared branches.

---

**Q: What is `git log` and how do you use it effectively?**

```bash
git log --oneline                            # compact one-line per commit
git log --oneline --graph --all              # visual branch/merge graph
git log --oneline -10                        # last 10 commits
git log --author="Shrisha"                   # commits by specific author
git log --since="2 weeks ago"               # commits in last 2 weeks
git log --grep="fix"                         # commits with "fix" in message
git log --oneline main..feature/catalog      # commits in feature not yet in main
git log -p -- filename.py                    # show diffs for a specific file
```

---

**Q: What is `git remote`? What is the difference between `origin` and `upstream`?**

> `git remote` manages connections to remote repositories.

```bash
git remote -v                              # list all remotes with URLs
git remote add upstream <url>              # add a new remote named "upstream"
git remote remove origin                   # remove a remote
```

| Remote | Points To | Purpose |
|---|---|---|
| `origin` | Your fork or your repo on GitHub | Where you push your changes |
| `upstream` | The original repo you forked from | Where you pull updates from the source |

---

## 11. AWS Cloud Services — Q&A

---

**Q: What AWS services have you worked with?**

| Service | Category | Where Used |
|---|---|---|
| EKS | Managed Kubernetes | Capgemini |
| ECR | Container image registry | Capgemini |
| EC2 + ALB/NLB + ASG | Compute + load balancing | Both |
| RDS (MySQL, PostgreSQL) | Managed relational DB | Capgemini |
| DynamoDB | Managed NoSQL DB | Capgemini |
| ElastiCache Redis | Managed in-memory cache | Capgemini |
| SQS | Message queue | Capgemini (Checkout → Orders async) |
| SNS | Pub/sub notifications | Both (alerting) |
| S3 | Object storage | Both (state, artifacts, logs) |
| IAM | Identity and access management | Both |
| Secrets Manager | Secret storage and rotation | Capgemini |
| CloudWatch | Logs, metrics, alarms | Both |
| CloudTrail | API audit trail | Both |
| Route53 | DNS management | Both |
| ACM | SSL/TLS certificates | Both |
| VPC | Network isolation | Both |
| CodePipeline + CodeBuild | CI/CD for IaC | Infosys |
| X-Ray | Distributed tracing | Capgemini |
| Karpenter (via EC2) | Node autoscaling | Capgemini |

---

**Q: What is the difference between SQS and SNS?**

| Feature | SQS | SNS |
|---|---|---|
| Type | **Message queue** — point-to-point | **Pub/Sub** — one message to many subscribers |
| Consumers | One consumer processes each message | Multiple subscribers receive the same message |
| Message retention | Up to 14 days — survives if consumer is down | Not stored — if subscriber is down, message is lost |
| Delivery | Pull-based — consumer polls the queue | Push-based — SNS pushes to subscribers |
| Use case | Decoupling async workloads, rate limiting | Fan-out notifications, alerts, broadcasts |

> **In my project:** Checkout service puts an order message on an **SQS queue** → Orders service polls and processes it asynchronously. If Orders is temporarily down, messages stay in queue (up to 14 days) and are processed when it recovers.
>
> **SNS fan-out pattern (common combo):** SNS topic → multiple SQS queues — one publish, multiple independent consumers each get their own copy.

---

**Q: What is the difference between ALB and NLB?**

| Feature | ALB (Application LB) | NLB (Network LB) |
|---|---|---|
| OSI Layer | Layer 7 — HTTP/HTTPS/WebSocket | Layer 4 — TCP/UDP/TLS |
| Routing | Path-based, host-based, header-based, query string | IP + port only |
| SSL termination | Yes — terminates TLS, pods get HTTP | Yes — TLS termination or passthrough |
| Target types | Instance, IP, Lambda | Instance, IP, ALB |
| Latency | Slightly higher (L7 processing) | Ultra-low latency (L4 passthrough) |
| Static IP | No (DNS name only) | Yes — static Elastic IP per AZ |
| Use case | Web apps, REST APIs, microservices on EKS | Low-latency TCP workloads, IoT, gaming, VPN |
| WebSockets | ✅ Yes | ✅ Yes |
| gRPC | ✅ Yes | ✅ Yes |

> **In my project:** I use **ALB** with the AWS Load Balancer Controller for HTTPS ingress to all 5 microservices — path-based routing (`/catalog/*`, `/orders/*`), ACM certificate termination, HTTP→HTTPS redirect.
>
> **When to pick NLB:** client needs a static IP to whitelist in their firewall, extreme low-latency TCP workloads, or TLS passthrough (ALB must terminate TLS; NLB can pass it through to the pod).

---

**Q: What is the difference between a Security Group and a Network ACL (NACL)?**

| Feature | Security Group | Network ACL (NACL) |
|---|---|---|
| Level | **Instance** level (applied to ENI) | **Subnet** level (applied to entire subnet) |
| State | **Stateful** — return traffic automatically allowed | **Stateless** — must explicitly allow both inbound and outbound |
| Rules | Allow rules only — no deny | Allow AND Deny rules |
| Rule evaluation | All rules evaluated — most permissive wins | Rules evaluated **in order** by rule number — first match wins |
| Default (new VPC) | Deny all inbound, allow all outbound | Allow all inbound and outbound |
| Scope | Applies to specific EC2 / RDS / EKS pod | Applies to all resources in the subnet |

> **Real use case — layered security:**
> - **NACL**: block a known malicious IP range at the subnet level (denies all traffic before it hits any instance)
> - **Security Group**: allow port 443 from ALB security group to EKS node security group only
>
> **Stateful vs stateless example:** A security group allows inbound port 80 → return traffic on a random ephemeral port is automatically allowed. A NACL must have both an inbound rule (allow port 80) AND an outbound rule (allow ephemeral ports 1024–65535) for the same flow to work.

---

**Q: What is the difference between RDS and DynamoDB?**

| Feature | RDS | DynamoDB |
|---|---|---|
| Type | **Relational** — SQL | **NoSQL** — key-value + document |
| Schema | Fixed schema — tables, columns, foreign keys | Flexible — schemaless, per-item attributes |
| Query language | SQL | DynamoDB API (GetItem, Query, Scan) |
| Scaling | **Vertical** — scale up instance size | **Horizontal** — auto-scales transparently |
| Joins | Yes — complex joins supported | No native joins |
| Transactions | Full ACID | DynamoDB Transactions (limited, same region) |
| Latency | Milliseconds — depends on query complexity | Single-digit milliseconds — consistently fast |
| Engines supported | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server | — (proprietary) |
| Cost model | Per instance-hour + storage | Per request (on-demand) or provisioned capacity |

> **In my project:**
> - **RDS MySQL** (Catalog) — structured product data, SQL queries needed
> - **RDS PostgreSQL** (Orders) — ACID transactions for order placement and payment
> - **DynamoDB** (Cart) — high-throughput cart operations, flexible schema per user, auto-scales for peak shopping

---

**Q: What is the difference between S3, EBS, and EFS?**

| Feature | S3 | EBS | EFS |
|---|---|---|---|
| Type | **Object** storage | **Block** storage | **File** storage (NFS) |
| Access | HTTP/REST API from anywhere | Attached to one EC2/pod at a time | Mounted by many EC2/pods simultaneously |
| Durability | 11 nines (99.999999999%) | 99.999% | 99.999999999% |
| Latency | Higher (object API overhead) | Low (block, like a local disk) | Moderate (network file system) |
| Use case | Artifacts, backups, static assets, Terraform state | Database data volumes, EKS PVC (ReadWriteOnce) | Shared config files, shared storage across pods |
| K8s access mode | Not a volume (API access) | ReadWriteOnce (one node) | ReadWriteMany (many nodes) |

> **I used:**
> - **S3** — Terraform remote state, ECR artifacts, CloudWatch log archive
> - **EBS** — EKS PVCs for microservices needing persistent local storage (via EBS CSI Driver)
> - **EFS** — not in my project, but would use it for shared storage across multiple pods

---

**Q: What is the difference between EC2 and Lambda?**

| Feature | EC2 | Lambda |
|---|---|---|
| Type | Virtual machine — always running | Serverless function — runs on demand |
| Management | You manage OS, patches, scaling, HA | AWS manages everything |
| Cold start | No cold start | Cold start latency (100ms – a few seconds) |
| Execution limit | No limit | 15 minutes max per invocation |
| Billing | Per hour/second (running or not) | Per 100ms of execution + invocations |
| Use case | Long-running apps, EKS worker nodes, databases | Event-driven short tasks — S3 triggers, API Gateway, cron jobs |

> **In my project:** I used EC2 (via EKS node groups + Karpenter) for all application workloads. Lambda would be used for: rotating secrets in Secrets Manager, processing S3 events, auto-remediation from CloudWatch alarms.

---

**Q: What is the difference between ECS and EKS?**

| Feature | ECS | EKS |
|---|---|---|
| Orchestrator | AWS proprietary (Tasks + Services) | Kubernetes (industry standard) |
| Learning curve | Lower — simpler concepts | Higher — full Kubernetes ecosystem |
| Ecosystem | AWS-only tools | Helm, ArgoCD, Karpenter, any CNCF tool |
| Fargate support | Yes — serverless containers | Yes — EKS Fargate profiles |
| Migration portability | AWS-specific — hard to migrate to other clouds | Kubernetes — same workloads run on GKE, AKS |
| Use case | Simpler AWS-native container workloads | Complex microservices, GitOps, multi-cluster |

> **I use EKS** — Kubernetes gives portability, a rich ecosystem (Helm, ArgoCD, Karpenter), and industry-standard tooling. If portability and advanced autoscaling matter, EKS is the right choice.

---

**Q: What is the difference between CloudWatch and CloudTrail?**

| Feature | CloudWatch | CloudTrail |
|---|---|---|
| Purpose | **Performance monitoring** — metrics, logs, alarms | **API audit trail** — who did what, when |
| Data | Metrics (CPU, request rate), logs, dashboards | API calls made to AWS services |
| Real-time | Yes — near real-time metrics | Near real-time with ~15 min delay to S3 |
| Use case | Set alarm if CPU > 80%, follow live logs | Audit who deleted an S3 bucket, who changed a SG |

> **CloudTrail real use:** if a Secrets Manager secret is accessed unexpectedly, CloudTrail logs the IAM identity, timestamp, source IP, and the exact API call (`GetSecretValue`) — critical for security investigations.

---

**Q: What is VPC peering and when would you use it?**

> VPC Peering creates a **private network connection between two VPCs** — traffic routes through AWS backbone, not the internet.
>
> - Works across regions and accounts
> - Not transitive — if VPC A peers with B, and B peers with C, A cannot reach C via B
> - Use cases: cross-account access (dev VPC → prod RDS), connecting to a shared services VPC, multi-region architecture
>
> **Alternatives:**
> - **AWS Transit Gateway** — hub-and-spoke model for connecting many VPCs; supports transitivity
> - **VPC endpoints** — private access to AWS services (S3, Secrets Manager) without leaving the VPC or using NAT Gateway

---

**Q: What is a NAT Gateway and why is it needed?**

> NAT Gateway allows resources in **private subnets** to initiate **outbound** internet traffic (download packages, call external APIs, pull ECR images) without being directly reachable from the internet.
>
> ```
> Private subnet (EKS node) → NAT Gateway (public subnet) → Internet Gateway → Internet
> Internet cannot initiate inbound connections to the private subnet
> ```
>
> Cost consideration: NAT Gateway charges per GB processed — for high-egress workloads (many ECR image pulls), use **VPC endpoints for ECR and S3** to bypass NAT Gateway entirely and save costs.

---

**Q: What is the difference between an IAM role, IAM user, and IAM policy?**

| Concept | Description | Use Case |
|---|---|---|
| **IAM User** | A permanent identity with long-term credentials (access key + secret) | Human users — best practice: use SSO instead |
| **IAM Role** | A temporary identity assumed by services, applications, or users | EC2, Lambda, EKS pods, GitHub Actions OIDC |
| **IAM Policy** | A JSON document defining allowed/denied actions on resources | Attached to users, groups, or roles |

> **Best practices I follow:**
> - No IAM users for applications — use roles only
> - OIDC for CI/CD — no static access keys in GitHub Actions
> - EKS Pod Identity — per-service-account role, not per-node
> - `least privilege` — only the permissions the workload actually needs

---

**Q: What is AWS Auto Scaling? How did you implement it?**

> AWS Auto Scaling automatically adjusts capacity based on demand — can scale EC2 instances, ECS tasks, or DynamoDB read/write capacity.
>
> **At Infosys (EC2-based):**
> - **Auto Scaling Group** with Launch Template — min/max/desired capacity
> - **Target Tracking policy** — maintain ALB request count per target at 1000 RPS
> - **CloudWatch alarm → Scale-out policy** — add instances when CPU > 70%
>
> **At Capgemini (EKS):**
> - **HPA** — scales pod replicas based on CPU/memory (Metrics Server)
> - **Karpenter** — scales EC2 nodes based on Pending pods — faster and more flexible than ASG-based Cluster Autoscaler

---

## 12. Networking Fundamentals — Q&A

---

**Q: What happens when you type `instagram.com` in a browser and press Enter?**

> This is a classic end-to-end networking question — covers DNS, TCP, TLS, HTTP, CDN.

| Step | What Happens | Protocol / Port |
|---|---|---|
| 1. Browser cache check | Browser checks its own DNS cache — if cached, skip step 2 | — |
| 2. DNS resolution | OS queries local DNS resolver → recursive resolver → root NS → `.com` NS → Instagram's NS → returns IP | UDP port 53 (TCP if response > 512 bytes) |
| 3. TCP handshake | Browser connects to Instagram's IP — SYN → SYN-ACK → ACK | TCP port 443 |
| 4. TLS handshake | Client Hello → Server Hello + Certificate → Key exchange → Finished — establishes encrypted session | TLS 1.3 |
| 5. HTTP request | Browser sends `GET / HTTP/2` over the encrypted TLS tunnel | HTTP/2 or HTTP/3 |
| 6. CDN routing | Request hits Instagram's CDN edge node (Cloudflare / Facebook PoP nearest to user) — static assets served from cache | — |
| 7. Response + render | HTML → browser parses → fetches CSS/JS/images (parallel, HTTP/2 multiplexing) → renders page | — |

---

**Q: What is the OSI model? Explain each layer briefly.**

| Layer | Name | What it does | Real Examples |
|---|---|---|---|
| 7 | Application | User-facing protocols — data formatting and communication | HTTP, HTTPS, DNS, SMTP, FTP, SSH |
| 6 | Presentation | Data encoding, encryption, compression | TLS/SSL, JPEG, ASCII, UTF-8 |
| 5 | Session | Establishes, manages, and terminates sessions | NetBIOS, RPC, SQL sessions |
| 4 | Transport | End-to-end delivery, segmentation, flow control, error recovery | TCP (reliable), UDP (fast, unreliable) |
| 3 | Network | Logical addressing, routing between networks | IP, ICMP (`ping`), routing protocols |
| 2 | Data Link | Physical addressing, error detection on local network | MAC addresses, Ethernet, Wi-Fi (802.11), ARP |
| 1 | Physical | Raw bit transmission over physical medium | Cables, fiber, NICs, hubs, repeaters |

> **DevOps relevance:**
> - ALB = Layer 7 (reads HTTP headers for routing)
> - NLB = Layer 4 (routes by IP + port only)
> - Security Groups = Layer 3/4 (IP + port rules)
> - VPC = Layer 3 (IP routing)

---

**Q: What is the difference between TCP and UDP?**

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented — 3-way handshake before data | Connectionless — sends without setup |
| Reliability | Guaranteed delivery — retransmits lost packets | No guarantee — packets can be lost or arrive out of order |
| Ordering | Packets delivered in order | No ordering guarantee |
| Speed | Slower (overhead for reliability) | Faster (no handshake, no ACKs) |
| Error checking | Yes — checksums + ACKs | Checksum only (no retransmit) |
| Use case | HTTP/HTTPS, SSH, databases, file transfer | DNS queries, video streaming, gaming, VoIP |

> **DevOps examples:**
> - `kubectl port-forward` → TCP
> - DNS resolution → UDP (port 53)
> - Prometheus scraping → HTTP/TCP
> - Kubernetes health check `tcpSocket` probe → TCP

---

**Q: Walk me through the TLS handshake step by step.**

> TLS (Transport Layer Security) establishes an encrypted channel before any HTTP data is sent.
>
> **TLS 1.3 handshake (simplified):**
> ```
> Client → Server:  ClientHello
>   (TLS version, supported cipher suites, random nonce, key share)
>
> Server → Client:  ServerHello + Certificate + CertificateVerify + Finished
>   (chosen cipher, server's public key cert signed by CA, encrypted with chosen key)
>
> Client:  Verifies certificate against trusted CA root store
>          Derives session keys from key exchange
>
> Client → Server:  Finished (encrypted)
>
> ← Encrypted HTTP traffic begins →
> ```
>
> **Key concepts:**
> - **Certificate** — server proves its identity; signed by a trusted CA (AWS ACM for us)
> - **Key exchange** — Diffie-Hellman (ECDHE) — client and server derive the same symmetric key without ever transmitting it
> - **Perfect Forward Secrecy** — each session has a unique key; past sessions cannot be decrypted even if private key is later compromised
>
> **In my project:** ACM certificates on ALB handle all TLS termination — pods receive plain HTTP inside the VPC.

---

**Q: What is the difference between HTTP/1.1, HTTP/2, and HTTP/3?**

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Transport | TCP | TCP | QUIC (UDP-based) |
| Multiplexing | ❌ One request per TCP connection (head-of-line blocking) | ✅ Multiple requests over one connection simultaneously | ✅ Even better — QUIC handles it per-stream |
| Header compression | ❌ None — headers resent in full each request | ✅ HPACK compression | ✅ QPACK compression |
| Server push | ❌ | ✅ Server can push resources before client asks | ✅ |
| Connection setup | TCP + TLS handshake = 2–3 round trips | TCP + TLS = 2–3 round trips | QUIC = 0–1 round trip (0-RTT reconnect) |
| Adoption | Universal | Most modern sites | Growing — YouTube, Google, Cloudflare |

> **DevOPS relevance:** ALB supports HTTP/2 between clients and the load balancer. HTTP/3 is not yet standard on ALB — handled at CDN (CloudFront) level.

---

**Q: What is DNS and how does it resolve a domain name?**

> DNS (Domain Name System) translates human-readable domain names (`catalog.myapp.com`) into IP addresses (`10.0.1.45`).
>
> **Resolution chain:**
> ```
> Browser checks local cache
>   → OS checks /etc/hosts
>   → OS queries local resolver (router / ISP DNS)
>   → Recursive resolver queries:
>       Root nameservers (.) → who handles .com?
>       .com nameservers → who handles myapp.com?
>       myapp.com authoritative NS (Route53) → what is catalog.myapp.com?
>       → returns IP
>   → Result cached at each level with TTL
> ```
>
> **In Kubernetes:** CoreDNS handles all internal DNS. `catalog.retail.svc.cluster.local` resolves to the ClusterIP of the catalog Service. Pods query CoreDNS at the cluster DNS IP (typically `10.96.0.10`).
>
> **In AWS:** Route53 is the authoritative DNS service. External DNS automatically creates/updates Route53 records when ALB Ingress resources are created in EKS.

---

**Q: What is the difference between a load balancer and a reverse proxy?**

| Feature | Load Balancer | Reverse Proxy |
|---|---|---|
| Primary purpose | Distribute traffic across multiple backend servers | Sit in front of servers — forward requests on their behalf |
| Health checks | Yes — routes only to healthy backends | Yes (if acting as LB too) |
| SSL termination | Often yes (ALB) | Yes (Nginx, HAProxy) |
| Caching | Rarely | Yes (Nginx, Varnish) |
| Compression | Rarely | Yes |
| Examples | AWS ALB, NLB, HAProxy | Nginx, Apache httpd, Envoy, Traefik |

> In practice: a reverse proxy like **Nginx** can also act as a load balancer. AWS ALB is both — it load-balances AND acts as a reverse proxy (terminates TLS, inspects HTTP headers). In Kubernetes, the Ingress controller (Nginx, Traefik) is a reverse proxy + load balancer.

---

**Q: What is CORS and why does it exist?**

> **CORS (Cross-Origin Resource Sharing)** is a browser security mechanism that blocks JavaScript on one domain from making API requests to a different domain — unless the server explicitly allows it.
>
> **Why:** Prevents malicious websites from making authenticated API calls to your bank/app using your stored cookies.
>
> **How it works:**
> ```
> Browser (app.mysite.com) → API (api.mysite.com):
>   Preflight: OPTIONS /api/orders
>   Server responds: Access-Control-Allow-Origin: https://app.mysite.com
>   Browser allows the actual request to proceed
> ```
>
> **DevOps relevance:** When ALB returns CORS headers for an API backend, the ALB or the application must include:
> ```
> Access-Control-Allow-Origin: https://app.mysite.com
> Access-Control-Allow-Methods: GET, POST, PUT
> Access-Control-Allow-Headers: Authorization, Content-Type
> ```
> Misconfigured CORS (`*` in production) is a security risk — allows any origin.

---

**Q: What is a CDN and how does it improve performance?**

> **CDN (Content Delivery Network)** — a globally distributed network of edge servers that cache and serve content from the location nearest to the user.
>
> **How it helps:**
> - **Reduced latency** — user in Chennai hits a Mumbai edge node, not a US-East origin server
> - **Reduced origin load** — static assets (JS, CSS, images) served from cache — origin only handles cache misses
> - **DDoS protection** — CDN absorbs attack traffic at the edge before it reaches your servers
> - **TLS offload** — TLS termination at the edge; origin gets plain HTTP
>
> **Examples:** AWS CloudFront, Cloudflare, Fastly, Akamai
>
> **In AWS setup:**
> ```
> User → CloudFront (edge — nearest PoP)
>   → Cache hit: returns immediately
>   → Cache miss: fetches from ALB/S3 origin, caches, returns
> ```
>
> **S3 + CloudFront** is the standard pattern for hosting static frontends — zero server management, global low latency.

---

**Q: What is the difference between a public IP and a private IP?**

> - **Public IP** — globally routable, reachable from the internet; assigned by IANA; e.g., `13.126.45.87`
> - **Private IP** — non-routable on the internet; used within private networks (VPC, LAN); RFC 1918 ranges:
>   - `10.0.0.0/8` — large organisations (VPCs)
>   - `172.16.0.0/12` — medium networks
>   - `192.168.0.0/16` — home/small office networks
>
> **In AWS VPC:**
> - EC2 in **public subnet** gets a public IP (via Internet Gateway) + private IP
> - EC2 in **private subnet** gets only a private IP — uses NAT Gateway for outbound internet
> - EKS pods get private VPC IPs assigned by the VPC CNI plugin — never public

---

**Q: What is NAT (Network Address Translation)?**

> NAT translates **private IPs to a public IP** for outbound internet traffic — allows many private resources to share one public IP.
>
> ```
> EKS node (10.0.1.50) → NAT Gateway (public subnet, Elastic IP: 52.90.x.x) → Internet
> Internet sees: request from 52.90.x.x (NOT the private IP)
> Return traffic: comes back to 52.90.x.x → NAT Gateway translates back to 10.0.1.50
> ```
>
> **Why it matters for DevOps:**
> - ECR image pulls from private subnets go via NAT Gateway (cost per GB)
> - Use **VPC endpoints** for ECR, S3, Secrets Manager to bypass NAT Gateway entirely — faster + cheaper
> - Security: inbound connections from internet cannot reach private subnet resources (NAT is one-way)

---

**Q: What is the difference between `ping`, `traceroute`, `curl`, and `telnet` for network debugging?**

```bash
ping 8.8.8.8                      # ICMP — checks if host is reachable, measures RTT
ping catalog.retail.svc.cluster.local   # check if Kubernetes DNS resolves the service

traceroute 8.8.8.8                # shows each network hop, identifies where latency occurs
traceroute -T -p 443 myapp.com    # TCP traceroute on port 443

curl -I https://myapp.com         # HTTP headers only — check response code, server headers
curl -v https://api.myapp.com/health  # verbose — shows TLS handshake + full request/response
curl -o /dev/null -w "%{time_total}\n" https://myapp.com  # measure response time

telnet myapp.com 443              # raw TCP connection test — checks if port 443 is reachable
telnet rds-endpoint 3306          # check if RDS is reachable from this server
```

> In Kubernetes debugging: `kubectl exec -it <pod> -- curl http://other-service/health` is the primary tool — tests both DNS resolution and HTTP connectivity from inside a pod.

---

## 13. Security & Secrets Management — Q&A


---

**Q: How do you prevent hardcoded secrets in your codebase?**

> Multiple layers working together — no single tool is enough:
>
> **1. SAST scanning in CI (shift-left):**
> ```yaml
> - name: Scan for secrets with Trivy
>   uses: aquasecurity/trivy-action@master
>   with:
>     scan-type: 'fs'
>     scan-ref: '.'
>     scanners: 'secret'
>     exit-code: '1'        # fail the pipeline if secrets found
> ```
>
> **2. Git pre-commit hook (local — before code even reaches GitHub):**
> ```bash
> pip install detect-secrets
> detect-secrets scan > .secrets.baseline
> # pre-commit hook runs detect-secrets on every commit attempt
> ```
>
> **3. `.gitignore` to exclude sensitive files:**
> ```
> .env
> *.pem
> secrets.yaml
> *credentials*
> terraform.tfvars    # if it contains real values
> ```
>
> **4. OIDC for CI/CD** — GitHub Actions never needs AWS access keys stored anywhere
>
> **5. Runtime** — EKS Pod Identity + Secrets Manager CSI Driver; secrets fetched at pod start from Secrets Manager — never in code, never in Docker images, never in Helm values

---

**Q: What is the difference between AWS Secrets Manager and AWS Parameter Store?**

| Feature | Secrets Manager | Parameter Store (SSM) |
|---|---|---|
| Cost | ~$0.40/secret/month + API call cost | Free (standard), paid (advanced) |
| Automatic rotation | ✅ Built-in — Lambda rotates secrets automatically | ❌ Manual or custom rotation |
| Secret size | Up to 64KB | 4KB (standard), 8KB (advanced) |
| Versioning | Yes — previous versions accessible | Yes |
| Cross-account access | Yes — resource-based policy | Yes |
| Best for | DB credentials, API keys needing auto-rotation | Config values, feature flags, non-sensitive params |
| KMS encryption | Default encryption | Optional encryption |

> **I use Secrets Manager** for all sensitive credentials — DB passwords, API keys, TLS certs — because of automatic rotation. For non-sensitive config (feature flags, environment names), Parameter Store works fine.

---

**Q: How does automatic secret rotation work in AWS Secrets Manager?**

> 1. You configure a **rotation schedule** (e.g., every 30 days) and a **rotation Lambda** on the secret
> 2. On rotation day, Secrets Manager invokes the Lambda
> 3. Lambda creates new credentials (e.g., new RDS password), updates the DB, updates the secret value
> 4. Secrets Manager sets the new secret as the current version — old version kept as `AWSPREVIOUS` for a brief overlap
> 5. Applications fetching the secret get the new value automatically on next fetch
>
> **With Secrets Store CSI Driver (ASCP):** the updated secret is re-mounted into pods on the next pod restart. For immediate rotation pickup without restarts, the ASCP can be configured to sync on a schedule.

---

**Q: A developer accidentally gains access to a production DB credential in Secrets Manager. What do you do?**

> **Immediate response — in this order:**
>
> 1. **Rotate the secret immediately** — invalidates the compromised credentials. New password applied to RDS, old one stops working within seconds.
> 2. **Revoke the developer's IAM access** — remove their ability to call `GetSecretValue` on production secrets
> 3. **Audit CloudTrail** — query for `GetSecretValue` events by that user/role in the last 30 days: what was accessed, from which IP, at what time?
>    ```bash
>    aws cloudtrail lookup-events \
>      --lookup-attributes AttributeKey=Username,AttributeValue=dev-username \
>      --start-time 2026-07-20 --end-time 2026-08-20
>    ```
> 4. **Check for lateral movement** — did they use the credentials to access the DB? Check RDS access logs, VPC flow logs
> 5. **Post-incident fix** — add a resource-based policy on the secret: `Allow GetSecretValue` only from specific IAM roles (the app's Pod Identity role), deny everything else
> 6. **Notify** — security team, incident report

---

**Q: How do you implement least-privilege access for IAM roles?**

> **Principles I apply:**
>
> 1. **Start with zero permissions** — add only what's explicitly needed, verified by testing
> 2. **Scope to specific resources** — never use `*` for resources if you can avoid it:
>    ```json
>    "Resource": "arn:aws:s3:::my-specific-bucket/*"
>    // NOT "Resource": "*"
>    ```
> 3. **Use EKS Pod Identity** — each microservice gets its own IAM role, not a shared node role. Cart service → DynamoDB only. Orders service → SQS + RDS only.
> 4. **Use condition keys** to restrict further:
>    ```json
>    "Condition": {
>      "StringEquals": {
>        "aws:SourceVpc": "vpc-abc123"    // only allow from within VPC
>      }
>    }
>    ```
> 5. **Audit regularly** — AWS IAM Access Analyzer identifies unused permissions and external access
> 6. **Enforce via AWS Config rules** — alert if any policy has `Action: *` or `Resource: *` in production
> 7. **SCPs (Service Control Policies)** — at the AWS Organizations level, block dangerous actions entirely (e.g., disable CloudTrail, create IAM users)

---

**Q: What is SAST? How do you use it in a CI/CD pipeline?**

> **SAST (Static Application Security Testing)** — scans source code and dependencies for security vulnerabilities **without running the application**. Catches issues at commit/PR time before they reach production.
>
> **Types of scans I run in GitHub Actions CI:**
>
> | Scan Type | Tool | What it catches |
> |---|---|---|
> | Secret detection | Trivy (`--scanners secret`), GitLeaks, detect-secrets | Hardcoded API keys, passwords, tokens |
> | Container image CVEs | Trivy (`--scanners vuln`) | Vulnerable packages in Docker images |
> | IaC misconfigurations | Trivy (`--scanners config`), Checkov | Terraform with public S3 buckets, SG open to 0.0.0.0/0 |
> | Dependency vulnerabilities | OWASP Dependency-Check, Snyk | Known CVEs in Maven/npm dependencies |
>
> ```yaml
> - name: Trivy full scan
>   uses: aquasecurity/trivy-action@master
>   with:
>     scan-type: 'fs'
>     scan-ref: '.'
>     scanners: 'vuln,secret,config'
>     severity: 'CRITICAL,HIGH'
>     exit-code: '1'           # block PR merge if CRITICAL found
>     format: 'sarif'
>     output: 'trivy-results.sarif'
>
> - name: Upload to GitHub Security tab
>   uses: github/codeql-action/upload-sarif@v3
>   with:
>     sarif_file: 'trivy-results.sarif'
> ```

---

**Q: How do you secure Kubernetes workloads beyond just IAM?**

> **Pod-level security context:**
> ```yaml
> securityContext:
>   runAsNonRoot: true          # must not run as root
>   runAsUser: 1001             # specific non-root UID
>   readOnlyRootFilesystem: true  # container cannot write to its own FS
>   allowPrivilegeEscalation: false  # process cannot gain more privileges
>   capabilities:
>     drop: ["ALL"]             # drop all Linux capabilities
>     add: ["NET_BIND_SERVICE"] # add back only what's needed
> ```
>
> **Namespace-level:**
> - **NetworkPolicy** — default deny all; allow only specific pod-to-pod communication
> - **ResourceQuota** — prevent one team's workload from starving others
> - **LimitRange** — enforce default CPU/memory requests on pods that don't specify them
>
> **Cluster-level:**
> - **RBAC** — least-privilege service accounts; no wildcard verb/resource permissions
> - **Pod Security Standards** (PSS) — enforce `restricted` profile on production namespaces (replaces deprecated PodSecurityPolicy)
> - **Admission controllers** — OPA Gatekeeper or Kyverno to enforce policies (e.g., block images from unverified registries, require resource limits)

---

**Q: How do you audit who accessed a secret in Secrets Manager?**

```bash
# CloudTrail — query for all GetSecretValue calls on a specific secret
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=ResourceName,AttributeValue=prod/db/password \
  --query 'Events[*].{Time:EventTime,User:Username,Source:SourceIPAddress,Event:EventName}'
```

> Every `GetSecretValue`, `PutSecretValue`, `RotateSecret`, `DeleteSecret` call is logged in CloudTrail with:
> - **IAM identity** (user, role, assumed role ARN)
> - **Timestamp**
> - **Source IP**
> - **AWS region**
>
> **CloudWatch alarm on suspicious access:**
> ```
> CloudTrail → CloudWatch Logs → Metric Filter (GetSecretValue on prod/*)
>   → CloudWatch Alarm (> 10 calls in 5 min from unexpected role)
>   → SNS → Slack/PagerDuty alert
> ```

---

**Q: What is AWS GuardDuty and how does it help?**

> GuardDuty is a **managed threat detection service** — continuously analyzes CloudTrail, VPC Flow Logs, and DNS logs using ML to detect suspicious patterns.
>
> What it detects:
> - Unusual API calls from unexpected IP / geo-location (credential compromise)
> - Port scanning or brute-force attempts against EC2
> - Communication with known malicious IPs or C2 servers
> - Cryptocurrency mining activity on EC2
> - Exfiltration of data from S3 (unusually high GetObject calls)
>
> **No agents, no config needed** — enable it in one click. Findings appear in Security Hub and trigger EventBridge rules for automated remediation.

---

**Q: How do you prevent a public S3 bucket from being created accidentally?**

> Multiple controls:
>
> **1. S3 Block Public Access at the Account level:**
> ```bash
> aws s3control put-public-access-block \
>   --account-id 123456789 \
>   --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
> ```
>
> **2. AWS Config rule** — `s3-bucket-public-read-prohibited` — auto-remediates or alerts if a bucket is made public
>
> **3. SCP (Service Control Policy)** at AWS Organizations level:
> ```json
> {
>   "Effect": "Deny",
>   "Action": ["s3:PutBucketAcl", "s3:PutBucketPolicy"],
>   "Condition": {
>     "StringEquals": {"s3:x-amz-acl": "public-read"}
>   }
> }
> ```
>
> **4. Terraform `lifecycle { prevent_destroy = true }`** + bucket policy that denies public access in IaC

---

**Q: What is the difference between encryption at rest and encryption in transit?**

> - **Encryption at rest** — data encrypted when stored on disk; even if someone steals the storage medium, data is unreadable without the key
>   - S3 SSE-S3, SSE-KMS; EBS volume encryption; RDS storage encryption; DynamoDB encryption; Secrets Manager KMS
>
> - **Encryption in transit** — data encrypted while moving over the network; prevents man-in-the-middle interception
>   - TLS/HTTPS for all ALB traffic (ACM certificate); TLS for RDS connections; HTTPS for ECR/Secrets Manager API calls; KMS API calls always over TLS
>
> **In my project:** both enforced everywhere — ALB terminates TLS (HTTPS only, HTTP→HTTPS redirect), EBS volumes encrypted with KMS, Secrets Manager state encrypted with KMS, RDS with storage encryption + TLS in transit, S3 state bucket with SSE-KMS.

---

**Q: What is OWASP Top 10? Which ones are most relevant to your DevOps work?**

> OWASP Top 10 lists the most critical web application security risks.
>
> **Most relevant to DevOps:**
>
> | OWASP Risk | DevOps Relevance | How I address it |
> |---|---|---|
> | **A01 — Broken Access Control** | Over-privileged IAM roles, public S3 buckets | Pod Identity least-privilege, S3 block public access, RBAC |
> | **A02 — Cryptographic Failures** | HTTP instead of HTTPS, unencrypted secrets | ACM + HTTPS on ALB, KMS encryption, Secrets Manager |
> | **A05 — Security Misconfiguration** | Open SGs, public subnets for DBs, default passwords | Private subnets, SG allow-list only, Trivy config scan in CI |
> | **A06 — Vulnerable Components** | CVEs in Docker base images, npm/Maven dependencies | Trivy image scan + ECR enhanced scanning on every build |
> | **A09 — Security Logging Failures** | No audit trail for secret access or infra changes | CloudTrail + CloudWatch alarms on sensitive API calls |

---

## 14. Scenario / Troubleshooting — Q&A

---

**Q: Your application is returning 500 errors in production. Walk through your complete investigation.**

> **Framework: start from the outside in — user → LB → pod → app → dependency**
>
> **Step 1 — ALB / Ingress level**
> ```bash
> # Check ALB metrics in CloudWatch — is the error rate high at LB level too?
> # ALB → HTTPCode_Target_5XX_Count rising? Or HTTPCode_ELB_5XX (LB itself failing)?
> ```
>
> **Step 2 — Kubernetes pod health**
> ```bash
> kubectl get pods -n retail                           # any CrashLoopBackOff / OOMKilled?
> kubectl get events -n retail --sort-by='.lastTimestamp' | tail -20
> kubectl rollout status deployment/catalog -n retail  # is rollout stuck?
> ```
>
> **Step 3 — Pod logs**
> ```bash
> kubectl logs -l app=catalog -n retail --tail=100
> kubectl logs <pod> --previous -n retail              # if pod restarted — logs from crashed container
> ```
>
> **Step 4 — X-Ray traces**
> - Open X-Ray Service Map — which service is red (high error %)?
> - Drill into a failing trace — which span has the error? Is it the DB call, external API, or app logic?
>
> **Step 5 — CloudWatch Logs Insights**
> ```sql
> fields @timestamp, @message
> | filter level = "ERROR" or level = "FATAL"
> | sort @timestamp desc
> | limit 50
> ```
>
> **Step 6 — Dependency check**
> ```bash
> kubectl exec -it <catalog-pod> -n retail -- \
>   mysql -h <rds-endpoint> -u catalog_user -p       # can pod reach the DB?
> kubectl exec -it <pod> -- curl http://other-service/health  # can pod reach other services?
> ```
>
> **Step 7 — Resource constraints**
> ```bash
> kubectl top pods -n retail                  # CPU/memory usage
> kubectl describe pod <pod> -n retail        # check OOMKilled, resource limits
> ```
>
> **Step 8 — Recent changes**
> - Check ArgoCD — was there a recent sync? What changed?
> - `git log --oneline -10` on the GitOps repo — who updated the image tag?
> - Was there a secret rotation? Is the new secret synced to the pod?

---

**Q: A pod is stuck in Pending state. What do you check?**

> `kubectl describe pod <pod>` — **Events section** always tells you the exact reason:
>
> | Event Message | Root Cause | Fix |
> |---|---|---|
> | `0/3 nodes are available: insufficient cpu` | Pod requests more CPU than any node has free | Scale up nodes or reduce `resources.requests.cpu` |
> | `0/3 nodes are available: insufficient memory` | Same for memory | Increase node size or reduce memory request |
> | `did not match node selector` | Pod has `nodeSelector` that no node satisfies | Check `nodeSelector` labels match a real node label |
> | `had taint that pod didn't tolerate` | Node has taint, pod has no toleration | Add `tolerations` to pod spec |
> | `pod has unbound PersistentVolumeClaims` | PVC is Pending — no PV available | `kubectl get pvc` — check StorageClass, EBS quota |
> | `pulling image ... pull access denied` | ECR auth failure — node can't pull image | Check node IAM role has `ecr:GetAuthorizationToken` |
>
> ```bash
> kubectl describe pod <pod> -n <namespace>        # read Events section
> kubectl get pvc -n <namespace>                   # check PVC status if volume-related
> kubectl get nodeclaim -A                         # check if Karpenter is provisioning a node
> kubectl get nodes                                # any nodes in Ready state?
> ```

---

**Q: A pod is in CrashLoopBackOff. How do you debug it?**

> ```bash
> kubectl logs <pod> -n <namespace>                # current container logs
> kubectl logs <pod> --previous -n <namespace>     # logs from the crashed container
> kubectl describe pod <pod> -n <namespace>        # look at Exit Code + Events
> ```
>
> **Exit Code decision tree:**
>
> | Exit Code | Meaning | Next Action |
> |---|---|---|
> | `1` | Application error — non-zero exit | Check app logs for exception/stacktrace |
> | `137` | OOMKilled — process exceeded memory limit | Increase `resources.limits.memory` or fix memory leak |
> | `139` | Segmentation fault | Native code crash — check native library versions |
> | `2` | Misuse of shell builtin | Check `ENTRYPOINT`/`CMD` in Dockerfile |
>
> **Common causes:**
> - Missing env var or wrong value — `kubectl exec` and check `printenv`
> - DB unreachable — connection refused at startup
> - Missing secret — Secrets Store CSI Driver not syncing properly
> - Port already in use inside the container
> - Startup too slow — liveness probe fires before app is ready → add `startupProbe`

---

**Q: New nodes joined the cluster but are in NotReady state. What do you check?**

> ```bash
> kubectl describe node <node-name>       # read Conditions and Events
> kubectl get events -A --sort-by='.lastTimestamp' | grep -i "node\|kubelet"
>
> # Check bootstrap logs via SSM (no SSH needed)
> aws ssm start-session --target <instance-id>
> sudo cat /var/log/cloud-init-output.log   # did user data script fail?
> sudo systemctl status kubelet             # is kubelet running?
> sudo journalctl -u kubelet -n 50          # kubelet logs
> ```
>
> | Symptom in `kubectl describe node` | Likely Cause |
> |---|---|
> | `KubeletNotReady: runtime network not ready` | VPC CNI plugin not started — check DaemonSet pods |
> | `CSINode does not exist` | EBS CSI Driver not ready on this node |
> | `node lease expired` | Network issue — node can't reach API server |
> | All conditions `Unknown` | Node lost connectivity to control plane — check SG rules (port 443) |
>
> **Most common causes:**
> - IAM role missing policy (`AmazonEKS_CNI_Policy` most often forgotten)
> - Security group not allowing node → control plane on port 443
> - Subnet ran out of IP addresses (VPC CNI assigns a VPC IP per pod)
> - User data script failed — incompatible AMI with cluster version

---

**Q: You try to increase node count and it's failing. How do you troubleshoot?**

> **With Karpenter:**
> ```bash
> kubectl get nodeclaim -A                          # see NodeClaim status
> kubectl describe nodeclaim <name>                 # events — why provisioning failed?
> # Common: Spot capacity not available → Karpenter tries next instance type
> # If all fail: check NodePool instance-type list is broad enough
> ```
>
> **With Managed Node Group (Terraform):**
> ```bash
> # Check ASG Activity tab in AWS Console
> aws autoscaling describe-scaling-activities \
>   --auto-scaling-group-name <asg-name> \
>   --max-items 10
> ```
>
> | Symptom | Likely Cause |
> |---|---|
> | EC2 not launching | Spot capacity unavailable — try more instance types |
> | EC2 launches but node stays `NotReady` | IAM, kubelet, or VPC CNI issue (see above) |
> | ASG won't scale past max | `max_size` in Terraform too low — update and apply |
> | Subnet exhausted | `/24` gives 251 IPs — for 50 pods-per-node × 10 nodes need larger CIDR |
> | Launch template error | AMI ID invalid or incompatible with EKS version |

---

**Q: How do you handle a sudden 10x traffic spike?**

> **Our automated response (no human intervention needed):**
> ```
> Traffic spike hits ALB
>   → ALB distributes to running pods
>   → CPU rises above HPA threshold (70%)
>   → HPA scales pod replicas up (e.g., 3 → 15)
>   → New pods go Pending (insufficient node capacity)
>   → Karpenter sees Pending pods
>   → Karpenter calls EC2 API directly → new node ready in < 60 seconds
>   → Pods schedule → traffic served
>   → Spike subsides → HPA scales pods down
>   → Karpenter consolidates underutilized nodes → cost back to normal
> ```
>
> **If spike exceeds Karpenter NodePool CPU limit:**
> - CloudWatch alarm fires: `KarpenterNodePoolsAtLimit`
> - On-call alert → engineer raises `NodePool.limits.cpu` in Terraform → `terraform apply` → Karpenter can provision more
>
> **Pre-planned spikes (product launches, sales events):**
> - Product team gives advance notice → run k6 load test to validate capacity
> - Pre-scale HPA `minReplicas` higher before the event
> - Set HPA threshold lower (60% instead of 70%) for more headroom

---

**Q: The CI/CD pipeline passed but the app is down. How do you investigate?**

> Pipeline green = build + deploy **succeeded**. It does NOT guarantee runtime health.
>
> ```bash
> kubectl get pods -n retail                              # check pod status
> kubectl describe pod <pod> -n retail                   # Events — exact failure reason
> kubectl logs <pod> --previous -n retail                # crashed container logs
> kubectl get events -n retail --sort-by='.lastTimestamp'
> kubectl rollout status deployment/<name> -n retail     # is rollout stuck mid-way?
> ```
>
> **Common causes when pipeline passes but app is down:**
>
> | Cause | How to Detect |
> |---|---|
> | Bad config/env var in new image | App logs show config exception at startup |
> | OOMKilled — new version uses more memory | `kubectl describe pod` shows OOMKilled |
> | DB migration failed — new version needs schema change | App fails with SQL error |
> | Readiness probe failing — app starts but not healthy | Pod Running but not Ready — check probe endpoint |
> | Secret not rotated — new code expects updated secret | Auth failure in app logs |
> | Dependency (other service) is down | X-Ray trace shows failure at downstream service |

---

**Q: How do you ensure zero downtime during a deployment?**

> ```yaml
> strategy:
>   type: RollingUpdate
>   rollingUpdate:
>     maxUnavailable: 0      # never go below desired replica count
>     maxSurge: 1            # one extra pod during rollout
> ```
>
> - `readinessProbe` — new pod only gets traffic once it passes; old pods kept alive until new ones are ready
> - `PodDisruptionBudget` — `minAvailable: 2` — prevents cluster events (node drain, Karpenter consolidation) from taking down too many pods simultaneously
> - **Pre-upgrade Helm hooks** — DB migration Job runs before pod rollout; pod won't start until migration Job completes successfully
> - **Feature flags** — decouple code deployment from feature activation; risky features behind a flag
> - **ArgoCD progressive rollout** — pause sync after deploying to 20% of replicas; check error rate; continue or rollback

---

**Q: How do you rollback if a deployment breaks production?**

> **Option 1 — Git revert (cleanest — full audit trail):**
> ```bash
> git revert <commit-sha>     # revert the image tag commit in GitOps repo
> git push                    # ArgoCD detects → auto-syncs previous image
> ```
>
> **Option 2 — ArgoCD UI:**
> - Select previous git revision in ArgoCD → sync to that revision → previous image redeployed in seconds
>
> **Option 3 — Helm rollback:**
> ```bash
> helm history retail-ui -n retail        # list revisions
> helm rollback retail-ui 3 -n retail     # rollback to revision 3
> ```
>
> **Option 4 — Kubernetes rollback:**
> ```bash
> kubectl rollout undo deployment/retail-ui -n retail
> kubectl rollout status deployment/retail-ui -n retail   # watch it complete
> ```
>
> **Decision guide:**
> - Git revert → best; preserves audit trail; ArgoCD will auto-sync
> - ArgoCD UI → fastest if you need seconds, not minutes
> - `kubectl rollout undo` → last resort if GitOps is not working

---

**Q: A node is consuming high CPU. How do you investigate and fix it?**

> ```bash
> kubectl top nodes                                # find the high-CPU node
> kubectl top pods -A --sort-by=cpu               # which pod is consuming it?
> kubectl describe node <node> | grep -A5 "Allocated"  # how much CPU is allocated vs allocatable?
>
> # For a specific pod:
> kubectl top pod <pod> -n <namespace> --containers   # per-container CPU breakdown
> kubectl exec -it <pod> -- top                        # process-level view inside container
> ```
>
> **Possible causes and fixes:**
>
> | Cause | Fix |
> |---|---|
> | Pod with no CPU limit running hot | Add `resources.limits.cpu` to cap it |
> | Infinite loop / runaway thread | Check app code; rollback if recent deploy caused it |
> | DDoS / traffic spike | HPA should have already scaled; check if HPA is configured |
> | JVM GC pressure (Java app) | Tune JVM flags, increase memory, check heap size |
> | Node too small for workload | Let Karpenter provision a larger instance type |

---

**Q: How do you debug a pod that is in ImagePullBackOff?**

> ```bash
> kubectl describe pod <pod> -n <namespace>
> # Events will show: Failed to pull image — exact error message
> ```
>
> | Error Message | Cause | Fix |
> |---|---|---|
> | `pull access denied` | ECR auth failure | Check node IAM role has `ecr:GetAuthorizationToken` + `ecr:BatchGetImage` |
> | `repository does not exist` | Wrong image name or tag | Check image URI in Helm values — typo? |
> | `no basic auth credentials` | Private registry, no image pull secret | Add `imagePullSecrets` to pod spec |
> | `manifest unknown` | Tag doesn't exist in ECR | Check if CI pushed the image successfully; check ECR console |
> | `context deadline exceeded` | Network issue — node can't reach ECR | Check security groups, NAT Gateway, VPC endpoint for ECR |
>
> **Tip:** Use a VPC endpoint for ECR (`com.amazonaws.us-east-1.ecr.api`) — pulls don't go via NAT Gateway, faster and cheaper.

---

**Q: Terraform apply is failing with a state lock error. What do you do?**

> ```
> Error: Error acquiring the state lock
> Lock Info: ID: abc-123, Who: engineer@machine
> ```
>
> **Cause:** Another `terraform apply` is running (or crashed mid-apply and left a stale lock).
>
> ```bash
> # Check if someone is actively applying — ask the team first
> # If confirmed stale lock (no apply running):
> terraform force-unlock abc-123    # pass the Lock ID from the error message
> ```
>
> **Then investigate:** why did the previous apply crash? Check the CI pipeline logs or ask the engineer. Running `terraform plan` to see the current state before applying again.
>
> **Prevention:** Run Terraform only via CI pipeline (not locally) — one pipeline at a time, no concurrent applies.

---

**Q: Scenario — Explain how you handled a production incident end-to-end.**

> **Use the STAR format (Situation → Task → Action → Result):**
>
> > "During peak load at Capgemini, the Orders service started returning 503 errors. The pipeline was green and pods were running — not obvious from the surface.
> >
> > **Task:** Identify root cause and restore service without data loss.
> >
> > **Action:**
> > 1. Checked X-Ray traces — found the SQS consumer span was timing out
> > 2. CloudWatch showed SQS queue depth had spiked to 50,000 messages (normal: < 100)
> > 3. Checked RDS PostgreSQL CloudWatch — `DatabaseConnections` was at the max_connections limit
> > 4. Cross-referenced with the recent HPA event — a traffic spike had scaled pods from 3 to 12; each pod has a connection pool of 10 → 120 connections → DB limit hit
> > 5. Immediate fix: manually reduced HPA `maxReplicas` to 5 to drop connections; DB recovered
> > 6. Permanent fix: deployed PgBouncer connection pooler in front of RDS; increased `max_connections` in RDS parameter group
> >
> > **Result:** Service restored in 8 minutes. Added a CloudWatch alarm for `DatabaseConnections > 80%` and an HPA annotation to cap connections via resource limits going forward. Added this scenario to our runbook."

---

## 15. Microservices Architecture — Q&A

---

**Q: Walk me through the microservices architecture you deployed at Capgemini.**

> I deployed a 5-microservice retail e-commerce platform on AWS EKS.

```
Internet → Route53 → AWS ALB (HTTPS / ACM certificate)
                           ↓
              EKS Cluster — Private Subnets
   ┌──────────────────────────────────────────────────┐
   │  UI Service (Java Spring Boot)                   │
   │    ↓ REST calls to all services                  │
   │  Catalog Service (Go)    → RDS MySQL             │
   │  Cart Service (Spring)   → DynamoDB              │
   │  Checkout Service (Node) → ElastiCache Redis     │
   │  Orders Service (Spring) → RDS PostgreSQL        │
   │                          → SQS (async from       │
   │                              Checkout)           │
   └──────────────────────────────────────────────────┘

CI/CD:        GitHub Actions → ECR → Helm values → ArgoCD → EKS
Observability: ADOT → X-Ray | CloudWatch | AMP + Grafana
Autoscaling:   HPA (pods) + Karpenter (nodes + Spot interruption)
Security:      EKS Pod Identity + Secrets Manager + ASCP CSI Driver
IaC:           Terraform (VPC + EKS + all AWS data services)
```

> **Service ownership:**

| Service | Language | Data Store | Why that DB |
|---|---|---|---|
| UI | Java Spring Boot | None — aggregates REST calls | Acts as BFF (Backend for Frontend) |
| Catalog | Go | RDS MySQL | High-throughput read queries; Go = fast, low-memory |
| Cart | Java Spring Boot | DynamoDB | Flexible schema per user, auto-scales for peak shopping load |
| Checkout | Node.js | ElastiCache Redis | Ephemeral session data; sub-millisecond reads; Redis TTL for cart expiry |
| Orders | Java Spring Boot | RDS PostgreSQL + SQS | ACID transactions for order placement; SQS for async processing from Checkout |

---

**Q: What is the Database-per-Service pattern and why is it used?**

> Each microservice **owns its own database exclusively** — no other service can connect to it directly.
>
> **Why:**
> - **Independent deployment** — Cart team changes DynamoDB schema without coordinating with Catalog team
> - **Independent scaling** — scale Orders DB independently during peak checkout; don't affect Catalog
> - **Technology freedom** — each team picks the best DB for their access pattern (polyglot persistence)
> - **Fault isolation** — RDS PostgreSQL (Orders) going down doesn't affect Cart or Catalog
>
> **Trade-off:** distributed transactions become complex — mitigated with:
> - **Eventual consistency** (accepted for most operations)
> - **Saga pattern** (choreography or orchestration)
> - **Outbox pattern** for reliable event publishing

---

**Q: What is polyglot persistence? Give an example from your project.**

> Polyglot persistence = using different database technologies for different services based on what fits best, rather than forcing one DB for everything.
>
> **In my project:**
> - **DynamoDB** (Cart) — flexible schema, every user's cart can have different attributes, scales automatically for Black Friday traffic
> - **Redis** (Checkout) — session cache, ephemeral, sub-millisecond latency, TTL-based expiry — no persistence needed
> - **RDS MySQL** (Catalog) — structured product data with complex filtering queries, joins needed, relational
> - **RDS PostgreSQL** (Orders) — ACID transactions, complex joins, payment integrity; PostgreSQL's JSON support for order metadata
>
> Each team chose the tool that fit their data access pattern, not the tool the infrastructure team was most comfortable with.

---

**Q: How do microservices communicate synchronously vs asynchronously? When do you use each?**

> **Synchronous — REST/HTTP:**
> - UI calls Catalog, Cart, Checkout, Orders via direct HTTP GET/POST
> - Simple, immediate response — caller waits for result
> - **Risk:** tight coupling — if Catalog is down, UI shows error; latency cascades
>
> **Asynchronous — SQS:**
> - Checkout publishes an order message to **SQS queue** → Orders service polls and processes independently
> - Caller doesn't wait — Checkout gets a success response immediately after publishing
> - **Resilient:** if Orders is down, messages queue for up to 14 days and process when Orders recovers
>
> | Use Case | Sync or Async | Reason |
> |---|---|---|
> | Load a product page | Sync | User is waiting — immediate response needed |
> | Add item to cart | Sync | User expects instant cart update |
> | Place an order | Async (SQS) | Order processing (payment, inventory, email) can happen in background |
> | Send confirmation email | Async | Not time-critical; doesn't block user |
> | Inventory update after order | Async | Eventual consistency acceptable |

---

**Q: How do services discover and communicate with each other inside Kubernetes?**

> Kubernetes internal DNS — every Service gets a stable DNS name:
> ```
> <service-name>.<namespace>.svc.cluster.local
> ```
>
> In practice, within the same namespace, just the service name works:
> ```bash
> curl http://catalog/products           # resolves to catalog service ClusterIP
> curl http://orders/api/orders          # resolves to orders service ClusterIP
> ```
>
> **How it works:**
> 1. Each Service gets a **ClusterIP** (stable virtual IP)
> 2. **CoreDNS** resolves `catalog` → ClusterIP
> 3. **kube-proxy** (iptables rules) load-balances ClusterIP to pod IPs
>
> Services use **ClusterIP** type for internal communication — not exposed externally. Only the UI service is exposed via ALB Ingress.

---

**Q: What is the difference between ClusterIP, NodePort, and LoadBalancer in your project?**

> - **ClusterIP** (all backend services) — Catalog, Cart, Checkout, Orders are ClusterIP; accessible only inside the cluster. No external exposure.
> - **ALB Ingress** (UI) — AWS Load Balancer Controller provisions an ALB for the UI service; external HTTPS traffic enters here only.
>
> Kubernetes Service types are not used for external access in EKS — ALB Ingress is cheaper (one ALB, multiple services) and has SSL termination, path routing, and host routing.

---

**Q: What happens when the Checkout service is down? Does it affect other services?**

> **With synchronous calls:** yes — the UI would return errors for checkout flows.
>
> **Design choices that limit blast radius:**
> - **Circuit breaker pattern** — if Checkout is down, fail fast with a user-friendly message rather than waiting for timeout on every request
> - **Bulkhead pattern** — Checkout pod failures don't consume threads from Catalog or Cart pods; each service has its own connection pool
> - **Retry + exponential backoff** — transient errors retried automatically without flooding the failing service
> - **Database independence** — Checkout Redis going down doesn't affect Orders RDS or Catalog MySQL
>
> **What Kubernetes handles automatically:**
> - `readinessProbe` — unhealthy Checkout pods removed from Service endpoints; healthy pods still serve
> - `livenessProbe` — crashed Checkout pods restarted automatically
> - HPA ensures minimum replicas are maintained

---

**Q: How do you handle distributed transactions across microservices?**

> True distributed ACID transactions across services are avoided where possible — they introduce tight coupling and performance overhead.
>
> **Approaches used:**
>
> **1. Saga pattern (what I use — event-driven):**
> - Checkout publishes "OrderPlaced" to SQS → Orders consumes → validates stock → publishes "OrderConfirmed" or "OrderFailed" back to SQS → Checkout updates status
> - Each step is local transaction; compensating transactions handle failures (e.g., "OrderFailed" → restore cart)
>
> **2. Eventual consistency (accepted for non-critical flows):**
> - Inventory count is eventually consistent — a brief moment of over-selling is acceptable; reconciled by Orders
>
> **3. Outbox pattern (reliable event publishing):**
> - Orders writes to its own DB AND to an "outbox" table in the same local transaction
> - A background process reads the outbox and publishes to SQS — guarantees the event is published even if the service crashes after DB write but before SQS publish

---

**Q: What is the strangler fig pattern?**

> A technique for **migrating a monolith to microservices incrementally** — instead of rewriting everything at once (risky), you strangle the monolith gradually.
>
> **How it works:**
> 1. New traffic goes to a proxy/API gateway in front of the monolith
> 2. Incrementally extract one feature at a time into a new microservice
> 3. Proxy routes that feature's traffic to the new service
> 4. The monolith "shrinks" as more features are extracted
> 5. Eventually the monolith handles nothing and can be decommissioned
>
> Named after the strangler fig tree — grows around a host tree, eventually replacing it.

---

**Q: What is an API Gateway and where does it sit in a microservices architecture?**

> An API Gateway is the **single entry point** for all client requests — it sits in front of all microservices and handles:
> - **Routing** — route `/catalog/*` to Catalog service, `/orders/*` to Orders service
> - **Authentication** — validate JWT tokens before forwarding to services
> - **Rate limiting** — throttle requests per client to prevent abuse
> - **SSL termination** — handle HTTPS at the gateway level
> - **Request/response transformation** — adapt API versions
>
> **In my project:** the **AWS ALB with Ingress rules** serves as the API gateway for external traffic. For more advanced API management (auth, rate limiting, developer portal), AWS API Gateway or Kong would be added.

---

**Q: What are the trade-offs of microservices vs monolith?**

| Aspect | Monolith | Microservices |
|---|---|---|
| Deployment | One deployable unit — simple | Many services — complex orchestration |
| Scaling | Scale the whole app | Scale individual services independently |
| Technology | One language/DB | Polyglot — best tool per service |
| Fault isolation | One crash = whole app down | One service crash = limited blast radius |
| Development speed | Fast initially | Slower initially — service contracts, APIs |
| Operational complexity | Low | High — need Kubernetes, service mesh, observability |
| Team structure | Works for small teams | Fits large teams — Conway's Law |
| When to use | Early stage, small team, simple domain | Scale > 20 engineers, clear domain boundaries |

> **My take:** Microservices complexity is only justified when the team size and deployment frequency demand it. We use them at Capgemini because 5 independent services can deploy independently — the Catalog team doesn't wait for Orders team to release.

---

**Q: How do you manage different technology stacks across microservices in one Kubernetes cluster?**

> Each microservice is packaged as its own Docker image — the cluster doesn't care what language or framework is inside.
>
> **How we manage it:**
> - **Separate Helm charts** per service — independent values, independent deployment lifecycle
> - **Separate namespaces** or label-based isolation — each service owns its own resources
> - **Separate CI/CD pipelines** — each service repo has its own GitHub Actions workflow; changing Orders doesn't rebuild Catalog
> - **Shared Helm library chart** — common templates (Deployment, Service, HPA, PDB) shared across all services to avoid duplication
> - **ArgoCD ApplicationSet** — manages all 5 services' ArgoCD Applications from a single config; adding a 6th service = one new entry

---

## 16. Change Requests (CR) / ITSM — Q&A

---

**Q: What is a Change Request and why is it used in DevOps?**

> A CR is a formal, documented proposal to modify any component of an IT system — infrastructure, application, or process. Part of **ITIL Change Management** — ensures changes are reviewed, approved, and have a rollback plan before touching production.
>
> **Fields in a CR:**

| Field | Description |
|---|---|
| CR ID | Unique identifier — e.g., CHG0012345 |
| Description | What is changing and why |
| Risk level | Low / Medium / High / Critical |
| Rollback plan | How to undo the change if it fails |
| Testing plan | How you verified it works in lower environments |
| Maintenance window | When it will be executed (typically off-peak) |
| Approvers | Manager, CAB, Security, Architect sign-off |
| Impacted services | What systems are affected |

---

**Q: What are the three types of Change Requests?**

| Type | Description | Approval | Example |
|---|---|---|---|
| **Standard** | Pre-approved, low-risk, routine — follows a template | None — already approved | SSL certificate renewal, password reset, minor config patch |
| **Normal** | Planned change — full CAB review cycle | CAB approval required | EKS version upgrade, new microservice deployment, VPC change |
| **Emergency** | Urgent production fix — no time for full CAB process | Security Lead + EM — expedited; post-review done within 24h | Critical CVE patch, production outage hotfix |

> **CAB (Change Advisory Board):** a group — Ops leads, Security, Dev Leads, Architects — that reviews and approves Normal CRs.

---

**Q: Where does a CR fit in your CI/CD pipeline?**

```
Developer merges to main
  ↓
GitHub Actions: build → test → scan → push to ECR       ← automated (no CR)
  ↓
ArgoCD: deploy to Dev → Staging                          ← automated (no CR)
  ↓
QA sign-off + performance test
  ↓
Engineer raises Normal CR in ServiceNow                  ← CR RAISED HERE
  (includes: deployment plan, rollback = ArgoCD revert,
   maintenance window, impacted services)
  ↓
CAB reviews → approves → maintenance window opens
  ↓
ArgoCD syncs to Production (manual trigger or auto in window)  ← CR reference logged
  ↓
Smoke tests pass → CR closed as Successful
  ↗ or ↘
      Tests fail → execute rollback → CR closed as Failed + PIR
```

---

**Q: In which DevOps scenarios would you raise each type of CR?**

| Scenario | CR Type |
|---|---|
| Production code deployment | Normal CR |
| EKS version upgrade (1.28 → 1.29) | Normal CR (high risk — full CAB) |
| RDS schema migration | Normal CR (critical risk — backup required) |
| Terraform infra change (new SG, VPC change) | Normal CR |
| ACM certificate renewal | Standard CR (pre-approved template) |
| Emergency CVE patch (CVSS ≥ 9.0) | Emergency CR |
| Production outage hotfix | Emergency CR |
| IAM policy modification | Normal CR (security review required) |
| Karpenter NodePool limit increase | Normal CR |

---

**Q: What tools are used to manage CRs?**

| Tool | Used by |
|---|---|
| **ServiceNow** | Enterprise standard — most common in large organisations |
| **Jira Service Management** | Dev-centric teams, common in tech companies |
| **BMC Remedy** | Legacy enterprise |
| **PagerDuty** | Emergency CR tracking + on-call coordination |

---

**Q: What is a post-implementation review (PIR)?**

> A PIR is conducted **after an Emergency CR** — typically within 24–48 hours. Reviews:
> - Did the change achieve its goal?
> - Did anything unexpected happen?
> - What would you do differently?
> - What process improvements prevent this emergency in future?
>
> For Normal CRs: PIR is optional unless the change caused an incident.

---

**Q: Walk me through how you handle a CVE patch end-to-end.**

> "When a high-severity CVE is identified affecting our EKS node AMI:
>
> 1. Security team identifies CVE → assess CVSS score
> 2. DevOps verifies if our cluster version/AMI is affected: `aws ec2 describe-images` to check current AMI
> 3. If CVSS ≥ 7.0 → raise **Emergency CR** in ServiceNow; get Security Lead + EM approval (bypasses full CAB wait)
> 4. Patch dev cluster: update `ami_release_version` in Terraform → `terraform apply` → rolling node replacement
> 5. Verify: check node AMI version, run Trivy on new nodes, check all pods Running
> 6. Monitor for 4 hours → move to staging → same process
> 7. Patch production in an emergency maintenance window (typically 2–4 AM)
> 8. Verify production: `kubectl get nodes -o wide` shows new AMI → Trivy scan → smoke tests
> 9. Close CR as Successful → PIR within 24 hours
>
> Total time: Critical CVE (≥ 9.0) patched within 24 hours; High (7.0–8.9) within 72 hours."

---

## 17. Behavioral — Q&A

---

**Q: Give an example of how you improved a pipeline's performance or reliability.**

> **Example 1 — 85% observability cost reduction (Capgemini):**
> Health check endpoints ran every 30s per pod — 5 services × multiple replicas = thousands of zero-value traces per hour. Added an ADOT filter processor to drop them at collection time. No code changes, no app restarts — just config. Result: 85% reduction in X-Ray ingestion cost with zero loss in diagnostic value for real requests.
>
> **Example 2 — 40% faster build times (Infosys):**
> Analyzed CodePipeline build history and found Maven dependency downloads took 3 of 7 minutes every build. Added S3-backed CodeBuild caching for the `.m2` repository. Cache hit rate: ~85%. Build time: 7 min → ~4 min.
>
> **Example 3 — Zero-touch deployment pipeline (Capgemini):**
> Deployments previously required an engineer to SSH into servers and run a script — error-prone and not auditable. Built GitHub Actions + ArgoCD GitOps pipeline — code commit to production fully automated, full audit trail in git, rollback is one `git revert`.

---

**Q: Describe a time you had to debug a complex production issue.**

> "During peak load, Orders service was intermittently returning 503s. Pipeline was green, pods running — not obvious from the surface.
>
> I opened X-Ray and found the SQS consumer span was timing out. CloudWatch showed queue depth had spiked to 50,000 messages. Cross-referenced with the HPA event log — a traffic spike had scaled pods from 3 to 12; each pod's connection pool was 10, so 120 DB connections were opened simultaneously. RDS max_connections was 100 — connections were being refused.
>
> Immediate fix: reduced HPA maxReplicas to 5 to drop connections below the limit — DB recovered within 2 minutes.
>
> Permanent fix: deployed PgBouncer connection pooler in front of RDS, increased RDS max_connections in the parameter group. Added a CloudWatch alarm for DatabaseConnections > 80% going forward.
>
> Service restored in 8 minutes. Added this to our runbook so any on-call engineer can handle it independently."

---

**Q: How do you promote DevOps culture in a team?**

> - **Make pipelines self-service** — developers deploy without raising tickets; ArgoCD UI gives them full visibility into their own deployment status
> - **Blameless post-mortems** — focus on what failed in the system, not who triggered it; document and automate the fix; share learnings openly
> - **Shift security left** — image scanning and secret detection in CI; developers get feedback at PR time, not after a production incident
> - **Observability by default** — new services get metrics, tracing, and dashboards from day one; the whole team sees the dashboards, not just ops
> - **Document everything** — runbooks for every incident pattern; any on-call engineer should be able to handle known issues without escalating

---

**Q: Tell me about a time you had a disagreement with a developer about a technical decision.**

> "A developer on my team wanted to hardcode DB credentials directly in the Helm values file for speed — 'we'll fix it properly later'. I understood the pressure but explained that 'later' rarely happens and the risk was real: credentials in git history are permanent.
>
> I didn't just say no — I sat with them for 30 minutes and set up Secrets Manager + ASCP CSI Driver integration together. It took the same time as writing the credentials manually, and once done it auto-rotates forever.
>
> The developer appreciated that I helped rather than just enforced a policy, and that pattern was adopted by the whole team from then on."

---

**Q: What would you automate first if you joined Accenture's team?**

> "First two weeks: understand the current state — what's manual, where the pain is, what the team complains about most.
>
> Then I'd prioritise:
> 1. **Any manual deployment steps** — SSH-based deploys, manual ECR pushes, manual Helm commands. Replace with GitHub Actions + ArgoCD.
> 2. **Infrastructure not in Terraform** — if anything was created in the console, `terraform import` it and bring it under IaC.
> 3. **Observability gaps** — if there's no centralised dashboards or alerting, that's a blind spot in every incident.
> 4. **Security scanning** — if there's no Trivy or secret detection in CI, add it. Fast wins, huge risk reduction.
>
> Quick wins first — build trust, then deeper platform work."

---

**Q: How do you handle on-call and production incidents?**

> "I follow a structured approach:
>
> 1. **Acknowledge** the alert within the SLA window — let the team know someone is on it
> 2. **Assess severity** — is the app fully down (P1) or degraded (P2/P3)?
> 3. **Communicate** — post in the incident Slack channel: what I know, what I'm investigating, ETA for update
> 4. **Mitigate first, root cause second** — rollback if needed to restore service; full RCA after users are unblocked
> 5. **Document timeline** — every action with timestamp; essential for the post-mortem
> 6. **Post-mortem** within 24–48 hours — blameless, focuses on system improvements
>
> For major incidents, I raise an Emergency CR if a production change is needed."

---

**Q: Where do you see yourself in 2–3 years?**

> "I want to move toward a Senior DevOps / Platform Engineer or Cloud Architect role — designing platform-level solutions that multiple product teams use, not just maintaining pipelines. I'm interested in expanding into multi-cloud architecture, service mesh (Istio), and platform engineering — building the internal developer platform that makes every team faster and more secure. Accenture's exposure to diverse clients and industries would accelerate that growth significantly."

---

## 18. Questions to Ask Accenture

> Always ask 2–3 questions — shows genuine interest and preparation.

1. **"What does the current CI/CD and container platform look like for this project — are Terraform, Vault, and EKS already in use, or is there a migration in progress?"**
   *(Shows you understand the JD stack and want to contribute immediately)*

2. **"What does the on-prem + AWS hybrid infrastructure look like — is there an active migration to cloud, or is it a stable hybrid setup?"**
   *(Addresses the one gap in your profile honestly and shows curiosity)*

3. **"How does the team handle on-call and incident response — what's the alerting stack and escalation process?"**
   *(Shows production mindset)*

4. **"Is this an embedded DevOps role within a product team, or a centralized platform/SRE team serving multiple teams?"**
   *(Helps you understand the day-to-day and show you've thought about team dynamics)*

5. **"What does success look like in the first 90 days for this role?"**
   *(Classic — shows you're thinking about impact, not just joining)*

---

## 19. Key Talking Points — Weave These Into Every Answer

| Point | Why It Lands |
|---|---|
| **"Zero-touch deployments — code commit to production"** | Proves complete end-to-end CI/CD ownership, not just one part |
| **"85% trace cost reduction through ADOT filter processor"** | Concrete, quantifiable, specific — not vague "improved observability" |
| **"60–70% compute cost savings using Karpenter Spot instances"** | Business impact — engineers who save money get noticed |
| **"OIDC in GitHub Actions — no static AWS access keys anywhere"** | Security-first mindset — Accenture cares about client security |
| **"EKS Pod Identity + Secrets Manager — zero hardcoded credentials at any layer"** | Security depth — production-grade, not just a good intention |
| **"Terraform modules with S3 + DynamoDB state locking"** | Team-ready IaC, not just solo scripts |
| **"GitOps with ArgoCD self-heal — drift detected and reverted automatically"** | Reliability mindset, not just deploy and hope |
| **"Multi-AZ VPC, PodDisruptionBudget, Karpenter Spot interruption handling"** | Shows you think about production availability from day one |
| **"I reduced Maven build time by 40% with CodeBuild S3 caching"** | Shows initiative — didn't just maintain, improved |

---

## 20. Quick Reference — JD Language → Your Experience

| JD Says | You Say |
|---|---|
| AWS Containerization | EKS + ECR + Docker multi-stage + Helm — 5-microservice retail platform |
| CI/CD pipelines | GitHub Actions + ArgoCD GitOps; CodePipeline + CodeBuild at Infosys |
| Container orchestration | Kubernetes on EKS — Karpenter, HPA, Pod Identity, Helm, ArgoCD |
| HashiCorp Terraform | Terraform modules, remote S3 state, DynamoDB locking — both companies |
| HashiCorp Vault | AWS Secrets Manager + Secrets Store CSI Driver (ASCP) — equivalent |
| Grafana + Prometheus | Amazon Managed Prometheus (AMP) + Grafana dashboards |
| Alert Manager | CloudWatch Alarms + SNS + Grafana alerting — equivalent |
| Linux + config management | EC2, Bash scripting, user data; Terraform as IaC config management |
| Git / version control | GitHub (Capgemini — app repos + GitOps repo), GitLab (Infosys) |
| Monitoring / logging | ADOT → X-Ray + CloudWatch + AMP + Grafana — full 3-pillar stack |
| Scaling | Karpenter (nodes) + HPA (pods) + EventBridge Spot handling |
| Security | OIDC, Pod Identity, Secrets Manager, Trivy SAST, non-root containers |
| Agile / DevOps culture | GitOps mindset, blameless post-mortems, shift-left security, self-service pipelines |
