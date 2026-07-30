# Interview Questions Asked

---

## PROJECT-BASED INTERVIEW QUESTIONS
### Based on: Ultimate DevOps Real-World Project (AWS Cloud) — Kalyan Reddy Daida

---

## MODULE-01: Retail Store Microservices Architecture

**Q: Walk me through the architecture of the retail store application you deployed.**

- 5 microservices: UI (Spring Boot), Carts (Spring Boot + DynamoDB), Catalog (Go + RDS MySQL), Orders (Spring Boot + RDS PostgreSQL), Checkout (Node.js + Redis/SQS)
- Each service owns its own data store — this is the **Database-per-Service** pattern
- Services communicate via REST/HTTP; async messaging via SQS between Checkout and Orders
- Deployed on EKS, exposed via ALB Ingress Controller, DNS managed by External DNS + Route53

**Q: Why use different languages and databases per service?**

- Each team owns their service independently — different languages suit different performance needs (Go for Catalog = high-throughput, low-latency reads)
- Polyglot persistence: DynamoDB (Carts = flexible schema, high scale), Redis (Checkout = ephemeral session cache), PostgreSQL (Orders = ACID transactions)
- Trade-off: operational complexity increases — mitigated by Kubernetes + Helm + observability stack

**Q: How do the microservices discover and communicate with each other inside Kubernetes?**

- Kubernetes internal DNS — each service is reachable as `<service-name>.<namespace>.svc.cluster.local`
- Services use ClusterIP type for internal communication; no external exposure needed
- UI service aggregates calls to backend services; end user only hits the UI via ALB

---

## MODULE-02 & 03: Docker Fundamentals and Dockerfile Mastery

**Q: What is the difference between CMD and ENTRYPOINT? Give a real example from your project.**

- `ENTRYPOINT` is the fixed executable; `CMD` provides default arguments that can be overridden
- Example: `ENTRYPOINT ["java", "-jar", "app.jar"]` and `CMD ["--spring.profiles.active=prod"]`
- At `docker run`, if you pass args they replace `CMD` but not `ENTRYPOINT`

**Q: How did you use multi-stage builds in this project? What was the image size difference?**

- Stage 1 (builder): uses `maven:3.9-eclipse-temurin-17` to compile — includes JDK, Maven, source code
- Stage 2 (runtime): uses `eclipse-temurin:17-jre-alpine` — only copies the `.jar`
- Typical result: image shrinks from ~800MB to ~180MB; attack surface reduced significantly
- Build tools (`mvn`, `gcc`) never appear in the final image

**Q: What security best practices did you apply in your Dockerfiles?**

| Practice | Why |
|---|---|
| Non-root USER instruction | Prevents container escape privilege escalation |
| Distroless / Alpine base | Smaller attack surface, fewer CVEs |
| Multi-stage builds | Strips build tools from runtime image |
| `.dockerignore` | Prevents secrets/`.git` from entering build context |
| HEALTHCHECK instruction | Kubernetes/Docker can detect unhealthy containers |
| Pinned base image versions | Avoids surprise breaking changes from `:latest` |

**Q: What does HEALTHCHECK do in a Dockerfile? How does it interact with Kubernetes probes?**

- `HEALTHCHECK` is a Docker-native health check — used by Docker Engine and `docker-compose`
- Kubernetes has its own `livenessProbe` and `readinessProbe` — these **override** Docker's HEALTHCHECK
- Best practice: define both for compatibility; Kubernetes probes are more feature-rich (HTTP, TCP, exec)

---

## MODULE-04 & 05: Docker Compose and BuildKit

**Q: How did you use Docker Compose for the retail store locally? What features did you use?**

- Named volumes for persistent data (MySQL, Redis), custom bridge networks for service isolation
- Health checks with `condition: service_healthy` to enforce startup order (DB must be ready before app)
- Profiles to selectively start subsets of services (`docker compose --profile monitoring up`)
- `deploy.replicas` for local scaling simulation

**Q: What is Docker BuildKit and why should you use it over the classic builder?**

- BuildKit is the next-generation build engine: parallel stage execution, better caching, secrets mounting
- `RUN --mount=type=secret` — mounts secrets at build time without baking them into layers
- `RUN --mount=type=cache` — caches package manager downloads across builds (npm, Maven cache)
- `docker buildx` enables multi-platform builds (AMD64 + ARM64 in one command)

**Q: How did you build multi-platform images? Why is this important in production?**

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t myrepo/catalog:v1.0 --push .
```
- Important because EKS Graviton nodes (ARM64) are 40% cheaper than x86; same image runs on both
- Also supports developer Mac M-series (ARM64) while production runs on x86 EC2

---

## MODULE-06: Terraform Basics

**Q: Explain how you managed Terraform state remotely. What problem does it solve?**

- State stored in **S3 bucket** with versioning enabled; **DynamoDB table** provides state locking
- Locking prevents two engineers running `terraform apply` simultaneously, which would corrupt state
- S3 versioning allows rollback to previous state if something goes wrong
- Backend config:
```hcl
terraform {
  backend "s3" {
    bucket         = "my-tf-state"
    key            = "prod/vpc/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "tf-state-lock"
    encrypt        = true
  }
}
```

**Q: What is variable precedence in Terraform? List from lowest to highest priority.**

| Priority | Source |
|---|---|
| 1 (lowest) | Default values in `variable` blocks |
| 2 | `terraform.tfvars` file |
| 3 | `*.auto.tfvars` files |
| 4 | `-var-file` flag |
| 5 | `-var` flag |
| 6 (highest) | Environment variables (`TF_VAR_name`) |

**Q: What is a Terraform data source? Give an example from your VPC setup.**

- A data source reads existing infrastructure without managing it — read-only reference
- Example: fetch the latest Amazon Linux 2023 AMI ID without hardcoding it:
```hcl
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["al2023-ami-*-x86_64"]
  }
}
```

**Q: What is `terraform taint` / `terraform apply -replace`? When would you use it?**

- Forces destruction and recreation of a specific resource on the next `apply`
- Use when a resource is in a bad state but Terraform doesn't detect drift (e.g., corrupted EC2 userdata ran incorrectly)
- Modern syntax: `terraform apply -replace="aws_instance.web"`

---

## MODULE-07: Terraform EKS Cluster

**Q: What IAM roles are required for an EKS cluster? Explain each.**

| Role | Attached Policy | Purpose |
|---|---|---|
| EKS Cluster Role | `AmazonEKSClusterPolicy` | Allows EKS control plane to manage AWS resources |
| Node Group Role | `AmazonEKSWorkerNodePolicy` | Allows worker nodes to join cluster |
| Node Group Role | `AmazonEC2ContainerRegistryReadOnly` | Allows nodes to pull images from ECR |
| Node Group Role | `AmazonEKS_CNI_Policy` | Allows VPC CNI plugin to manage ENIs |

**Q: How do you configure `kubectl` to connect to your EKS cluster?**

```bash
aws eks update-kubeconfig --region us-east-1 --name my-cluster
```
- Updates `~/.kube/config` with cluster endpoint, CA cert, and auth token command
- Authentication uses `aws eks get-token` under the hood (no static credentials)

**Q: What is EKS Pod Identity and how is it different from IRSA?**

| Feature | IRSA (IAM Roles for Service Accounts) | Pod Identity (newer) |
|---|---|---|
| Setup | Annotate service account + OIDC provider | Pod Identity Agent DaemonSet |
| Token projection | OIDC JWT via volume mount | Agent handles token exchange |
| Cross-account | Requires trust policy setup | Simpler association API |
| Preferred for new clusters | No | Yes — AWS recommended |

---

## MODULE-08: Kubernetes Foundation

**Q: What is the difference between a Deployment and a StatefulSet? When do you use each?**

| Feature | Deployment | StatefulSet |
|---|---|---|
| Pod identity | Random names (`app-abc123`) | Stable ordinal names (`app-0`, `app-1`) |
| Storage | Shared or ephemeral | Each pod gets its own PVC |
| Scaling order | Parallel | Sequential (0→1→2) |
| Use case | Stateless apps (UI, API) | Databases, Kafka, ZooKeeper |

**Q: What is the difference between liveness and readiness probes?**

- **Liveness probe** — is the container alive? If it fails, Kubernetes **restarts** the container
- **Readiness probe** — is the container ready to serve traffic? If it fails, pod is **removed from Service endpoints** (no traffic sent) but NOT restarted
- **Startup probe** — gives slow-starting apps time to initialize before liveness kicks in

**Q: What happens when a pod exceeds its memory limit?**

- Container is **OOMKilled** (Out of Memory Killed) by the Linux kernel
- Kubernetes restarts the container; `kubectl describe pod` shows `OOMKilled` as the reason
- Fix: increase `resources.limits.memory`, find memory leak, or add autoscaling
- `resources.requests` affects **scheduling** (where the pod lands); `limits` affects **runtime enforcement**

---

## MODULE-09: Kubernetes Secrets Management

**Q: What is the difference between External Secrets Operator (ESO) and Secrets Store CSI Driver?**

| Feature | External Secrets Operator | Secrets Store CSI Driver |
|---|---|---|
| Secret delivery | Creates native K8s Secret objects | Mounts secrets directly as files/env vars |
| Sync | Periodic sync + auto-rotation | Mounts at pod start; rotation requires pod restart |
| Access pattern | Standard env var / volume mount | File-based mount |
| Best for | Teams preferring native K8s secrets | Avoiding secrets stored in etcd |

**Q: Why is it bad practice to store secrets in Kubernetes Secrets natively without encryption?**

- Kubernetes Secrets are base64-encoded, **not encrypted** by default in etcd
- Anyone with `kubectl get secret` access can decode them trivially: `echo "dmFsdWU=" | base64 -d`
- Solution: enable **Envelope Encryption** (KMS key to encrypt etcd data at rest) or use ESO/CSI to avoid storing secrets in etcd entirely

---

## MODULE-10: Kubernetes Persistent Storage

**Q: Explain the relationship between PV, PVC, and StorageClass.**

```
StorageClass (defines HOW to provision — AWS EBS gp3)
    ↓ dynamic provisioning
