# Interview Questions Asked

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

## 17. Behavioral — Automation

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
