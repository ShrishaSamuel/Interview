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