PersistentVolume (actual EBS volume — created automatically)
    ↑ bound to
PersistentVolumeClaim (app's request — "I need 20Gi ReadWriteOnce")
    ↑ mounted by
Pod
```

- Without StorageClass: PV must be manually pre-provisioned (static provisioning)
- With StorageClass: PV is auto-created when PVC is submitted (dynamic provisioning)

**Q: What is the EBS CSI Driver? Why is it needed?**

- EBS CSI Driver runs as a DaemonSet on worker nodes; implements the Container Storage Interface spec
- Allows Kubernetes to dynamically create/attach/detach AWS EBS volumes to pods
- Without it, EBS volumes can't be dynamically provisioned from PVCs

**Q: What are the EBS volume access modes and what does ReadWriteOnce mean?**

| Access Mode | Meaning | EBS Support |
|---|---|---|
| `ReadWriteOnce` (RWO) | One node can mount read-write | Yes |
| `ReadOnlyMany` (ROX) | Many nodes can mount read-only | Yes |
| `ReadWriteMany` (RWX) | Many nodes can mount read-write | No (use EFS) |

- EBS is block storage — only one node can attach at a time → `ReadWriteOnce` only

---

## MODULE-11: Kubernetes Ingress and ALB

**Q: How does the AWS Load Balancer Controller work with Kubernetes Ingress?**

- Controller watches for `Ingress` resources with `kubernetes.io/ingress.class: alb` annotation
- Automatically provisions an ALB in AWS, configures listeners, rules, and target groups
- Target group points to pod IPs directly (IP mode) or node NodePort (instance mode)
- IP mode preferred: lower latency, no double-hop through NodePort

**Q: How do you configure HTTPS with SSL termination on ALB Ingress?**

```yaml
annotations:
  kubernetes.io/ingress.class: alb
  alb.ingress.kubernetes.io/scheme: internet-facing
  alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:123:certificate/abc
  alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
  alb.ingress.kubernetes.io/ssl-redirect: '443'
```
- ACM certificate handles TLS termination at ALB — pods receive plain HTTP
- `ssl-redirect` forces HTTP → HTTPS redirect

---

## MODULE-12 & 19: Helm Package Manager

**Q: What is Helm? What problem does it solve over raw Kubernetes YAML?**

- Helm is a package manager for Kubernetes — bundles K8s manifests into a versioned **chart**
- Problem with raw YAML: environment-specific values (image tags, replica counts, resource limits) require manual editing or fragile `sed` commands
- Helm uses **Go templates** — one chart, multiple `values.yaml` files for different environments
- `helm upgrade --install` is idempotent — install if not exists, upgrade if it does

**Q: What is the difference between `helm install`, `helm upgrade`, and `helm rollback`?**

| Command | Purpose |
|---|---|
| `helm install` | First-time install of a chart; fails if release exists |
| `helm upgrade` | Updates an existing release with new chart/values |
| `helm upgrade --install` | Idempotent — preferred in CI/CD |
| `helm rollback <release> <revision>` | Rolls back to a previous revision number |
| `helm history <release>` | Shows all revisions and their status |

**Q: How do you override Helm values in CI/CD without modifying `values.yaml`?**

```bash
helm upgrade --install retail-ui ./charts/ui \
  --set image.tag=$IMAGE_TAG \
  --set replicaCount=3 \
  -f values-prod.yaml
```
- `--set` for individual overrides; `-f` for environment-specific values files
- In GitOps with ArgoCD: commit new `image.tag` to git, ArgoCD detects and syncs

---

## MODULE-13: Terraform EKS with Add-Ons

**Q: How do you install EKS add-ons with Terraform? Name the add-ons you used.**

```hcl
resource "aws_eks_addon" "ebs_csi" {
  cluster_name             = aws_eks_cluster.main.name
  addon_name               = "aws-ebs-csi-driver"
  addon_version            = "v1.28.0-eksbuild.1"
  service_account_role_arn = aws_iam_role.ebs_csi.arn
}
```
Add-ons used:
| Add-On | Purpose |
|---|---|
| `aws-ebs-csi-driver` | Dynamic EBS volume provisioning |
| `eks-pod-identity-agent` | Pod-level IAM without IRSA overhead |
| `aws-load-balancer-controller` | ALB/NLB provisioning from Ingress/Service |
| `secrets-store-csi-driver` | Mount AWS Secrets Manager secrets as files |

---

## MODULE-15 & 16: External DNS

**Q: What is External DNS and how does it automate Route53 records?**

- External DNS controller watches Ingress and Service resources
- When an ALB is created with a hostname annotation, External DNS automatically creates/updates Route53 A records
- Eliminates manual DNS management — destroy the Ingress, the DNS record is removed automatically
- Requires IAM permissions to `route53:ChangeResourceRecordSets`

---

## MODULE-17: Karpenter Autoscaling

**Q: What is the difference between Karpenter and Cluster Autoscaler?**

| Feature | Cluster Autoscaler | Karpenter |
|---|---|---|
| Node provisioning | Scales existing ASG node groups | Directly calls EC2 API — any instance type |
| Speed | 3-5 minutes | Under 60 seconds |
| Flexibility | Limited to pre-defined node groups | Any instance type/family dynamically |
| Bin packing | Limited | Efficient consolidation built-in |
| Spot handling | Basic | EventBridge → SQS interruption handling |

**Q: How does Karpenter handle Spot Instance interruptions? Walk through the full flow.**

1. AWS sends 2-minute Spot interruption warning → **EventBridge** rule captures it
2. EventBridge sends event to **SQS queue**
3. **Karpenter** polls SQS, receives interruption notice
4. Karpenter **cordons** the node (no new pods scheduled) and **drains** it gracefully
5. Pods are evicted and rescheduled on available nodes (or Karpenter provisions a new node)
6. **PodDisruptionBudget** ensures minimum replicas stay alive during eviction

**Q: What is a NodePool in Karpenter and what does it define?**

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
          values: ["m5.large", "m5.xlarge", "m4.large"]
  limits:
    cpu: "1000"
  disruption:
    consolidationPolicy: WhenUnderutilized
```
- Defines what kind of nodes Karpenter can provision — instance types, capacity types, AZs
- `EC2NodeClass` defines AMI, subnet, security groups, instance profile

---

## MODULE-18: HPA

**Q: How does HPA + Karpenter work together? Explain the scaling chain.**

```
High traffic
  → HPA detects CPU > threshold
  → HPA scales pod replicas up
  → Pods become Pending (insufficient node capacity)
  → Karpenter sees Pending pods
  → Karpenter provisions new EC2 node within ~60 seconds
  → Pods schedule on new node
```
- HPA scales **pods**; Karpenter scales **nodes** — they complement each other
- Without Karpenter, new pods would remain Pending until Cluster Autoscaler (slower) adds nodes

**Q: What metrics can HPA scale on beyond CPU?**

- CPU and Memory (built-in via Metrics Server)
- Custom metrics via **KEDA** (Kubernetes Event-Driven Autoscaling): SQS queue depth, HTTP request rate, Kafka consumer lag
- External metrics: Datadog, Prometheus, CloudWatch metrics

---

## MODULE-20: OpenTelemetry Observability

**Q: Explain the three pillars of observability and which AWS services you used for each.**

| Pillar | What it tells you | AWS Service Used |
|---|---|---|
| **Traces** | Request flow across microservices, latency per hop | AWS X-Ray via ADOT |
| **Logs** | Detailed event records from each service | CloudWatch Logs via ADOT |
| **Metrics** | Aggregate numbers (CPU, request rate, error rate) | Amazon Managed Prometheus + Grafana |

**Q: How did ADOT auto-instrumentation work for Java Spring Boot? Did you change application code?**

- Zero code changes — ADOT Operator injects a Java agent as an init container via **mutating webhook**
- Annotate the pod/namespace: `instrumentation.opentelemetry.io/inject-java: "true"`
- Agent intercepts Spring Boot HTTP calls and publishes spans to OTEL Collector automatically
- OTEL Collector exports to X-Ray (traces), CloudWatch (logs), AMP (metrics)

**Q: How did you reduce observability costs by 85%? What was the technique?**

- Health check endpoints (`/health`, `/actuator/health`) generate thousands of traces per hour — low value, high cost
- Configured **tail-based sampling** and **filter processors** in OTEL Collector to drop health check traces:
```yaml
processors:
  filter/drop_health:
    traces:
      span:
        - 'attributes["http.target"] == "/health"'
        - 'attributes["http.target"] == "/actuator/health"'
```
- Only kept error traces and slow traces (>500ms) from health check paths

**Q: What is the OTEL Collector and why is it used instead of sending data directly to backends?**

- Central telemetry pipeline: receives, processes, and exports observability data
- Benefits: vendor-neutral (switch from X-Ray to Jaeger without code change), batching, filtering, sampling, data enrichment
- Architecture: `App → OTEL SDK → OTEL Collector → X-Ray / CloudWatch / Prometheus`

---

## MODULE-21: CI/CD with GitHub Actions and ArgoCD

**Q: How did you avoid storing AWS credentials in GitHub Actions? Explain OIDC.**

- GitHub Actions supports **OIDC (OpenID Connect)** — no static access keys needed
- Flow:
  1. GitHub Actions requests a short-lived JWT token from GitHub's OIDC provider
  2. AWS STS `AssumeRoleWithWebIdentity` validates the token against the trusted OIDC provider
  3. Returns temporary credentials valid only for that workflow run
- IAM trust policy restricts to specific repo/branch: `token.actions.githubusercontent.com:sub: repo:org/repo:ref:refs/heads/main`

**Q: Explain the complete CI/CD flow from code commit to production deployment.**

```
Developer pushes code / creates PR
  ↓
GitHub Actions CI workflow:
  - Lint + unit tests
  - Build Docker image
  - Trivy security scan (fail if CRITICAL CVEs)
  - Push to ECR with semantic version tag (e.g., v1.2.3)
  - Update Helm values.yaml image.tag in GitOps repo
  ↓
ArgoCD detects git change (polls every 3 min or webhook)
  ↓
ArgoCD compares desired state (git) vs live state (cluster)
  ↓
ArgoCD applies diff — rolling update in Kubernetes
  ↓
Slack notification: success/failure
```

**Q: What is GitOps? How is ArgoCD implementing it?**

- GitOps: Git repository is the **single source of truth** for desired cluster state
- No `kubectl apply` in CI — CI only pushes to git; ArgoCD handles the apply
- Benefits: full audit trail (every deployment is a git commit), easy rollback (`git revert`), drift detection
- ArgoCD **self-heal**: if someone manually `kubectl apply`s a change, ArgoCD reverts it to match git

**Q: What is ArgoCD's sync policy? What is the difference between manual and auto sync?**

| Setting | Behavior |
|---|---|
| Manual sync | ArgoCD shows drift but waits for human to click "Sync" |
| Auto sync | ArgoCD syncs automatically when git changes detected |
| Self-heal | Reverts manual changes to cluster back to git state |
| Prune | Deletes resources removed from git (disabled by default — dangerous) |

**Q: How do you do a rollback in this CI/CD setup?**

- **Helm rollback**: `helm rollback retail-ui 3` — reverts to revision 3 in Helm history
- **ArgoCD rollback**: Select previous git commit hash in ArgoCD UI → sync to that revision
- **Kubernetes rollback**: `kubectl rollout undo deployment/retail-ui` — reverts to previous ReplicaSet
- **GitOps rollback** (cleanest): `git revert <commit>` + push → ArgoCD auto-deploys the reverted state, maintaining full audit trail

---

## CROSS-CUTTING SCENARIO QUESTIONS

**Q: Your Catalog service (Go + RDS MySQL) is returning 500 errors in production. Walk through your investigation.**

1. Check ArgoCD — is the deployment healthy? Any recent sync?
2. `kubectl get pods -n retail` — are pods in CrashLoopBackOff or Running?
3. `kubectl logs -l app=catalog -n retail --tail=100` — look for DB connection errors
4. Check X-Ray traces — which span is failing? Is it the DB call?
5. Check CloudWatch Logs Insights — query for ERROR level logs in last 15 min
6. `kubectl exec` into pod → test RDS connectivity: `mysql -h <rds-endpoint> -u user -p`
7. Check RDS CloudWatch metrics — CPU, connections, `DatabaseConnections` at limit?
8. Check Secrets Manager — did the DB password rotate? Is the secret updated in the pod?

**Q: A new Karpenter node was provisioned but your pod is still Pending. What do you check?**

1. `kubectl describe pod <pod>` — look at Events section for the exact reason
2. Common causes:
   - **Taints/tolerations mismatch** — node has a taint the pod doesn't tolerate
   - **Node selector / affinity** — pod requires labels the node doesn't have
   - **Resource request too high** — pod requests 8Gi but node only has 6Gi allocatable
   - **PVC not bound** — pod waiting on a PVC that's in Pending state
   - **Image pull failure** — ECR auth issue (check node IAM role has `ecr:GetAuthorizationToken`)
3. `kubectl get nodeclaim -A` — check Karpenter NodeClaim status

**Q: How do you ensure zero downtime deployments for the Orders service?**

- `strategy.rollingUpdate`: `maxUnavailable: 0`, `maxSurge: 1` — always have full capacity during rollout
- `readinessProbe` — new pods only receive traffic once healthy
- `PodDisruptionBudget`: `minAvailable: 2` — prevents too many pods being down simultaneously
- Database migrations run as a pre-upgrade Helm hook (separate Job) before pods rollout
- Feature flags for risky schema changes — decouple deployment from feature release

---

## 1. Linux / Shell

**Q: How to extract only the last column from each line in a text file?**

```bash
awk '{print $NF}' sample.txt
```

---

## 2. AI / Concepts

**Q: What is an Agent and Agentic AI?**

- An **agent** perceives its environment, makes decisions autonomously, and takes actions to achieve a goal.
- **Agentic AI** goes beyond Q&A — it plans, uses tools, maintains memory, and self-corrects to complete multi-step tasks autonomously.

---

## 3. Docker

**Q: What is a Docker image?**

- A lightweight, immutable, read-only template used to create containers
- Contains application code, runtime, libraries, environment variables, and config files
- Built in layers using a `Dockerfile` and stored in registries (Docker Hub, ECR, JFrog, etc.)
- Each `RUN`, `COPY`, `ADD` instruction creates a new layer on top of the previous one

**Q: What is containerization?**

- Packaging an application and its dependencies into a single isolated unit (container) that runs consistently across any environment
- Containers share the host OS kernel — unlike VMs which run a full OS
- Enables portability: same container runs on dev laptop, CI server, and production

| Feature | Container | VM |
|---|---|---|
| Startup | Seconds | Minutes |
| Size | MBs | GBs |
| Isolation | Process-level | OS-level |
| Overhead | Low | High |

---

**Q: Why multi-stage Docker builds?**

- Keeps final image small and secure by separating build environment from runtime environment.
- Build tools, source code, and dev dependencies are discarded — only the final artifact is copied.

**Q: What is ENTRYPOINT and CMD in Docker?**

- `CMD` — default command/args, easily overridden at `docker run`
- `ENTRYPOINT` — fixed executable, args from `docker run` are appended to it
- When both are set: `ENTRYPOINT` runs with `CMD` as its arguments

**Q: Given this Dockerfile, what is the output?**
```dockerfile
FROM ubuntu
ENTRYPOINT ["ls"]
CMD ["echo", "test"]
```
**Answer:** Runs `ls echo test` — lists files named `echo` and `test`, which don't exist, so outputs error: `No such file or directory`

**Q: You have a 10-line Dockerfile and a 20-line Dockerfile — which do you prefer and why?**

- Prefer the **10-line** Dockerfile — but only if it follows best practices correctly
- Fewer `RUN` instructions = fewer layers = smaller image
- Chain commands with `&&` to reduce layers: `RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*`
- A bad 10-line (single-stage, bloated base) is worse than a good 20-line multi-stage
- **Real answer:** Line count is secondary — what matters is image size, security, cache efficiency, and readability

| Priority | What Matters |
|---|---|
| Fewer layers | Chain `RUN` commands with `&&` |
| Small base image | Use `alpine` or `distroless` |
| Multi-stage | Discard build tools from final image |
| Cache order | Copy `package.json` before source code |
| Non-root user | Security best practice |
| `.dockerignore` | Exclude `node_modules`, `.git` |

---

## 4. Terraform

**Q: What is the Terraform state file and why is it used?**

- A JSON file tracking the real-world state of infrastructure
- Maps `.tf` config to actual cloud resource IDs
- Used for drift detection, dependency mapping, idempotency, and performance

**Q: How to set up different environments (dev, prod, etc.) using Terraform?**

Three approaches:
1. **Workspaces** — built-in, simple but low isolation
2. **Directory-per-environment** — recommended, full isolation, separate state
3. **`.tfvars` files** — one codebase, different variable files per env

---

## 5. JFrog

**Q: What is JFrog SSOT (Single Source of Truth)?**

- JFrog Artifactory acts as the central hub for all artifacts
- Proxies and caches external registries (DockerHub, npm, PyPI)
- Promotes artifacts (dev → staging → prod) without rebuilding
- Provides security scanning (Xray), immutability, audit trail, and license compliance

---

## 6. Git

**Q: What is git squash?**

- Combining multiple commits into one clean commit
- Done via `git rebase -i`, soft reset, or "Squash and merge" on GitHub
- Only do on your own branch — rewrites history

**Q: What is git cherry-pick?**

- Applies a specific commit from one branch onto another
- Does not merge the whole branch — just copies selected commits
- Common for hotfixes, wrong-branch commits, partial releases

---

## 7. Terraform — Drift & Incident Scenarios

**Q: A team member manually deleted an EC2 instance. What happens and how does Terraform act?**

- The EC2 is gone from AWS but still recorded in the **statefile** — this is called **drift**
- `terraform plan` detects the missing resource and shows it will be **recreated**
- `terraform apply` **recreates** the EC2 instance to match desired state
- The new instance gets a new ID; statefile is updated accordingly

| Scenario | Terraform Behaviour |
|---|---|
| Resource deleted outside Terraform | Recreates it on next `apply` |
| Resource modified outside Terraform | Reverts changes on next `apply` |
| `terraform refresh` | Syncs statefile with real infra |
| `terraform plan -refresh-only` | Shows drift without changing anything |

**Prevention:** Restrict IAM permissions, enable AWS Config / CloudTrail, use Sentinel policies.

---

## 8. Security — Secrets Manager Access

**Q: A developer gains access to credentials in Secrets Manager. How do you detect and respond?**

- **AWS CloudTrail** — logs every `GetSecretValue` API call with user identity and timestamp
- **CloudWatch Alarm** — alert when sensitive secrets are accessed unexpectedly
- **GuardDuty** — detects anomalous access patterns automatically
- **Secrets Manager Resource Policy** — audit who has `GetSecretValue` permission

**Immediate Response:**
1. Rotate the secret immediately (invalidates compromised credentials)
2. Revoke developer's IAM access temporarily
3. Audit CloudTrail to see what was accessed
4. Apply least-privilege IAM policies

---

## 9. Kubernetes — Network Policy (Pod Access Restriction)

**Q: You have 3 pods — API, Database, UI. How do you restrict access so UI cannot reach Database directly?**

**Goal:**
```
UI → API → Database   ✅
UI → Database         ❌
```

**Solution: Kubernetes NetworkPolicy**

Label your pods (`app: ui`, `app: api`, `app: database`), then apply policies:

```yaml
# Allow only API to reach Database
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-api-to-database
spec:
  podSelector:
    matchLabels:
      app: database
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: api
```

```yaml
# Allow only UI to reach API
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-ui-to-api
spec:
  podSelector:
    matchLabels:
      app: api
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: ui
```

```yaml
# Default deny all (best practice — start with this)
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

> **Note:** Requires a CNI plugin that enforces NetworkPolicy — **Calico**, **Cilium**, or **Weave**.

---

## 10. Networking

**Q: What happens when you click on instagram.com? What network protocols are used?**

| Step | What Happens | Protocol |
|---|---|---|
| DNS Resolution | Resolves domain to IP | UDP/TCP port 53 |
| TCP Handshake | Establishes connection | TCP |
| TLS Handshake | Encrypts the connection | TLS 1.3 / port 443 |
| HTTP Request | Browser requests page | HTTP/2 or HTTP/3 |
| CDN / Load Balancer | Routes to nearest server | — |
| Response + Render | HTML/CSS/JS parsed & displayed | — |

---

## 11. Linux — File Permissions

**Q: What is chmod 753 and chmod 755?**

chmod uses octal notation: **Owner | Group | Others** (4=read, 2=write, 1=execute)

| chmod | Owner | Group | Others | Symbolic |
|---|---|---|---|---|
| `755` | rwx (7) | r-x (5) | r-x (5) | -rwxr-xr-x |
| `753` | rwx (7) | r-x (5) | -wx (3) | -rwxr-x-wx |

- `755` — owner can read/write/execute; group & others can read & execute. **Most common for scripts and directories.**
- `753` — others can write & execute but NOT read. Rare in practice.

Common permission values:
| chmod | Use Case |
|---|---|
| `755` | Scripts, directories |
| `644` | Regular files |
| `600` | SSH keys, secrets |
| `400` | Read-only `.pem` files |
| `777` | ⚠️ Avoid — full access to everyone |

---

## 12. Terraform — Modules

**Q: What are Terraform modules?**

- A **module** is a reusable, self-contained package of Terraform config — like a function in programming
- Avoids code duplication (DRY) across environments
- Same module called with different variables for dev/prod

```hcl
# modules/ec2/main.tf
resource "aws_instance" "this" {
  ami           = var.ami
  instance_type = var.instance_type
}

# dev/main.tf
module "web_server" {
  source        = "../modules/ec2"
  instance_type = "t2.micro"
}

# prod/main.tf
module "web_server" {
  source        = "../modules/ec2"
  instance_type = "t3.large"
}
```

---

## 13. GitHub Actions

**Q: What is GitHub Actions?**

- CI/CD platform built into GitHub — automates workflows triggered by events (push, PR, schedule)
- Key concepts: Workflow (YAML file), Trigger (`on:`), Job, Step, Action, Runner

**Q: What types of Actions are used in a project?**

| Action | Purpose |
|---|---|
| `actions/checkout@v4` | Checkout code |
| `actions/setup-node@v4` | Setup Node.js runtime |
| `docker/build-push-action@v5` | Build & push Docker image |
| `aws-actions/configure-aws-credentials@v4` | AWS auth via OIDC |
| `aws-actions/amazon-ecr-login@v2` | Login to ECR |
| `hashicorp/setup-terraform@v3` | Setup Terraform |
| `actions/cache@v4` | Cache dependencies |
| `slackapi/slack-github-action@v1` | Slack notifications |

**Q: What are reusable workflows in GitHub Actions?**

- Define once with `workflow_call` trigger, call from any repo
- Accepts `inputs` and `secrets` as parameters
- Eliminates duplicating CI/CD YAML across multiple repos

**Q: What are the stages to deploy an application using GitHub Actions?**

```
Code Push
  ↓
Stage 1: CI — lint, unit tests, SAST scan
  ↓
Stage 2: Build Docker image + security scan (Trivy)
  ↓
Stage 3: Push image to registry (ECR / JFrog)
  ↓
Stage 4: Deploy to Dev/Staging → integration tests
  ↓
Stage 5: Manual approval → Deploy to Production
  ↓
Stage 6: Notify (Slack / Teams)
```

**Q: How do you use GitHub Actions in your project?**

- PR raised → `ci.yml` runs lint + tests automatically
- Merge to `main` → image built, pushed to ECR, deployed to dev
- QA passes → manual approval in GitHub → deployed to prod
- Slack notification on success/failure

---

## 14. Cloud Services Used

**Q: What AWS cloud services have you used?**

| Service | Purpose |
|---|---|
| EC2 | Virtual machines |
| EKS | Managed Kubernetes |
| ECR | Docker image registry |
| S3 | Object storage (artifacts, state, static assets) |
| RDS | Managed relational database |
| IAM | Access management and roles |
| Secrets Manager | Store and rotate credentials |
| CloudWatch | Logging, monitoring, alarms |
| CloudTrail | Audit trail for API calls |
| VPC | Network isolation |
| ALB / NLB | Load balancing |
| Route53 | DNS management |
| Lambda | Serverless functions |
| SQS / SNS | Messaging and notifications |
| GuardDuty | Threat detection |

---

## 15. Kubernetes — Basics

**Q: What is the difference between a Pod and a Container? How do they work?**

- A **Container** is a running instance of a Docker image — it holds the app process
- A **Pod** is the smallest deployable unit in Kubernetes; it wraps one or more containers that share the same **network namespace** (IP, ports) and **storage volumes**

```
Pod
 ├── Main container (app)
 └── Sidecar container (e.g., log collector)
       └── Both share localhost network & volumes
```

- Kubernetes manages Pods, not containers directly
- If a container crashes inside a Pod, Kubernetes restarts it within the same Pod
- Containers in the same Pod communicate via `localhost`; different Pods communicate via Services

| Aspect | Container | Pod |
|---|---|---|
| Unit | Docker/OCI image instance | Kubernetes scheduling unit |
| Networking | Own network stack | Shared network namespace |
| Managed by | Docker daemon | Kubelet (via API server) |
| Lifecycle | Independent | All containers start/stop together |

**Q: What is Kubelet?**

- An agent that runs on every **worker node** in a Kubernetes cluster
- Watches the API server for Pod specs assigned to its node
- Ensures containers described in those specs are running and healthy
- Reports node and Pod status back to the control plane
- Does **not** manage containers not created by Kubernetes

```
Control Plane (API Server)
        ↓  Pod spec assigned to node
    Kubelet (on worker node)
        ↓  Instructs container runtime
   Container Runtime (containerd / Docker)
        ↓
    Container running inside Pod
```

---

## 16. CI/CD Troubleshooting

**Q: The CI/CD pipeline passed, but the application is down. How do you investigate?**

**Step-by-step approach:**

1. **Check Pod status:**
   ```bash
   kubectl get pods -n <namespace>
   kubectl describe pod <pod-name> -n <namespace>
   ```
2. **Check container logs:**
   ```bash
   kubectl logs <pod-name> -n <namespace>
   kubectl logs <pod-name> --previous   # if pod restarted
   ```
3. **Check cluster events:**
   ```bash
   kubectl get events -n <namespace> --sort-by='.lastTimestamp'
   ```
4. **Check service/ingress routing:**
   ```bash
   kubectl get svc,ingress -n <namespace>
   ```
5. **Check deployment rollout status:**
   ```bash
   kubectl rollout status deployment/<name> -n <namespace>
   ```
6. Check **resource limits** — OOMKilled, CPU throttling
7. Check **config/secrets** — missing env vars, wrong DB connection strings
8. Check **health/readiness probes** — app may be running but failing probe checks

> Pipeline passing means the **build and deploy succeeded** — it does not guarantee the app is healthy at runtime. Runtime failures (bad config, OOM, DB unreachable) happen after deployment.

---

## 17. Change Request (CR) — ITSM / DevOps Process

**Q: What is a Change Request (CR) and why is it used?**

- A **Change Request** is a formal, documented proposal to modify any component of an IT system, infrastructure, application, or process
- Part of **ITIL Change Management** — ensures changes are reviewed, approved, and executed in a controlled way
- Prevents unplanned outages, ensures a rollback plan exists, and maintains an audit trail

**Fields in a CR:**
| Field | Description |
|---|---|
| CR ID | Unique identifier (e.g., CHG0012345) |
| Description | What change is being made and why |
| Risk level | Low / Medium / High / Critical |
| Rollback plan | How to undo the change if it fails |
| Testing plan | How you verified it works |
| Maintenance window | When it will be executed |
| Approvers | Manager, CAB, Architect sign-off |
| Impacted services | What systems are affected |

**Q: What are the types of Change Requests?**

| Type | Description | Approval |
|---|---|---|
| **Standard** | Pre-approved, low-risk, routine (e.g., password reset, config patch) | No — pre-approved template |
| **Normal** | Planned change, goes through full CAB review | Yes — CAB approval required |
| **Emergency** | Urgent production fix — hotfix for outage | Expedited, post-review done later |

> **CAB** = Change Advisory Board — a group (Ops, Security, Dev Leads) that reviews and approves Normal CRs.

**Q: Where does a CR fit in a DevOps CI/CD pipeline?**

```
Developer merges to main
  ↓
GitHub Actions builds image + pushes to ECR    ← automated (no CR needed)
  ↓
ArgoCD deploys to Dev / Staging                ← automated (no CR needed)
  ↓
QA sign-off + manual approval gate
  ↓
Engineer raises CR in ServiceNow/Jira          ← CR created HERE
  ↓
CAB approves → maintenance window opens
  ↓
ArgoCD syncs to Production                     ← CR reference logged
  ↓
CR closed (success) or rollback triggered (failure)
```

**Q: In which DevOps scenarios are CRs raised?**

| Scenario | CR Type |
|---|---|
| Production code deployment | Normal CR |
| EKS version upgrade | Normal CR |
| RDS schema migration | Normal CR (high risk) |
| Terraform infra change (VPC, SG) | Normal CR |
| ACM certificate rotation | Standard CR |
| Emergency hotfix for production outage | Emergency CR |
| IAM policy modification | Normal CR (security review) |

**Q: What tools are used to manage CRs?**

| Tool | Used by |
|---|---|
| **ServiceNow** | Enterprise standard — most common |
| **Jira Service Management** | Dev-centric teams |
| **BMC Remedy** | Legacy enterprises |
| **PagerDuty** | Emergency CR tracking and on-call coordination |

**Interview answer framework:**
> "In my project, every production deployment required a Normal CR raised in ServiceNow. The pipeline automated everything up to staging — build, test, push to ECR, deploy to staging. Once QA signed off, I raised a CR with the deployment plan, rollback steps (Helm rollback or ArgoCD revert to previous commit), and maintenance window. After CAB approval, ArgoCD synced to prod during the window. For hotfixes, we raised an Emergency CR, deployed immediately, and did a post-review within 24 hours."

---

## 18. Behavioral — Automation

**Q: Have you automated anything?**

**Framework for answering:**
> "Yes — I automated [specific task] using [tool/script]. The problem was [manual pain point]. I built [what you created], which reduced [time/errors/manual steps] by [X%]."

**Common examples:**
| What was automated | Tool used |
|---|---|
| CI/CD pipelines (build, test, deploy) | GitHub Actions, Jenkins |
| Infrastructure provisioning | Terraform, Ansible |
| Auto-scaling policies | Kubernetes HPA/VPA |
| Security scanning on every PR | Trivy, Snyk in CI |
| Scheduled cleanup/backup jobs | Shell scripts + cron |
| Slack/email alerts on deployment | GitHub Actions + Slack webhook |
| Rotating secrets automatically | AWS Secrets Manager rotation Lambda |

---

## 19. Terraform — Deep Dive (All Core Concepts)

### Basics & CLI

**Q: What are the core Terraform CLI commands and what does each do?**

| Command | Purpose |
|---|---|
| `terraform init` | Downloads provider plugins, sets up backend, initializes modules |
| `terraform validate` | Checks syntax and internal consistency — does NOT contact cloud APIs |
| `terraform plan` | Shows what changes will be made — dry run, nothing is modified |
| `terraform apply` | Executes the plan — creates/modifies/destroys real resources |
| `terraform destroy` | Destroys all resources managed by the config |
| `terraform fmt` | Auto-formats `.tf` files to canonical style |
| `terraform output` | Prints output values from state |
| `terraform show` | Human-readable view of state or plan file |
| `terraform state list` | Lists all resources tracked in state |

**Q: What is the difference between `terraform validate` and `terraform plan`?**

- `validate` — checks only syntax and config structure; works offline, no AWS credentials needed
- `plan` — calls cloud APIs to compare desired state vs real state; requires credentials and network access
- Always run `validate` first (fast), then `plan` (slower, requires auth)

---

### Language Syntax

**Q: What is a Block in Terraform HCL? Name the main block types.**

```hcl
resource "aws_instance" "web" {   # block type = resource, labels = "aws_instance" + "web"
  ami           = "ami-0abc123"   # argument
  instance_type = "t3.micro"      # argument
}
```

| Block Type | Purpose |
|---|---|
| `terraform {}` | Settings block — backend, required providers, Terraform version |
| `provider {}` | Configures a cloud provider (AWS, Azure, GCP) |
| `resource {}` | Declares a managed infrastructure resource |
| `variable {}` | Declares an input variable |
| `output {}` | Declares an output value |
| `locals {}` | Defines local computed values |
| `data {}` | Reads existing infrastructure (read-only) |
| `module {}` | Calls a child module |

**Q: What is the Terraform Settings Block (`terraform {}`)? What goes inside it?**

```hcl
terraform {
  required_version = ">= 1.6.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "my-tf-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```
- `required_version` — pins Terraform CLI version to avoid breaking changes
- `required_providers` — pins provider version; `~> 5.0` means `>= 5.0, < 6.0`
- `backend` — where state is stored (local by default, S3 for teams)

---

### Resource Meta-Arguments

**Q: What are Meta-Arguments in Terraform? List them all.**

Meta-arguments are special arguments supported by every `resource` block — not provider-specific:

| Meta-Argument | Purpose |
|---|---|
| `depends_on` | Explicit dependency — force ordering when implicit dependency isn't detected |
| `count` | Create N identical copies of a resource |
| `for_each` | Create one resource per map key or set element |
| `provider` | Use a non-default provider alias |
| `lifecycle` | Control create/destroy behavior (`create_before_destroy`, `prevent_destroy`, `ignore_changes`) |

**Q: When do you use `depends_on` vs implicit dependency?**

- Terraform automatically detects dependencies when you reference one resource's attribute in another (`aws_subnet.id`)
- `depends_on` is needed when the dependency exists at the **API level** but not in HCL — e.g., an IAM policy must be attached before an EC2 instance launches (but the EC2 doesn't reference the policy ARN directly)

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abc123"
  instance_type = "t3.micro"
  depends_on    = [aws_iam_role_policy_attachment.web_policy]
}
```

**Q: What is the difference between `count` and `for_each`? When do you prefer each?**

| Feature | `count` | `for_each` |
|---|---|---|
| Input type | Number | Map or Set of strings |
| Resource address | `aws_instance.web[0]`, `[1]` | `aws_instance.web["prod"]` |
| Deletion behavior | Deleting index 0 shifts all others — causes mass destroy/recreate | Removing one key only affects that resource |
| Best for | Identical resources (N copies of the same thing) | Resources with unique names/configs |

```hcl
# count — 3 identical subnets
resource "aws_subnet" "public" {
  count      = 3
  cidr_block = cidr(var.vpc_cidr, count.index, 8)
}

# for_each — subnets with unique names
resource "aws_subnet" "public" {
  for_each   = toset(["us-east-1a", "us-east-1b", "us-east-1c"])
  cidr_block = "10.0.${index(keys(each.key), each.key)}.0/24"
  availability_zone = each.value
}
```

---

### Input Variables

**Q: What are all the ways to assign a value to a Terraform input variable? (Priority order)**

| Priority | Method | Example |
|---|---|---|
| 1 (lowest) | Default in `variable {}` block | `default = "t3.micro"` |
| 2 | `terraform.tfvars` file | `instance_type = "t3.small"` |
| 3 | `*.auto.tfvars` files | auto-loaded, no flag needed |
| 4 | `-var-file` flag | `terraform apply -var-file=prod.tfvars` |
| 5 | `-var` flag | `terraform apply -var="instance_type=t3.large"` |
| 6 | Prompt at runtime | No default set → Terraform prompts interactively |
| 7 (highest) | Environment variable | `export TF_VAR_instance_type=t3.xlarge` |

**Q: What is the difference between `terraform.tfvars` and `*.auto.tfvars`?**

- Both are automatically loaded — no flag needed
- `terraform.tfvars` — always loaded if present (single file)
- `*.auto.tfvars` — any file matching this pattern is auto-loaded (allows per-feature files: `network.auto.tfvars`, `compute.auto.tfvars`)
- Order matters: `terraform.tfvars` loads first, then `*.auto.tfvars` files alphabetically

**Q: What are List and Map variable types? Give examples.**

```hcl
# List — ordered, same type
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

# Map — key-value pairs
variable "instance_types" {
  type = map(string)
  default = {
    dev  = "t3.micro"
    staging = "t3.small"
    prod = "t3.large"
  }
}

# Usage
instance_type = var.instance_types["prod"]
subnet        = var.availability_zones[0]
```

**Q: What are Sensitive Input Variables? How do they protect secrets?**

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```
- Marked as `sensitive = true` — Terraform **redacts** the value in `plan` and `apply` output (shows `(sensitive value)`)
- Value is still stored in state file (plain text) — use remote state with encryption
- Does **not** prevent the value from being used — just hides it from terminal output/logs

---

### Output Values & Local Values

**Q: What are Output Values? When are they useful?**

```hcl
output "alb_dns_name" {
  value       = aws_lb.main.dns_name
  description = "DNS name of the Application Load Balancer"
}
```
- Exposes resource attributes after `apply` — readable with `terraform output`
- Essential for **module chaining** — child module outputs become inputs to parent
- Used in CI/CD to pass ALB DNS, EKS cluster endpoint, RDS endpoint to next steps

**Q: What are Local Values (`locals`)? How are they different from variables?**

```hcl
locals {
  name_prefix = "${var.project}-${var.environment}"
  common_tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

resource "aws_instance" "web" {
  tags = local.common_tags
}
```
- `locals` = computed, reusable expressions within a module — like constants in programming
- `variable` = external input (user-supplied); `local` = internal computed value (no external input)
- Avoids repeating the same expression in 10 places

---

### Provisioners

**Q: What are Terraform Provisioners? What are the three types?**

Provisioners run scripts on resources **after creation** — used as a last resort when no native Terraform resource exists:

| Provisioner | What it does |
|---|---|
| `file` | Copies a file or directory from local machine to remote resource |
| `local-exec` | Runs a command on the **local machine** where Terraform is running |
| `remote-exec` | Runs a command on the **remote resource** (via SSH or WinRM) |

```hcl
resource "aws_instance" "web" {
  # ...

  provisioner "local-exec" {
    command = "echo ${self.private_ip} >> inventory.txt"
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx"
    ]
    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip
    }
  }
}
```

> **Best practice:** Avoid provisioners when possible — use `user_data`, AWS Systems Manager, or Ansible instead. Provisioners break idempotency.

**Q: What is a Null Resource? When do you use it?**

```hcl
resource "null_resource" "trigger_ansible" {
  triggers = {
    instance_id = aws_instance.web.id
  }

  provisioner "local-exec" {
    command = "ansible-playbook -i ${aws_instance.web.public_ip}, playbook.yml"
  }
}
```
- A `null_resource` has no real cloud infrastructure — it's a container for provisioners
- `triggers` map: resource re-runs if any trigger value changes
- Common use: run Ansible after EC2 creation, send webhook, invoke Lambda

---

### Functions

**Q: What is the `file()` function in Terraform?**

```hcl
resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/id_rsa.pub")  # reads file content as string
}
```
- Reads the content of a file at the given path and returns it as a string
- Evaluated at plan time — file must exist on the machine running Terraform

**Q: What are `toset()`, `tomap()`, and `keys()` functions used for?**

| Function | Purpose | Example |
|---|---|---|
| `toset(list)` | Converts a list to a set (removes duplicates, unordered) | Used with `for_each` — `for_each = toset(var.zones)` |
| `tomap(object)` | Converts an object to a map type | Type-converts for `for_each` compatibility |
| `keys(map)` | Returns a sorted list of map keys | `keys({a=1, b=2})` → `["a", "b"]` |

```hcl
# toset — for_each requires set or map, not list
resource "aws_subnet" "pub" {
  for_each          = toset(var.availability_zones)
  availability_zone = each.value
}

# keys — iterate only over map keys
output "env_names" {
  value = keys(var.instance_types)  # ["dev", "prod", "staging"]
}
```

---

### For Loops & Splat Operators

**Q: How do For Loops work in Terraform? Show list and map examples.**

```hcl
# For loop with list — returns list
output "upper_zones" {
  value = [for az in var.availability_zones : upper(az)]
  # ["US-EAST-1A", "US-EAST-1B"]
}

# For loop with map — returns map
output "instance_ids" {
  value = {for k, v in var.instance_types : k => upper(v)}
  # {dev = "T3.MICRO", prod = "T3.LARGE"}
}

# For loop with filter (if condition)
output "prod_only" {
  value = [for env, type in var.instance_types : type if env == "prod"]
}
```

**Q: What is the Splat Operator? What is the difference between legacy `.*` and latest `[*]`?**

```hcl
# Legacy splat — works only on lists from count
output "all_private_ips" {
  value = aws_instance.web.*.private_ip   # legacy — .*
}

# Latest splat — works on any list/set/tuple
output "all_private_ips" {
  value = aws_instance.web[*].private_ip  # modern — [*]
}
```
- Both iterate over all instances and collect a specific attribute into a list
- `[*]` (latest) is preferred — more consistent behavior with `for_each` and nested objects
- `.*` (legacy) can produce unexpected results with null values

---

### Modules

**Q: What is the difference between a Public Registry module and a Local module?**

```hcl
# Public Registry module (Terraform Registry)
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.2"
  name    = "my-vpc"
  cidr    = "10.0.0.0/16"
}

# Local module
module "ec2" {
  source        = "../modules/ec2"
  instance_type = "t3.micro"
}
```

| Feature | Public Registry | Local Module |
|---|---|---|
| Source | `registry.terraform.io` | Local file path |
| Versioning | Yes — `version = "5.1.2"` | No — tied to file system |
| Maintenance | Community/HashiCorp maintained | You own and maintain it |
| Update command | `terraform init -upgrade` | Edit files directly |

**Q: How do you upgrade a module version? What command do you use?**

```bash
# Update version in .tf file:
# version = "5.1.2"  →  version = "5.2.0"

terraform init -upgrade   # downloads new version, updates .terraform.lock.hcl
terraform plan            # review changes
terraform apply
```
- `.terraform.lock.hcl` pins exact provider and module versions — commit this file to git
- `-upgrade` overrides the lock file to fetch newer versions

**Q: What is the `random` provider / Random Resource used for?**

```hcl
resource "random_id" "suffix" {
  byte_length = 4
}

resource "aws_s3_bucket" "state" {
  bucket = "my-tf-state-${random_id.suffix.hex}"  # e.g., "my-tf-state-a3f2b1c4"
}
```
- Generates random values: `random_id`, `random_string`, `random_password`, `random_integer`
- Common use: ensure globally unique S3 bucket names, RDS identifiers, avoid naming conflicts
- Value is stable across plans — only changes if the resource is destroyed and recreated

---

## 20. ArgoCD — Multi-Cluster & Configuration Management

**Q: You have 50 Kubernetes clusters, each with its own config. Will you create configuration files manually or use Helm templates?**

- Never manually — use **ArgoCD ApplicationSet** with Helm templates
- **ApplicationSet** is an ArgoCD controller that generates multiple `Application` CRs from a single template using generators

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: retail-store-all-clusters
  namespace: argocd
spec:
  generators:
    - list:
        elements:
          - cluster: cluster-us-east-1
            env: prod
            region: us-east-1
          - cluster: cluster-eu-west-1
            env: prod
            region: eu-west-1
          # ... 48 more clusters
  template:
    metadata:
      name: '{{cluster}}-retail-store'
    spec:
      source:
        repoURL: https://github.com/org/gitops-repo
        path: charts/retail-store
        helm:
          valueFiles:
            - values.yaml
            - values-{{env}}.yaml       # env-specific values
            - values-{{cluster}}.yaml   # cluster-specific overrides
      destination:
        server: 'https://{{cluster}}.example.com'
        namespace: retail
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
```

- One `ApplicationSet` → generates 50 ArgoCD `Application` objects automatically
- Adding a new cluster = add one entry to the `elements` list + create `values-<cluster>.yaml` in git
- **Cluster Generator** can also auto-discover registered ArgoCD clusters — fully dynamic

**Q: How do you manage configurations across all clusters using ArgoCD?**

```
GitOps Repository Structure:
  charts/
    retail-store/
      Chart.yaml
      templates/          ← Helm templates (same for all clusters)
      values.yaml         ← global defaults
      values-prod.yaml    ← prod environment overrides
      values-dev.yaml     ← dev environment overrides
      values-cluster-us-east-1.yaml   ← cluster-specific (region, endpoints)
      values-cluster-eu-west-1.yaml
  apps/
    applicationset.yaml   ← single file manages all 50 clusters
```

- Helm templates are shared across all clusters — DRY principle
- Per-cluster values files override only what differs (cluster endpoints, region-specific config)
- CI pipeline updates `image.tag` in `values.yaml` → ArgoCD syncs all clusters
- `selfHeal: true` ensures manual changes to any cluster are reverted to git state

---

## 21. Kubernetes Cluster Patching & Upgrades

**Q: Do you do patching for your Kubernetes cluster? What is the process?**

Two types of patching:

**1. OS/Node Patching (worker nodes):**
- EKS managed node groups — AWS releases patched AMIs
- Process: update `ami_release_version` in Terraform → `terraform apply` → node group performs rolling replacement
- New nodes provisioned with patched AMI → old nodes drained and terminated one by one
- Zero downtime if `PodDisruptionBudgets` are set correctly

**2. Kubernetes Version Patching (minor/patch versions):**
- EKS supports in-place control plane upgrades
- Process: update `cluster_version` in Terraform → `terraform apply` → AWS upgrades control plane first, then node groups

**Q: Walk me through the full patching process from test to prod — steps, approvals, and timeline.**

```
DAY 1-2: Preparation
  ├── Review AWS EKS patch release notes / CVE advisories
  ├── Check add-on compatibility matrix (CSI driver, LB controller, etc.)
  ├── Raise Normal CR in ServiceNow with:
  │     - Patch details and CVEs being addressed
  │     - Risk assessment (Low/Medium/High)
  │     - Rollback plan
  │     - Maintenance windows for each environment
  └── CAB review and approval

DAY 3: DEV cluster (non-prod)
  ├── Update Terraform config (ami_release_version or cluster_version)
  ├── Run terraform plan → review node replacement plan
  ├── terraform apply in maintenance window
  ├── Verify: kubectl get nodes (all Ready, new version)
  ├── Run smoke tests — check all pods running, app accessible
  └── Monitor for 24 hours

DAY 4-5: STAGING cluster
  ├── Same Terraform change applied
  ├── Full regression test suite run
  ├── Performance / load test
  └── Sign-off from QA team

DAY 6-7: PRODUCTION cluster
  ├── Maintenance window (typically off-peak: Sunday 2AM–4AM)
  ├── terraform apply — rolling node replacement
  ├── Monitor CloudWatch metrics, ArgoCD health, app error rates
  ├── Close CR as successful
  └── Post-implementation review

Total timeline: ~1 week (urgent security patches: 24-48 hours)
```

**Q: A vulnerability patch comes in. What approvals are needed and how does the process go?**

| Step | Action | Owner |
|---|---|---|
| 1 | Security team identifies CVE — assess severity (CVSS score) | Security |
| 2 | Check if CVE affects your cluster version/AMI | DevOps |
| 3 | Raise **Emergency CR** (if Critical/High) or Normal CR (Medium/Low) | DevOps |
| 4 | Emergency CR approval: Security Lead + Engineering Manager (no full CAB wait) | Security + EM |
| 5 | Patch dev/staging within 24h (Critical), 72h (High) | DevOps |
| 6 | Prod patch in emergency maintenance window | DevOps |
| 7 | Verify patch applied: check node AMI version, run vulnerability scan (Trivy/AWS Inspector) | DevOps + Security |
| 8 | Post-implementation review and close CR | All |

- **Critical CVE** (CVSS ≥ 9.0): patch within 24 hours, Emergency CR
- **High CVE** (CVSS 7.0–8.9): patch within 72 hours, Emergency CR
- **Medium/Low**: next planned maintenance window, Normal CR

---

## 22. EKS Version Upgrades

**Q: Have you done EKS version upgrades? Who manages the master (control plane)?**

- **AWS manages the EKS control plane (master nodes)** — you never SSH into or patch master nodes directly
- You only manage: worker nodes (node groups), add-ons, and the Kubernetes version upgrade trigger

**EKS upgrade process:**
```
1. Check EKS upgrade docs — breaking changes between versions
2. Upgrade add-ons to versions compatible with new K8s version:
   - aws-ebs-csi-driver, aws-load-balancer-controller, kube-proxy, coredns, vpc-cni
3. Update cluster_version in Terraform:
   cluster_version = "1.29"  →  "1.30"
4. terraform apply → AWS upgrades control plane (15-20 min, zero downtime)
5. Update node group version (Terraform ami_release_version)
6. Rolling node replacement — old nodes drained, new nodes with 1.30 kubelet join
7. Verify: kubectl version, kubectl get nodes
```

> EKS supports only **one minor version upgrade at a time** — to go from 1.27 to 1.30, you must do 1.27 → 1.28 → 1.29 → 1.30.

---

## 23. Kubernetes Node Scaling & Troubleshooting

**Q: How do you increase the number of nodes in a Kubernetes cluster?**

**With Karpenter (used in this project):**
- No manual action needed — Karpenter automatically provisions nodes when pods are Pending
- To increase baseline capacity: adjust `NodePool` limits or deploy more workloads

**With Managed Node Groups (manual/Terraform):**
```hcl
# Update desired_size in Terraform
resource "aws_eks_node_group" "workers" {
  scaling_config {
    desired_size = 10   # was 5
    min_size     = 3
    max_size     = 20
  }
}
```
```bash
terraform apply   # triggers ASG to add nodes
kubectl get nodes # verify new nodes join as Ready
```

**Q: You tried to increase nodes and it's failing. How do you troubleshoot?**

| Symptom | Likely Cause | How to Check |
|---|---|---|
| Nodes stuck in `NotReady` | kubelet startup issue | `kubectl describe node <node>` → check conditions |
| EC2 instances not joining | IAM role missing policy | Check node group IAM role has `AmazonEKSWorkerNodePolicy` |
| EC2 instances not launching | ASG capacity issue / spot unavailable | AWS Console → ASG → Activity tab |
| Subnet IP exhaustion | Not enough free IPs in subnet | VPC Console → check available IPs in subnet CIDR |
| AMI incompatibility | Node AMI version mismatch | Check `ami_release_version` matches cluster version |
| Security group blocking | Nodes can't reach API server | Check SG allows 443 from node SG to cluster SG |

```bash
# Key troubleshooting commands
kubectl get nodes                          # check node status
kubectl describe node <node-name>          # events and conditions
kubectl get events -A --sort-by='.lastTimestamp'  # cluster-wide events

# Check EC2 instance system log (console output)
aws ec2 get-console-output --instance-id i-xxxxxxxx

# Check node bootstrap log via SSM
aws ssm start-session --target i-xxxxxxxx
sudo cat /var/log/cloud-init-output.log
```

---

## 24. Autoscaling & Traffic Management

**Q: You're not using an Autoscaling Group — how does the cluster handle traffic spikes?**

- We **do** use autoscaling — via **Karpenter** (not the traditional ASG-based Cluster Autoscaler)
- Karpenter **directly calls the EC2 API** to provision nodes — faster than ASG (under 60 seconds vs 3-5 minutes)
- HPA scales pods horizontally when CPU/memory exceeds threshold → Pending pods signal Karpenter → Karpenter provisions exactly the right node size

```
Traffic spike hits ALB
  → ALB routes to running pods
  → CPU rises above HPA threshold (e.g., 70%)
  → HPA adds pod replicas
  → New pods go Pending (no capacity)
  → Karpenter provisions new EC2 in < 60 seconds
  → Pods schedule and serve traffic
  → Traffic drops → HPA scales pods down
  → Karpenter consolidates underutilized nodes (terminates them)
```

**Q: How do you know in advance how much traffic will hit your application?**

| Method | How it helps |
|---|---|
| **Historical metrics** | CloudWatch / Grafana — analyze past traffic patterns (peak hours, day-of-week trends) |
| **APM traces (X-Ray)** | Request rate per service — understand baseline and peak RPS |
| **Load testing** | k6, Locust, JMeter — simulate expected peak before launch or major event |
| **Business calendar** | Product team informs of planned campaigns, launches, sales events |
| **Anomaly detection** | CloudWatch Anomaly Detection / Datadog forecasting — ML-based traffic prediction |
| **HPA + buffer** | Set HPA thresholds at 70% CPU (not 90%) — 30% buffer absorbs sudden spikes before scaling kicks in |
| **Karpenter limits** | Set `NodePool.limits.cpu` high enough to handle worst-case; budget alerts if limits approached |

**Interview answer framework:**
> "We combine historical CloudWatch metrics with X-Ray request rate data to understand our baseline and peak traffic patterns. Before major business events (product launches, promotions), the product team gives advance notice and we run load tests with k6 to validate the cluster can handle projected load. For unexpected spikes, HPA + Karpenter handles it automatically — HPA kicks in at 70% CPU and Karpenter provisions new nodes within 60 seconds. We also set CloudWatch alarms to alert us if request error rates or latency exceeds SLO thresholds so we can respond proactively."
