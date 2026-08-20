# NatWest SRE — Complete Interview Q&A Bank
**Role:** Site Reliability Engineer | Associate Level | India  
**Goal:** Cover every possible question across all interview rounds — Technical, Behavioural, and Situational

---

## TABLE OF CONTENTS
1. [Self Introduction & Background](#1-self-introduction--background)
2. [SRE Core Concepts](#2-sre-core-concepts)
3. [Java & Microservices](#3-java--microservices)
4. [Kubernetes — Deep Dive](#4-kubernetes--deep-dive)
5. [Monitoring & Observability](#5-monitoring--observability)
6. [CI/CD & Release Engineering](#6-cicd--release-engineering)
7. [Incident Management](#7-incident-management)
8. [Change Management & Risk](#8-change-management--risk)
9. [Capacity Planning & Performance](#9-capacity-planning--performance)
10. [Terraform & Infrastructure as Code](#10-terraform--infrastructure-as-code)
11. [Docker & Containers](#11-docker--containers)
12. [Security](#12-security)
13. [Networking](#13-networking)
14. [Linux & Shell Scripting](#14-linux--shell-scripting)
15. [AWS Services](#15-aws-services)
16. [Behavioural / HR Questions](#16-behavioural--hr-questions)
17. [Scenario-Based / Problem-Solving](#17-scenario-based--problem-solving)
18. [NatWest / Financial Services Specific](#18-natwest--financial-services-specific)
19. [Questions to Ask the Interviewer](#19-questions-to-ask-the-interviewer)

---

## 1. Self Introduction & Background

### Q: Tell me about yourself.
> "Hi, I'm Shrisha. I have around 4 years of experience in DevOps and cloud infrastructure, with a strong focus on reliability engineering on AWS.
>
> I started at Infosys as an AWS DevOps Engineer, managing production infrastructure using Terraform — 3-tier VPCs, EC2 Auto Scaling, load balancers, CloudWatch monitoring, and CI/CD pipelines using AWS CodePipeline.
>
> At Capgemini, I moved into a platform and reliability-focused role. I manage the full lifecycle of a cloud-native microservices platform on AWS EKS — provisioning clusters with Terraform, building GitOps pipelines with GitHub Actions and ArgoCD, implementing full observability with OpenTelemetry and AWS X-Ray, and handling capacity planning with Karpenter and HPA.
>
> I approach my work through an SRE lens — defining service level objectives, automating toil, designing for failure, and using data to drive reliability improvements. I'm excited about this NatWest SRE role because it directly aligns with what I do daily."

---

### Q: Why are you moving from a DevOps role into SRE?
> "I see SRE as the natural evolution of what I already do. The distinction for me is intentionality — SRE formalises the reliability work with error budgets, SLOs, and blameless post-mortems, rather than doing it reactively. At Capgemini I've been applying SRE principles without the title — I set up alerting on SLI thresholds, ran blameless post-mortems after incidents, and eliminated toil through GitOps and automation. I want to work in an environment where this approach is the culture, and NatWest's engineering brand reflects exactly that."

---

### Q: Why NatWest?
> "NatWest is known for running complex, high-availability systems where reliability isn't optional — in financial services, downtime has direct customer and regulatory impact. That's the environment where SRE practices matter most. I also appreciate that NatWest is investing heavily in cloud-native engineering, and working at this scale will challenge me to go deeper on reliability systems design, capacity planning, and incident response than I can in a smaller environment."

---

### Q: Walk me through your current project architecture.
> "In my current role at Capgemini, I work on a Retail Store e-commerce platform running on AWS EKS.
>
> The application has 5 microservices: UI in Java Spring Boot, Catalog in Go backed by RDS MySQL, Cart in Spring Boot using DynamoDB, Checkout in Node.js using ElastiCache Redis, and Orders in Spring Boot backed by RDS PostgreSQL and SQS for async messaging.
>
> All infrastructure is provisioned with Terraform — VPC with public/private subnets across 3 AZs, EKS cluster with managed node groups, and all data services.
>
> For CI/CD: GitHub Actions builds and pushes Docker images to ECR, updates Helm chart values, and ArgoCD auto-syncs to EKS — fully GitOps, zero manual deployments.
>
> For autoscaling: Karpenter handles node provisioning including Spot instances with interruption handling; HPA handles pod scaling on CPU and memory.
>
> For observability: ADOT with auto-instrumentation exports traces to X-Ray, logs to CloudWatch, and metrics to Amazon Managed Prometheus, visualised in Grafana.
>
> Traffic flows: Internet → Route53 → ALB (HTTPS via ACM) → EKS pods. External DNS manages Route53 records automatically."

---

## 2. SRE Core Concepts

### Q: What is Site Reliability Engineering? How is it different from DevOps?
> "SRE is a discipline that applies software engineering principles to operations problems — specifically to build and run reliable, scalable systems. Google coined it, and the core idea is that 'reliability is a feature.'
>
> DevOps is a cultural and process philosophy about breaking silos between dev and ops. SRE is more prescriptive — it gives you concrete tools: SLOs, error budgets, toil measurement, blameless post-mortems.
>
> The key difference: DevOps asks 'how do we collaborate better?' SRE asks 'how reliable does this system need to be, and are we spending the right energy to get there?'
>
> In practice, SRE is an opinionated implementation of DevOps."

---

### Q: What are SLI, SLO, SLA, and Error Budget? Explain with an example.
**SLI (Service Level Indicator):** A measurable metric of service behaviour.  
Examples: request success rate, p99 latency, uptime percentage.

**SLO (Service Level Objective):** A target threshold for an SLI.  
Example: "99.9% of HTTP requests return 2xx within 300ms."

**SLA (Service Level Agreement):** A business/legal commitment, usually weaker than the SLO.  
Example: "We guarantee 99.5% uptime — if breached, we offer service credits."

**Error Budget:** `Error Budget = 1 - SLO`. At 99.9% SLO, error budget = 0.1% = 43.8 min/month of allowed failure.

> "In my EKS project, we tracked request success rate and p99 latency per service in Grafana as SLIs. Our internal SLO for the Orders service was 99.9% success rate. When a deployment caused error rate to spike for 8 minutes, we consumed part of our monthly error budget. That triggered a rollback via ArgoCD and a post-mortem to prevent recurrence. Error budgets create a shared language between product velocity and reliability — when the budget is burned, we slow releases until reliability improves."

---

### Q: What happens when the error budget is exhausted?
> "When the error budget is exhausted before month-end, we freeze feature releases and redirect the team's capacity to reliability work — fixing the root cause, improving alerting, adding circuit breakers, or strengthening the deployment pipeline. It's not a punishment; it's a data-driven signal that the system needs reliability investment before taking on more change risk."

---

### Q: What is toil? Give examples and how you reduced it.
**Toil** = manual, repetitive, tactical work that doesn't produce lasting value and scales with service size.

**Characteristics of toil:** Manual, repetitive, automatable, tactical, grows with traffic.

**My examples of toil eliminated:**
| Toil | Automation |
|---|---|
| Manual `kubectl apply` for each deployment | ArgoCD GitOps — deploy on Git merge |
| Manual secret rotation | Secrets Manager auto-rotation + CSI Driver |
| Manual node group scaling | Karpenter — zero human intervention |
| Health-check traces clogging X-Ray | ADOT sampling filter — reduced 85% |
| Manual Terraform runs | CodePipeline / GitHub Actions `terraform apply` |

---

### Q: What is the Google SRE 50% rule?
> "SREs should spend at most 50% of their time on operational/toil work. The other 50% must be on engineering work that reduces future toil or improves reliability. If operational work exceeds 50%, it's a signal that automation work is overdue. This ratio is tracked and enforced as a team health metric."

---

### Q: What is MTTD, MTTR, MTBF? Why do they matter?
| Metric | Full Form | Meaning |
|---|---|---|
| MTTD | Mean Time To Detect | Average time from failure start to alert/detection |
| MTTR | Mean Time To Recover | Average time from detection to full service restoration |
| MTBF | Mean Time Between Failures | Average time between two incidents |

> "These are reliability KPIs. In my project, MTTD was reduced from ~15 minutes to under 2 minutes after deploying ADOT with CloudWatch Alarms. MTTR for deployment-related issues is typically under 5 minutes because ArgoCD rollback is a single Git revert. Shorter MTTD + MTTR = higher effective availability."

---

### Q: What is an SLO burn rate? What is a fast-burn / slow-burn alert?
> "Burn rate measures how fast you're consuming your error budget relative to the budget period.
>
> A burn rate of 1 = consuming exactly as expected (will exactly exhaust budget at month-end).  
> A burn rate of 14.4 = consuming 14.4x faster than expected — at this rate, 100% of the monthly budget burns in 2 hours.
>
> Fast-burn alert: high burn rate over a short window (e.g., 1hr) — indicates a severe active incident. Page immediately.  
> Slow-burn alert: moderate burn rate over a long window (e.g., 6hr) — a slow leak that will eventually exhaust budget. Investigate during business hours.
>
> Multi-window alerting (e.g., 1hr + 6hr) reduces both false positives and missed incidents."

---

### Q: What is a blameless post-mortem?
> "A blameless post-mortem is a structured review after an incident that focuses on system and process failures, not individual blame. The assumption is that people operate in good faith — if they could have done better, the system should have made the right action easier or more obvious.
>
> A good post-mortem covers: timeline of events, root cause (5 Whys), impact (duration, users affected, SLO budget burned), contributing factors, action items with owners and deadlines.
>
> Blamelessness is critical because if engineers fear punishment, they hide information — and you can't fix what you don't know about."

---

### Q: What is chaos engineering? Have you used it?
> "Chaos engineering is the practice of deliberately injecting failures into a system in a controlled way to identify weaknesses before they cause real incidents. The principle: if failure is inevitable, test your resilience proactively.
>
> Classic tools: Netflix Chaos Monkey (random EC2 termination), AWS Fault Injection Simulator (FIS), Chaos Toolkit.
>
> I haven't used formal chaos engineering tools, but I've applied the mindset: Karpenter Spot interruption handling was validated by manually draining nodes to confirm PodDisruptionBudgets held and pods rescheduled within SLO. That's the essence — verify your assumptions about resilience under controlled failure."

---

### Q: What is the difference between high availability and fault tolerance?
| Concept | Meaning | Example |
|---|---|---|
| High Availability (HA) | System minimises downtime, recovers quickly from failure | EKS multi-AZ deployment — pod rescheduled on another AZ node in seconds |
| Fault Tolerance | System continues operating WITHOUT interruption during component failure | Active-active multi-region — no failover needed, traffic auto-routed |

> "HA tolerates some brief downtime with fast recovery. Fault tolerance means zero observable downtime. HA is easier and cheaper. Fault tolerance requires full redundancy at every layer."

---

### Q: What is the CAP theorem? How does it apply to distributed systems?
**CAP Theorem:** A distributed system can guarantee at most 2 of 3:
- **C**onsistency — every read returns the most recent write
- **A**vailability — every request gets a response (no timeout)
- **P**artition tolerance — system works despite network partitions

> "In practice, partition tolerance is non-optional in distributed systems — networks do fail. So the real trade-off is CP vs AP.
>
> In my project: RDS PostgreSQL (Orders) is CP — during a partition, it may be temporarily unavailable but never returns stale data. DynamoDB (Cart) is AP with eventual consistency — it stays available during partition but may return stale cart data. I chose DynamoDB for Cart because a slightly stale cart is acceptable; for Orders (financial transaction record), consistency is critical."

---

### Q: Explain the difference between latency, throughput, and bandwidth.
| Term | Definition | Example |
|---|---|---|
| Latency | Time to complete one request (end-to-end) | API responds in 120ms |
| Throughput | Requests processed per unit time | 5,000 RPS |
| Bandwidth | Maximum data transfer rate of a link | 1 Gbps network link |

> "Latency and throughput are often inversely related under load — as throughput approaches capacity, latency increases non-linearly due to queuing. This is Little's Law: L = λW (avg queue depth = arrival rate × avg wait time). When I saw p99 latency spike on the Orders service, the HikariCP connection pool was the bottleneck — requests queued waiting for a connection, driving latency up even though CPU was fine."

---

## 3. Java & Microservices

### Q: What is a microservice? What are its advantages and trade-offs?
**Advantages:**
- Independent deployment — each service deploys without affecting others
- Technology flexibility — each service picks its best language/DB
- Fault isolation — one service crashing doesn't bring down others
- Independent scaling — scale the bottleneck service only

**Trade-offs (SRE perspective):**
- Distributed tracing required — a single request spans multiple services
- Network latency between services — not present in monolith
- More complex incident triage — which of 5 services is the source?
- More operational surface area — 5 deploys instead of 1

> "In my project, we mitigated the trade-offs with ADOT + X-Ray for distributed tracing, service mesh concepts via readiness/liveness probes, and structured logging with correlation IDs injected at the ALB level."

---

### Q: What is a Spring Boot actuator and why is it important from an SRE perspective?
> "Spring Boot Actuator is a production-ready feature module that exposes HTTP endpoints for health checks, metrics, and runtime info.
>
> Key endpoints from an SRE perspective:
> - `/actuator/health` — liveness and readiness state (maps to Kubernetes probes)
> - `/actuator/metrics` — JVM heap, GC, HikariCP pool size — scraped by Prometheus
> - `/actuator/info` — app version, git commit hash — invaluable during incident triage
> - `/actuator/prometheus` — Prometheus-formatted metrics endpoint
>
> We configure Kubernetes readiness probe to hit `/actuator/health/readiness` — the pod only receives traffic after Spring context is fully initialized and DB connections are established."

---

### Q: What are the most common causes of Spring Boot microservice latency spikes? How do you diagnose each?

| Cause | Diagnosis | Fix |
|---|---|---|
| DB connection pool exhaustion | HikariCP `hikaricp_connections_active` metric at 100% | Increase pool size or scale pods |
| GC pause (Stop-the-World) | JVM GC pause duration metric, `jvm_gc_pause_seconds` | Tune heap size, switch to G1GC/ZGC |
| Slow downstream dependency | X-Ray service map — identify which span is long | Circuit breaker, timeout, async |
| CPU throttling | `container_cpu_cfs_throttled_periods_total` in Prometheus | Increase CPU limit |
| Thread pool saturation | `executor_queue_remaining_tasks` metric | Increase thread pool, async processing |

---

### Q: What is HikariCP? What metrics do you monitor for it?
> "HikariCP is the default JDBC connection pool in Spring Boot. It manages a pool of database connections that are reused by application threads.
>
> Key metrics:
> - `hikaricp_connections_active` — currently in-use connections
> - `hikaricp_connections_idle` — available connections waiting
> - `hikaricp_connections_pending` — threads waiting for a connection (critical — if > 0, you have a bottleneck)
> - `hikaricp_connections_timeout_total` — connections that timed out — indicates pool undersizing
>
> In production, I alert when `connections_pending > 0` for more than 30 seconds — that's when latency starts climbing."

---

### Q: What is a circuit breaker pattern? Why is it critical in microservices?
> "A circuit breaker wraps calls to external dependencies (other services, databases) and stops calling them when failure rate exceeds a threshold — like an electrical circuit breaker.
>
> States: Closed (normal), Open (failing, calls rejected fast), Half-Open (test if dependency recovered).
>
> Why critical: without circuit breakers, one slow downstream service cascades — the calling service's thread pool fills with waiting threads, making the calling service itself slow. This cascade can take down the entire system.
>
> In Spring Boot: Resilience4j is the standard library. I would configure a circuit breaker on every external service call — other microservices, RDS, Redis — with appropriate thresholds."

---

### Q: What is the difference between synchronous and asynchronous communication in microservices?
| Pattern | Technology | Use case | Trade-off |
|---|---|---|---|
| Synchronous | HTTP/REST, gRPC | Immediate response needed (e.g., Catalog lookup) | Tight coupling, latency chains |
| Asynchronous | SQS, Kafka, SNS | Decoupled processing (e.g., order creation → fulfillment) | Eventual consistency, harder debugging |

> "In my project, Checkout → Orders is async via SQS. The Checkout service publishes an order message and returns 200 immediately — it doesn't wait for Orders to process. This means if the Orders service is slow or down, Checkout is unaffected. The queue absorbs the burst. We monitor SQS `ApproximateAgeOfOldestMessage` as an SLI — if it grows, Orders is falling behind."

---

### Q: What is a readiness probe vs liveness probe in the context of a Spring Boot app?

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 8080
  initialDelaySeconds: 60   # Spring context needs time to start
  periodSeconds: 10
  failureThreshold: 3        # 3 failures → restart container

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 5
  failureThreshold: 3        # 3 failures → remove from Service endpoints
```

> "Liveness handles 'is the app stuck/deadlocked?' — restart if yes. Readiness handles 'is the app ready for traffic?' — remove from rotation if not, but don't restart. For Spring Boot with DB dependencies, readiness includes DB connectivity checks so we never route traffic to a pod that lost its DB connection."

---

### Q: What is the 12-Factor App methodology and how does SRE care about it?
| Factor | Relevance to SRE |
|---|---|
| Config in environment | No hardcoded values → secrets via env vars (Secrets Manager) |
| Stateless processes | Horizontal scaling is trivial → HPA works cleanly |
| Disposability (fast start/stop) | Pods start in seconds → rolling updates work |
| Logs as event streams | Structured JSON logs → CloudWatch Insights queries |
| Dev/prod parity | Staging mirrors production → fewer surprises in prod |

---

### Q: What is a service mesh? When would you use one?
> "A service mesh is an infrastructure layer for inter-service communication — it handles mTLS, traffic management, circuit breaking, retries, and observability at the network level, without changing application code.
>
> Popular options: Istio, Linkerd, AWS App Mesh.
>
> When to use: when you have many services, need mTLS between every pod pair, want fine-grained traffic control (A/B, canary by header), or need protocol-level observability.
>
> I haven't used a service mesh in production, but I understand the trade-off: it adds complexity (sidecar proxies, control plane) that's justified at scale. At my current project scale, we achieved service-to-service security with Network Policies and ADOT for observability."

---

## 4. Kubernetes — Deep Dive

### Q: A pod is in CrashLoopBackOff. Walk me through your diagnostic process.
```bash
# Step 1: check pod status and restart count
kubectl get pods -n <namespace>

# Step 2: describe pod — events section shows why it's crashing
kubectl describe pod <pod-name> -n <namespace>

# Step 3: check current logs
kubectl logs <pod-name> -n <namespace>

# Step 4: check previous container's logs (last crash)
kubectl logs <pod-name> -n <namespace> --previous

# Step 5: check resource metrics — OOMKilled?
kubectl top pods -n <namespace>
```

**Common causes and fixes:**
| Cause | Indicator | Fix |
|---|---|---|
| OOMKilled | `kubectl describe` shows `OOMKilled` reason | Increase `resources.limits.memory` |
| Bad config / missing env var | App logs show startup exception | Fix ConfigMap or Secret reference |
| Missing dependency | App can't reach DB / other service | Check network policy, service DNS |
| Readiness probe too aggressive | Probe fails before app starts | Increase `initialDelaySeconds` |
| Image pull failure | `ImagePullBackOff` status | Check ECR permissions, image tag |

---

### Q: A pod is in Pending state. What do you check?
```bash
kubectl describe pod <pod-name>  # Look at Events section
```

**Common causes:**
| Cause | Events Message | Fix |
|---|---|---|
| Insufficient CPU/Memory | `0/3 nodes available: Insufficient cpu` | Scale nodes or reduce requests |
| No nodes match nodeSelector/affinity | `0/3 nodes matched node affinity` | Fix affinity rules |
| PVC not bound | `persistentvolumeclaim not found` | Check StorageClass and PVC |
| Taint/toleration mismatch | `1 node had taints` | Add toleration to pod spec |

> "If using Karpenter, a pending pod should trigger node provisioning within seconds. If the pod remains pending, I check Karpenter controller logs — it may be that no NodePool can satisfy the pod's resource or affinity requirements."

---

### Q: What is the difference between Deployment rolling update, Recreate, and Blue-Green in Kubernetes?

| Strategy | Behaviour | Downtime | Use Case |
|---|---|---|---|
| `RollingUpdate` | Replaces pods incrementally | Zero (if configured correctly) | Standard stateless services |
| `Recreate` | Kills all old pods, then creates new | Brief downtime | DB schema migrations requiring version lock |
| Blue-Green (manual) | Two full deployments, switch Service selector | Zero | High-risk releases needing instant rollback |

```yaml
# Rolling update — zero downtime config
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0   # never go below desired count
    maxSurge: 1         # allow 1 extra pod during update
```

---

### Q: What is a PodDisruptionBudget and why is it important?
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: orders-pdb
spec:
  minAvailable: 1        # always keep at least 1 pod running
  selector:
    matchLabels:
      app: orders
```

> "PDBs prevent voluntary disruptions — like node drains during upgrades — from taking all pods offline simultaneously. Without a PDB, draining a node could evict all pods of a service if they all happen to land on that node. With `minAvailable: 1`, Kubernetes refuses to evict the last pod, ensuring at least one replica always serves traffic. Critical for zero-downtime node upgrades and Spot interruption handling with Karpenter."

---

### Q: How does Karpenter handle Spot interruptions?
> "When AWS decides to reclaim a Spot instance, it sends a 2-minute interruption notice via EventBridge. Karpenter listens to these events via an SQS queue integrated with EventBridge.
>
> When a Spot interruption is detected:
> 1. Karpenter cordons the node (marks it unschedulable)
> 2. Karpenter begins provisioning a replacement node (On-Demand or new Spot)
> 3. Karpenter drains the node — evicting pods gracefully, respecting PodDisruptionBudgets
> 4. Pods are rescheduled to the new node
>
> With PDBs and multi-AZ pod anti-affinity, pods always have a landing zone on another node. The replacement node is usually ready before the 2-minute window expires."

---

### Q: What is the difference between HPA and VPA?
| Feature | HPA (Horizontal Pod Autoscaler) | VPA (Vertical Pod Autoscaler) |
|---|---|---|
| Scaling axis | Adds/removes pods (horizontal) | Increases/decreases pod CPU/memory (vertical) |
| Metrics | CPU, memory, custom metrics | Actual historical consumption |
| Disruption | Low — new pods added | High — pods restarted to apply new resources |
| Use case | Stateless, scalable services | Right-sizing; stateful services where adding pods is complex |

> "I use HPA for all stateless microservices — scales smoothly under load. I use VPA in recommendation mode only to right-size initial `resources.requests` — I don't enable VPA auto-apply in production because it evicts and restarts pods, which can cause brief disruption."

---

### Q: What is a Kubernetes NetworkPolicy? Why should you use it?
> "By default, all pods in a Kubernetes cluster can talk to any other pod — zero network isolation. NetworkPolicy adds firewall rules at the pod level using label selectors.
>
> Why use it: principle of least privilege. If the Cart service is compromised, it shouldn't be able to reach the Orders database or the Checkout service.
>
> Example — allow only Catalog pods to talk to RDS MySQL:"

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-catalog-to-rds
spec:
  podSelector:
    matchLabels:
      app: catalog
  policyTypes:
    - Egress
  egress:
    - ports:
        - port: 3306
          protocol: TCP
```

---

### Q: What happens to a pod when the node it runs on fails?
> "If a node fails (hardware failure, network partition, or Spot termination):
> 1. The node's heartbeat to the API server stops
> 2. After the `node-monitor-grace-period` (default 40 seconds), the node is marked `NotReady`
> 3. After `pod-eviction-timeout` (default 5 minutes), pods are evicted from the node
> 4. Deployment controller recreates the pods on healthy nodes
>
> With Karpenter, step 4 also triggers new node provisioning if no capacity exists. We reduced effective recovery time by combining PodDisruptionBudgets + pod anti-affinity (guarantees pods are spread across AZs, so a full AZ loss doesn't kill all replicas)."

---

### Q: How does Kubernetes DNS work?
> "Kubernetes runs CoreDNS as a cluster-internal DNS server. Every service gets a DNS name:
> `<service-name>.<namespace>.svc.cluster.local`
>
> Pods use this DNS to discover and call other services. The Kubelet configures each pod's `/etc/resolv.conf` to point to CoreDNS.
>
> In my project: the Cart service calls the Orders service as `http://orders.retail.svc.cluster.local:8080/api/orders`. Kubernetes handles the resolution to the Orders Service ClusterIP, which load-balances across all Orders pods."

---

### Q: What is a Helm chart? How do you structure one?
```
my-chart/
├── Chart.yaml          # chart name, version, app version
├── values.yaml         # default values
├── values-dev.yaml     # dev environment overrides
├── values-prod.yaml    # prod environment overrides
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    ├── hpa.yaml
    ├── configmap.yaml
    └── _helpers.tpl    # named template helpers
```

> "In my project, each of the 5 microservices has its own Helm chart. GitHub Actions CI builds the image, updates `image.tag` in the chart's `values.yaml`, commits to the GitOps repo, and ArgoCD picks up the change and deploys."

---

### Q: ArgoCD self-heal — what is it and when does it trigger?
> "ArgoCD continuously compares the live cluster state against the desired state in the Git repo. If any drift is detected — someone manually ran `kubectl edit`, a pod spec was changed directly, or a resource was deleted — ArgoCD automatically reverts the cluster to match Git.
>
> Self-heal triggers immediately when drift is detected. This is configured with `selfHeal: true` in the ArgoCD Application spec.
>
> Why it matters: it enforces Git as the single source of truth. No configuration drift. No undocumented manual changes. Every state is auditable via Git history."

---

### Q: How do you do a zero-downtime EKS cluster version upgrade?
> "Upgrade strategy:
> 1. Upgrade the EKS control plane first — AWS handles this with no workload disruption
> 2. Upgrade EKS add-ons (VPC CNI, CoreDNS, kube-proxy) — check compatibility matrix
> 3. Upgrade managed node groups — using rolling replacement (AWS drains one node at a time)
> 4. PodDisruptionBudgets ensure minimum replicas stay up during node drain
> 5. Validate all workloads healthy after each step before proceeding
>
> We test on non-production 48 hours before production. EKS only supports N-2 Kubernetes versions, so upgrades are time-pressured."

---

## 5. Monitoring & Observability

### Q: Explain the three pillars of observability with your specific tools.
| Pillar | Tool | What it captures |
|---|---|---|
| Logs | ADOT → CloudWatch Logs | Structured JSON app logs, K8s events, container stdout |
| Metrics | ADOT → Amazon Managed Prometheus → Grafana | JVM, HTTP, HikariCP, Karpenter, node metrics |
| Traces | ADOT → AWS X-Ray | End-to-end request trace across all 5 microservices |

> "Logs tell you *what* happened — the exception, the SQL query, the user ID. Metrics tell you *how much* — RPS, error rate, latency percentiles, heap usage. Traces tell you *where* — which service, which DB call, which line of code introduced the 800ms latency. All three together give you the full picture during an incident."

---

### Q: What is OpenTelemetry / ADOT?
> "OpenTelemetry (OTel) is a vendor-neutral observability framework — a standard API and SDK for collecting logs, metrics, and traces from applications. AWS Distro for OpenTelemetry (ADOT) is AWS's supported distribution.
>
> In my project, ADOT runs as a DaemonSet on every EKS node and as a sidecar for some services. It uses auto-instrumentation — a Java agent injected at runtime via an annotation that intercepts HTTP calls, DB queries, and messaging without changing application code. All telemetry flows through the ADOT collector, which routes traces to X-Ray, logs to CloudWatch, and metrics to AMP."

---

### Q: What Grafana dashboards would you build for a Java microservice in production?

**Dashboard panels I configure:**
1. Request rate (RPS) — split by service and HTTP status code
2. Error rate (%) — 4xx and 5xx separately
3. p50 / p95 / p99 latency — by endpoint and service
4. JVM heap utilization — used vs committed vs max
5. GC pause duration — frequency and duration of GC events
6. HikariCP connections — active, idle, pending, timeout count
7. Pod count vs HPA target — are we scaling as expected?
8. CPU and memory utilization vs limits
9. Karpenter node count — by instance type and lifecycle (On-Demand/Spot)
10. SLO burn rate — error budget consumption over time

---

### Q: What is a CloudWatch Alarm? How do you configure one properly?
> "A CloudWatch Alarm monitors a metric and triggers an action (SNS notification, Auto Scaling, Systems Manager) when it crosses a threshold.
>
> Key parameters to configure correctly:
> - **Evaluation period:** How many periods must breach before alarm triggers (avoid single-point spikes)
> - **Datapoints to alarm:** e.g., 3 out of 5 periods — reduces noise
> - **Missing data treatment:** `notBreaching` for metrics that stop when pods are healthy; `breaching` for heartbeat-style metrics where absence = problem
> - **Alarm resolution:** Does it auto-resolve when metric recovers?
>
> Example: CPU > 80% for 3 out of 5 consecutive 1-minute periods → SNS → PagerDuty page."

---

### Q: What is distributed tracing? How does X-Ray trace a request across services?
> "In a microservices architecture, one user request touches multiple services. Distributed tracing assigns a unique `trace-id` to the initial request. Every service that handles that request creates a `span` — a time-boxed unit of work — and propagates the `trace-id` in HTTP headers.
>
> X-Ray collects these spans and assembles them into a service map and waterfall trace view.
>
> In my project: a user hits the UI → UI calls Catalog → Catalog queries RDS MySQL. X-Ray shows the full waterfall: 50ms UI processing, 20ms HTTP to Catalog, 120ms RDS query. I can immediately see the RDS query is the bottleneck without guessing."

---

### Q: How do you distinguish a performance degradation caused by the application vs the infrastructure?
> "I correlate metrics across three layers:
> 1. **Infrastructure:** Is CPU, memory, or disk I/O on the nodes saturated? Is EBS throughput throttled? Checked via Karpenter/node metrics in Grafana.
> 2. **Kubernetes:** Is the pod CPU-throttled (CFS throttling)? Is the pod restarting? Are HPA replicas maxed out? Checked via `container_cpu_cfs_throttled_periods_total`.
> 3. **Application:** Is JVM GC pausing? Is the HikariCP pool saturated? Is a specific endpoint slow while others are fast? Checked via X-Ray traces + JVM metrics.
>
> If infrastructure metrics are flat and application metrics are degraded — it's the app. If node CPU is saturated — it's infrastructure. If only one service is slow in X-Ray — it's that service or its dependency."

---

### Q: What is the difference between p50, p95, and p99 latency? Why not use average?
> "p50 = median latency — 50% of requests finish faster than this. p95 = 95% of requests finish faster. p99 = 99% of requests finish faster.
>
> Average hides outliers. If 99% of requests finish in 10ms and 1% finish in 10 seconds, the average might be 110ms — which sounds reasonable but hides the fact that 1% of users have a terrible experience.
>
> For SLOs we use p99 — it captures the experience of the worst-served users. For capacity planning we use p95. For baseline understanding we use p50.
>
> At NatWest, where customer experience in banking is critical, p99 is the metric I'd prioritise."

---

## 6. CI/CD & Release Engineering

### Q: Explain your complete CI/CD pipeline end-to-end.
```
Developer pushes code to GitHub
        ↓
GitHub Actions CI pipeline triggers:
  - Unit tests (Maven / Jest)
  - Docker multi-stage build
  - Image scan (Trivy)
  - Push to Amazon ECR (OIDC — no access keys)
  - Update image tag in Helm values.yaml (GitOps repo)
        ↓
ArgoCD detects change in GitOps repo (webhook or poll)
        ↓
ArgoCD syncs: applies Helm chart to EKS
  - Rolling update with readiness probe gating
  - Self-heal enabled
        ↓
Post-deploy: Grafana alert silence auto-lifted, smoke tests run
```

> "The key design decisions: OIDC instead of IAM access keys in GitHub Actions (no long-lived credentials); GitOps repo separate from app repo (separation of concerns); ArgoCD self-heal as automatic drift correction; and ECR image scanning before deploy to catch CVEs."

---

### Q: What is OIDC authentication in GitHub Actions? Why is it better than access keys?
> "OIDC (OpenID Connect) allows GitHub Actions to authenticate to AWS without storing any credentials in GitHub Secrets. GitHub generates a short-lived JWT token for each workflow run. AWS IAM is configured to trust GitHub's OIDC provider and exchange the JWT for temporary STS credentials.
>
> Why better than access keys:
> - No long-lived credentials to rotate, leak, or expire
> - Credentials are scoped to the specific repo and branch
> - AWS CloudTrail logs show exactly which GitHub workflow assumed which role
> - Compromise of a GitHub Secret can't expose permanent credentials"

---

### Q: What is the difference between GitOps and traditional push-based CI/CD?
| Feature | Traditional Push-Based | GitOps (Pull-Based) |
|---|---|---|
| Deploy trigger | CI pipeline pushes to cluster | ArgoCD pulls from Git |
| Cluster access | CI server has kubectl/Helm access | ArgoCD has cluster access; CI does not |
| Drift detection | None | Continuous — ArgoCD detects and corrects |
| Audit trail | CI logs | Git history — every change is a commit |
| Rollback | Re-run pipeline with old image | `git revert` — instant |

---

### Q: How do you implement a canary deployment in Kubernetes?
**Option 1 — Weighted Ingress (Nginx/ALB):**
```yaml
# Canary ingress with 10% traffic weight
annotations:
  nginx.ingress.kubernetes.io/canary: "true"
  nginx.ingress.kubernetes.io/canary-weight: "10"
```

**Option 2 — Argo Rollouts:**
```yaml
strategy:
  canary:
    steps:
    - setWeight: 5     # 5% traffic to new version
    - pause: {duration: 10m}
    - analysis:        # auto-promote if error rate < 1%
        templates:
        - templateName: success-rate
    - setWeight: 50
    - pause: {duration: 10m}
    - setWeight: 100
```

---

### Q: How do you handle database migrations in a CI/CD pipeline without downtime?
> "Database migrations in a continuous deployment environment require a backward-compatible approach:
>
> 1. **Expand-Contract pattern:** Never add a NOT NULL column without a default. Never rename a column directly. Add new columns as nullable → populate → add constraint.
> 2. **Flyway / Liquibase:** Schema migrations as versioned scripts in the codebase — run automatically on app startup.
> 3. **Blue-Green awareness:** During a rolling update, old and new app versions run simultaneously. The schema must be compatible with both versions — so additive-only changes during the rollout.
> 4. **Never drop columns in the same release:** Deploy without the column drop → old code gone → drop in next release.
>
> In my project, RDS PostgreSQL migrations run via Flyway on Spring Boot startup, with `spring.flyway.baseline-on-migrate=true` for safety."

---

### Q: What is feature flagging and why is it an SRE tool?
> "A feature flag is a configuration-driven switch that enables or disables a feature without deploying new code.
>
> SRE value: it separates deploy from release. You can deploy code to production with the flag OFF — no user impact — and turn it on for 1% of users first to validate. If something goes wrong, turn the flag off in seconds — much faster than a rollback deployment.
>
> It also supports: A/B testing, gradual rollout, kill switches for risky features.
>
> Tools: LaunchDarkly, AWS AppConfig, Unleash (open source)."

---

### Q: What is semantic versioning? How do you apply it to Docker images?
> "Semantic versioning: `MAJOR.MINOR.PATCH`
> - MAJOR: breaking API change
> - MINOR: backward-compatible new feature
> - PATCH: backward-compatible bug fix
>
> For Docker images, I avoid the `:latest` tag in production — it's mutable and makes rollback impossible.
>
> In my pipeline:
> - Feature branches: `my-app:pr-123-abc1234` (PR number + commit hash)
> - Production: `my-app:1.4.2` (semver) and `my-app:1.4.2-abc1234` (semver + commit)
>
> This ensures every image is traceable back to an exact Git commit and every deployment is reproducible."

---

## 7. Incident Management

### Q: Walk me through how you handle a production incident from alert to resolution.

**Phase 1 — Detect:**
CloudWatch Alarm fires (e.g., error rate > 1% for 3 minutes) → SNS → PagerDuty/Slack page.

**Phase 2 — Triage (first 5 minutes):**
```bash
kubectl get pods -n retail          # Any CrashLoopBackOff or OOMKilled?
kubectl top pods -n retail          # CPU/memory spike?
# Open Grafana — error rate dashboard
# Open X-Ray — service map — which service has red spans?
```

**Phase 3 — Communicate:**
Post incident start message in incident Slack channel: "Investigating elevated error rate on Orders service. Estimated impact: X% of order creation requests. Will update in 10 minutes."

**Phase 4 — Mitigate:**
```bash
# If recent deployment is the cause:
git revert <last-commit>  # Revert Helm values change
# ArgoCD auto-syncs and rolls back — ~30 seconds

# If traffic spike:
kubectl patch hpa orders-hpa -p '{"spec":{"maxReplicas":20}}'
```

**Phase 5 — Resolve:**
Confirm metrics returning to normal. Lift monitoring silence. Close incident.

**Phase 6 — Post-mortem (within 48 hours):**
Document: timeline, root cause, impact, SLO budget consumed, action items.

---

### Q: How do you communicate an incident to stakeholders without technical jargon?
> "I adapt the communication to the audience. For non-technical stakeholders:
>
> - **What:** 'Customers are experiencing errors when trying to place orders — approximately X% of orders are failing.'
> - **Since when:** 'The issue started at 14:32 IST.'
> - **Impact:** 'Estimated X customers affected in the last 20 minutes.'
> - **What we're doing:** 'Our team identified the cause as a software deployment issue and has initiated a rollback. We expect full restoration in approximately 10 minutes.'
> - **Next update:** 'We will update you at 15:10 IST or sooner if resolved.'
>
> I avoid terms like 'pod crash', 'HPA', 'EKS'. I speak in customer and business impact terms."

---

### Q: What is a runbook? How do you maintain one?
> "A runbook is a documented set of procedures for handling a specific operational scenario — like 'Orders service high latency' or 'Database failover'.
>
> A good runbook contains:
> 1. Alert that triggers it
> 2. Impact description
> 3. Triage steps (with exact commands to run)
> 4. Decision tree — if X, do Y; if Z, escalate
> 5. Escalation path — who to page if not resolved in N minutes
> 6. Last updated date and owner
>
> Maintenance: after every incident, update the runbook if the procedure differed from documentation. Runbooks should be stored in the Git repo, version-controlled, and linked directly from PagerDuty alert definitions."

---

### Q: What is the difference between MTTD and MTTR? How do you improve each?

**Improving MTTD (Mean Time To Detect):**
- Add more SLI-based alerts (not just CPU/memory — error rate, latency, queue depth)
- Reduce alert evaluation window — alert on 1-minute data, not 5-minute averages
- Use synthetic monitoring — proactively send test requests and alert if they fail
- Implement distributed tracing — detect slow spans before users notice

**Improving MTTR (Mean Time To Recover):**
- Automated rollback via ArgoCD revert
- Pre-written runbooks for common failures
- Chaos drills — practise recovery so it's muscle memory
- Blue-green deployment — rollback is a traffic switch, not a re-deploy
- PodDisruptionBudgets — reduce blast radius so partial recovery is automatic

---

### Q: What is the difference between an incident and a problem? (ITIL)
| Concept | ITIL Definition | Action |
|---|---|---|
| Incident | Unplanned interruption or quality degradation | Restore service ASAP |
| Problem | Root cause of one or more incidents | Find and permanently fix root cause |

> "During an incident, the goal is restoration, not diagnosis — restore service first, investigate later. The post-mortem converts the incident into a Problem record with a root cause and permanent fix. SRE's post-mortem culture maps closely to ITIL problem management."

---

## 8. Change Management & Risk

### Q: How do you ensure a safe deployment in a production environment?
> "Multiple layers:
> 1. **Testing gates in CI:** Unit tests, integration tests, image security scan — all must pass before any image is pushed.
> 2. **Staging validation:** Deploy to staging first, run smoke tests. Production deploy only after staging is green.
> 3. **GitOps PR review:** Any change to the production GitOps repo requires a peer-reviewed and approved PR.
> 4. **Rolling update strategy:** `maxUnavailable: 0` ensures old pods serve traffic until new pods pass readiness probes.
> 5. **Automated rollback trigger:** If error rate exceeds threshold within 5 minutes of a deploy, ArgoCD Rollout analysis triggers automatic rollback.
> 6. **Deployment freeze windows:** No production deployments during peak hours or known high-traffic events."

---

### Q: What is change advisory board (CAB) and how does GitOps relate to it?
> "A CAB is a governance body in ITIL that reviews and approves significant changes before they're applied to production — common in regulated industries like banking.
>
> GitOps maps elegantly to CAB: every change is a Git PR with description, justification, and peer review. The PR merge is the CAB approval. The Git history is the full audit trail.
>
> In financial services, GitOps can satisfy many CAB requirements automatically: who approved the change, when, what the change was, and what the rollback procedure is (git revert). This replaces manual change tickets with an automated, auditable workflow."

---

### Q: What is immutable infrastructure? Why does it improve reliability?
> "Immutable infrastructure means servers/containers are never modified in place — they are replaced entirely when a change is needed.
>
> In my project: Docker containers are immutable — I never exec into a running container and change config. Every change goes through a new Docker build → new image → new deployment.
>
> Benefits:
> - No configuration drift — every environment is built from the same source
> - Rollback is trivial — switch to the previous image tag
> - Debugging is reproducible — the exact running image can be rebuilt and tested
> - Security — no persistent access to running containers"

---

### Q: How do you manage Terraform state safely in a team environment?
> "Three rules:
> 1. **Remote state:** State stored in S3 with server-side encryption — never on a local machine.
> 2. **State locking:** DynamoDB lock table prevents concurrent `terraform apply` — if two engineers run simultaneously, one gets a lock error.
> 3. **Never edit state manually:** Use `terraform state rm`, `terraform state mv`, or `terraform import` — never open the `.tfstate` file in a text editor.
>
> Additional safety: Terraform pipelines require a `plan` step with output reviewed before `apply` can run. Production applies require a separate manual approval step in the pipeline."

---

## 9. Capacity Planning & Performance

### Q: How do you set Kubernetes resource requests and limits?
> "Process:
> 1. Start with VPA in recommendation mode — runs for 1 week, observes actual CPU/memory consumption.
> 2. Set `requests` = p50 consumption (scheduler uses this for node placement).
> 3. Set `limits` = p99 + 20% buffer (allows burst without OOMKill).
> 4. For Java: add JVM off-heap overhead to memory limits. The JVM heap is set via `-Xmx`, but total JVM memory = heap + metaspace + code cache + thread stacks + off-heap. A common mistake is setting `limits.memory` = heap size — which causes OOMKill.
> 5. After HPA is configured, monitor throttling metrics — if CPU is throttled, limits are too tight."

```yaml
resources:
  requests:
    cpu: "500m"       # 0.5 vCPU — scheduler placement
    memory: "512Mi"   # p50 consumption
  limits:
    cpu: "1000m"      # 1 vCPU — runtime cap
    memory: "1024Mi"  # p99 + 20% buffer (includes JVM overhead)
```

---

### Q: How do you approach capacity planning for a new service before launch?
> "Steps:
> 1. **Define peak load:** What's the expected RPS at launch? What's the 3x spike scenario?
> 2. **Load test:** Use k6 or JMeter to drive synthetic traffic at 1x, 2x, and 3x expected peak. Observe latency, error rate, CPU, memory, and DB connection pool.
> 3. **Find the bottleneck:** Where does p99 latency start climbing — is it the app, the DB, or the node?
> 4. **Set HPA thresholds:** Scale out before the bottleneck is reached — e.g., if CPU at 70% causes latency spikes, set HPA to scale at 60%.
> 5. **Pre-warm:** For launches with known traffic spikes, pre-scale pods and nodes manually before the event.
> 6. **Set Karpenter NodePool limits:** Define max nodes and instance types to cap cost while ensuring capacity."

---

### Q: What is the difference between vertical scaling and horizontal scaling? When do you use each?
| Aspect | Vertical (Scale Up) | Horizontal (Scale Out) |
|---|---|---|
| Meaning | Bigger instance / more CPU+RAM | More instances / pods |
| Limit | Physical hardware ceiling | Theoretically unlimited |
| Downtime | Often requires restart | Zero with rolling updates |
| Cost | Exponential at high scale | Linear |
| Use case | Databases (single-writer), stateful apps | Stateless APIs, microservices |

> "For my stateless Spring Boot microservices, horizontal scaling via HPA is always the answer — add more pods, distribute the load. For RDS PostgreSQL (Orders), vertical scaling is used — a larger instance class handles more concurrent queries. The trade-off: RDS failover during instance resize takes ~1 minute."

---

### Q: What is auto-scaling warm-up time and why does it matter?
> "Warm-up time is the delay between when an auto-scaling event triggers and when new capacity is ready to serve production traffic. Components:
> - Node provisioning: 60-90 seconds for a new EC2 node to join the cluster
> - Container startup: 30-60 seconds for Spring Boot to initialize (JVM + Spring context + DB connection pool)
> - Readiness probe: pods only receive traffic after probes pass
>
> Total: a demand spike can take 2-3 minutes before new pods serve traffic. Mitigation:
> 1. Keep a buffer of idle HPA replicas (`minReplicas` > 1) to absorb initial spikes
> 2. Scale aggressively on leading indicators (queue depth, p95 latency) not lagging indicators (CPU)
> 3. Use Karpenter with pre-provisioned nodes for critical services"

---

## 10. Terraform & Infrastructure as Code

### Q: What is infrastructure drift and how do you handle it?
> "Infrastructure drift occurs when the actual state of infrastructure diverges from the Terraform state — typically because someone made a manual change directly in the AWS console or CLI without updating Terraform.
>
> Detection: `terraform plan` shows unexpected changes — resources that Terraform didn't touch showing as modified.
>
> Handling options:
> 1. **Accept the change:** `terraform plan -refresh-only` then `terraform apply -refresh-only` — updates state to match reality, keeps the manual change.
> 2. **Revert to IaC:** Run `terraform apply` — Terraform overwrites the manual change.
> 3. **Import the resource:** If a resource was created manually, `terraform import` brings it under Terraform management.
>
> Prevention: restrict console/CLI access; require all changes via Terraform pipeline; use AWS Config Rules to detect manual changes."

---

### Q: What is `terraform taint` and when do you use it?
> "`terraform taint` (now `terraform apply -replace`) marks a resource for destruction and recreation on the next apply, even if Terraform doesn't detect a config change.
>
> Use when:
> - EC2 user data ran incorrectly and the instance needs to be reprovisioned
> - An EKS node is in an unhealthy state but Terraform shows it as `OK`
> - A certificate is corrupted and needs to be reissued
>
> Command: `terraform apply -replace='aws_instance.web_server'`"

---

### Q: What happens if the Terraform state file is deleted?
> "Terraform loses its memory of what it manages. Running `terraform apply` would try to create all resources again — causing duplicates or conflicts.
>
> Recovery options:
> 1. Restore from S3 version history (reason to always enable S3 versioning on state buckets)
> 2. If no backup: `terraform import` each resource one by one to rebuild state
>
> Prevention:
> - S3 versioning on state bucket — always
> - Restrict `s3:DeleteObject` permission on the state bucket — only allow Terraform to read/write, not delete
> - Enable MFA delete on the S3 bucket for critical state files"

---

### Q: Explain Terraform workspaces vs separate state files per environment.
| Approach | How it works | Trade-off |
|---|---|---|
| Workspaces | `terraform workspace new prod` — separate state per workspace, same code | Simple but can accidentally apply to wrong workspace |
| Separate state files | Different backend configs per environment directory | Clear separation, harder to accidentally cross-contaminate |
| Separate repos/directories | `environments/prod/` `environments/staging/` | Maximum isolation, recommended for production |

> "I prefer separate directories per environment with different backend configs — `prod/terraform.tfstate` and `staging/terraform.tfstate` in separate S3 keys. It's explicit and eliminates workspace selection errors that could apply prod changes to the wrong target."

---

### Q: What is the Terraform `lifecycle` block? When do you use it?
```hcl
resource "aws_rds_instance" "db" {
  lifecycle {
    prevent_destroy = true          # refuse to delete this resource
    create_before_destroy = true    # create new resource before destroying old
    ignore_changes = [tags]         # don't track tag changes in state
  }
}
```

> "`prevent_destroy = true` is critical for databases and state buckets — it prevents accidental `terraform destroy` from deleting persistent data. I apply it to every RDS instance, S3 bucket with critical data, and DynamoDB table in production."

---

## 11. Docker & Containers

### Q: What is a multi-stage Docker build? Why is it important?
```dockerfile
# Stage 1 — build
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline      # cache dependencies separately
COPY src ./src
RUN mvn package -DskipTests

# Stage 2 — runtime
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup  # non-root
COPY --from=builder /app/target/app.jar ./app.jar
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

> "Multi-stage builds reduce image size from ~800MB (JDK + Maven + source) to ~180MB (JRE + JAR only). Build tools never appear in the runtime image — smaller attack surface, fewer CVEs to patch, faster ECR pulls."

---

### Q: What is the difference between `CMD` and `ENTRYPOINT`?
| Feature | ENTRYPOINT | CMD |
|---|---|---|
| Purpose | Fixed executable | Default arguments |
| Override at `docker run` | Requires `--entrypoint` flag | Replaced by any arguments |
| Common pattern | `ENTRYPOINT ["java", "-jar", "app.jar"]` | `CMD ["--spring.profiles.active=prod"]` |

> "I use ENTRYPOINT for the fixed executable and CMD for overridable defaults. In Kubernetes, the pod spec's `command` overrides ENTRYPOINT and `args` overrides CMD."

---

### Q: How do you scan Docker images for vulnerabilities?
```bash
# Trivy — open source, integrates with GitHub Actions
trivy image --exit-code 1 --severity HIGH,CRITICAL my-app:latest

# In GitHub Actions pipeline:
- name: Scan image
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: ${{ env.ECR_REGISTRY }}/my-app:${{ env.IMAGE_TAG }}
    severity: HIGH,CRITICAL
    exit-code: '1'   # fail pipeline if HIGH/CRITICAL CVEs found
```

> "I run Trivy as a mandatory pipeline step before pushing to ECR. Any HIGH or CRITICAL CVE fails the pipeline — the image is never pushed. Results are also reported to the GitHub Security tab as SARIF."

---

## 12. Security

### Q: What is EKS Pod Identity and why is it more secure than IAM access keys?
> "EKS Pod Identity allows Kubernetes pods to assume IAM roles without any static credentials. A Pod Identity Agent DaemonSet runs on each node and intercepts AWS SDK credential requests from pods, exchanging a short-lived Kubernetes service account token for temporary AWS credentials via STS.
>
> Security advantages:
> - No long-lived credentials — tokens expire in 15 minutes
> - Credentials are scoped to the specific IAM role
> - No credentials stored in environment variables or Secrets — can't be leaked in logs
> - IAM access is per-service-account — the Cart service can't access Orders' DynamoDB table
>
> In my project: each of the 5 microservices has its own service account with its own IAM role, following least-privilege — Catalog can only read RDS MySQL, Orders can only write to RDS PostgreSQL and SQS."

---

### Q: How do you secure secrets in Kubernetes?
> "Native Kubernetes Secrets are base64-encoded in etcd — not encrypted by default. My approach:
>
> 1. Store secrets in AWS Secrets Manager — versioned, auditable, auto-rotation supported.
> 2. Use Secrets Store CSI Driver (ASCP) to mount secrets directly into pods as files or environment variables.
> 3. Pods access Secrets Manager via their IAM role (Pod Identity) — no Kubernetes Secret object created in etcd.
> 4. Enable AWS CloudTrail on Secrets Manager — every secret access is logged with caller identity.
> 5. Enable `aws_secretsmanager_secret_rotation` for DB passwords — automatic rotation without code changes."

---

### Q: What is RBAC in Kubernetes? How do you implement it?
> "RBAC (Role-Based Access Control) controls who can perform which actions on which Kubernetes resources.
>
> Components:
> - `Role` / `ClusterRole` — defines what actions are allowed on which resources
> - `RoleBinding` / `ClusterRoleBinding` — assigns a Role to a user or service account"

```yaml
# Restrict dev team to read-only access in the retail namespace
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: retail
  name: read-only
rules:
- apiGroups: [""]
  resources: ["pods", "services", "configmaps"]
  verbs: ["get", "list", "watch"]  # no create/delete/patch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: retail
  name: dev-read-only
subjects:
- kind: Group
  name: dev-team
roleRef:
  kind: Role
  name: read-only
```

---

### Q: What is the OWASP Top 10 and how does SRE interact with it?

| OWASP Risk | SRE Mitigation |
|---|---|
| Broken Access Control | RBAC, Pod Identity least-privilege, Network Policies |
| Cryptographic Failures | TLS everywhere (ALB → pods), Secrets Manager encryption, EBS encryption |
| Injection | Parameterised queries in app code; WAF rules at ALB |
| Security Misconfiguration | IaC enforces consistent config; Terraform `prevent_destroy`; image scanning |
| Vulnerable Components | Trivy image scanning in CI; automated dependency updates |
| Identification & Authentication | OIDC for GitHub Actions; no long-lived IAM keys |
| Security Logging & Monitoring | CloudTrail, CloudWatch Logs, Grafana alerts, Secrets Manager audit |

---

### Q: How do you implement a principle of least privilege in AWS for your EKS workloads?
> "Three layers:
> 1. **IAM roles per service:** Each microservice has its own IAM role via Pod Identity. Cart can only access DynamoDB, Orders can only access RDS PostgreSQL and SQS. No shared roles.
> 2. **Resource-level IAM policies:** Policies restrict to specific resource ARNs — not `*`. E.g., Cart's role can only `GetItem/PutItem/DeleteItem` on `arn:aws:dynamodb:region:account:table/cart-table`.
> 3. **Network Policies:** Pods can only communicate with the services they need — no cross-service communication outside defined routes."

---

## 13. Networking

### Q: What happens when a user makes an HTTPS request to your EKS application? Trace the full path.
```
1. User browser: DNS lookup for api.retailstore.com
2. Route53: returns ALB public IP (managed by External DNS)
3. TLS handshake: ALB presents ACM certificate, TLS terminates at ALB
4. ALB: HTTPS → HTTP forwarding; evaluates listener rules (path-based routing)
5. ALB Target Group: selects a pod IP (IP mode — direct to pod, no NodePort hop)
6. Kubernetes NetworkPolicy: verifies the traffic is allowed to reach the pod
7. Pod: receives plain HTTP request, processes it
8. Spring Boot: routes to controller, calls downstream services via Kubernetes DNS
9. Response: travels back through the same path
```

---

### Q: What is the difference between ClusterIP, NodePort, LoadBalancer, and Ingress in Kubernetes?
| Type | Scope | Use Case |
|---|---|---|
| ClusterIP | Internal only | Pod-to-pod communication within the cluster |
| NodePort | External via node IP:port | Development/testing only; not production |
| LoadBalancer | External via cloud LB | Exposes one service directly via an NLB/ALB |
| Ingress | External via HTTP/HTTPS rules | Multiple services behind one LB with path/host routing |

> "In my project: all services use ClusterIP for internal communication. One Ingress resource with ALB Ingress Controller exposes all services externally via path-based routing — no separate LoadBalancer per service."

---

### Q: What is a VPC and how is yours designed?
```
VPC: 10.0.0.0/16
├── Public Subnets (10.0.1.0/24, 10.0.2.0/24, 10.0.3.0/24) — AZ-a, AZ-b, AZ-c
│   └── ALB, NAT Gateways, Bastion (if any)
└── Private Subnets (10.0.11.0/24, 10.0.12.0/24, 10.0.13.0/24) — AZ-a, AZ-b, AZ-c
    └── EKS nodes, RDS, ElastiCache, DynamoDB VPC endpoints
```

> "EKS worker nodes are in private subnets — no direct internet access. Outbound traffic goes through NAT Gateways in public subnets. Inbound traffic comes only through the ALB in public subnets. RDS and ElastiCache are in private subnets with Security Groups restricting access to pod CIDR ranges only."

---

### Q: What is the difference between TCP and UDP? When does each matter in SRE?
| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (handshake) | Connectionless |
| Reliability | Guaranteed delivery, ordered | Best effort, no guarantee |
| Speed | Slower (retransmission overhead) | Faster |
| Use case | HTTP, databases, SSH | DNS, QUIC/HTTP3, metrics (StatsD) |

> "For SRE: TCP for everything where data loss is unacceptable (HTTP traffic, DB queries). UDP for high-volume, loss-tolerant telemetry (StatsD metrics, some APM agents). DNS uses UDP by default — understanding this matters when debugging DNS-related Kubernetes pod connectivity issues."

---

## 14. Linux & Shell Scripting

### Q: A Java service on a Linux server is slow. How do you diagnose it?
```bash
# CPU consumption
top -p $(pgrep java)       # is the JVM using all CPU?
pidstat -p $(pgrep java) 1 # per-second CPU breakdown

# Memory
ps aux | grep java         # resident set size (RSS)
jmap -heap <pid>           # JVM heap breakdown
jstat -gcutil <pid> 1000   # GC activity — is it spending all time in GC?

# Thread state
jstack <pid> | grep -A5 "BLOCKED"   # any blocked threads?
jstack <pid> | grep "TIMED_WAITING" # threads waiting on DB/IO?

# Network
ss -tnp | grep java        # established connections — DB connections?
netstat -s | grep retrans  # TCP retransmissions — network issue?

# Disk I/O
iostat -x 1                # is disk I/O saturated?
```

---

### Q: How do you check if a port is open on a remote host?
```bash
# Method 1 — telnet
telnet db.internal 5432

# Method 2 — nc (netcat)
nc -zv db.internal 5432

# Method 3 — curl
curl -v telnet://db.internal:5432

# Method 4 — /dev/tcp (bash built-in, no external tool)
timeout 3 bash -c 'echo >/dev/tcp/db.internal/5432' && echo "OPEN" || echo "CLOSED"
```

---

### Q: What does `2>&1` mean? Give a practical example.
> "`2>&1` redirects file descriptor 2 (stderr) to wherever file descriptor 1 (stdout) is currently pointing.
>
> Practical example:
> ```bash
> terraform apply > deploy.log 2>&1
> # Both stdout and stderr go to deploy.log — nothing lost
> ```
>
> Without `2>&1`, error messages go to the terminal and not to the log file — you'd miss the actual error when reviewing logs."

---

### Q: How do you find the top 5 CPU-consuming processes?
```bash
ps aux --sort=-%cpu | head -6   # sort by CPU descending, show top 5

# or in real-time:
top -bn1 | grep "Cpu\|PID" | head -20
```

---

### Q: How do you find files modified in the last 24 hours and larger than 100MB?
```bash
find /var/log -mtime -1 -size +100M -type f -ls
```

---

### Q: What is `systemctl` and how is it different from `service`?
> "`systemctl` is the control command for systemd — the modern Linux init system. `service` is the legacy command that in modern systems is a wrapper around `systemctl`.
>
> Key `systemctl` operations for SRE:
> ```bash
> systemctl status kubelet        # is kubelet running?
> systemctl restart kubelet       # restart after config change
> systemctl enable kubelet        # auto-start on boot
> journalctl -u kubelet -f        # follow kubelet logs
> systemctl is-active docker      # check without output
> ```"

---

### Q: How do you grep for a pattern across all Kubernetes pod logs?
```bash
# All pods in a namespace matching a label
kubectl logs -l app=orders -n retail --tail=1000 | grep "ERROR"

# Specific time range (requires structured timestamps in logs)
kubectl logs orders-abc123 --since=1h | grep "EXCEPTION"

# In CloudWatch Logs Insights:
fields @timestamp, @message
| filter @message like /ERROR/
| filter @logStream like /orders/
| sort @timestamp desc
| limit 100
```

---

## 15. AWS Services

### Q: What is the difference between SQS Standard and SQS FIFO?
| Feature | Standard | FIFO |
|---|---|---|
| Order | Best-effort ordering | Strict FIFO order |
| Delivery | At-least-once (duplicates possible) | Exactly-once |
| Throughput | Unlimited | 300 TPS (3,000 with batching) |
| Use case | High throughput, order doesn't matter | Order processing, financial transactions |

> "In my project, Checkout → Orders uses SQS Standard — high throughput matters more than ordering. For financial transactions at NatWest, SQS FIFO would be appropriate — order and exactly-once delivery are critical."

---

### Q: What is the difference between RDS Multi-AZ and Read Replicas?
| Feature | Multi-AZ | Read Replica |
|---|---|---|
| Purpose | High availability / failover | Read scalability |
| Replication | Synchronous — no data loss | Asynchronous — possible lag |
| Failover | Automatic, ~1 minute | Manual promotion required |
| Endpoint | Same endpoint (DNS updated) | Separate endpoint |
| Can it serve reads? | No — standby is passive | Yes |

> "Multi-AZ for availability. Read Replica for scaling read-heavy workloads. In my project, Orders uses Multi-AZ RDS PostgreSQL — in a financial context, automatic failover with no data loss is non-negotiable."

---

### Q: What is the difference between ElastiCache Redis and Memcached?
| Feature | Redis | Memcached |
|---|---|---|
| Data structures | Rich (lists, sets, hashes, sorted sets) | Simple key-value only |
| Persistence | Optional (RDB/AOF snapshots) | No persistence |
| Replication | Yes (primary + replica) | No built-in replication |
| Pub/Sub | Yes | No |
| Use case | Session cache, rate limiting, leaderboards, pub/sub | Simple distributed caching |

> "I use Redis for the Checkout service — it stores cart session data. Redis persistence and replication mean cart data survives an ElastiCache node restart. Memcached is simpler but I'd only use it for pure caching with acceptable data loss."

---

### Q: What is AWS CloudTrail and how do you use it for security?
> "CloudTrail records every API call made to AWS services — who called it, when, from where, and what parameters. It's the audit trail for all AWS activity.
>
> SRE use cases:
> - **Incident investigation:** 'Who deleted that S3 bucket?' — CloudTrail shows the IAM identity, timestamp, and source IP.
> - **Security alerts:** CloudWatch metric filter on CloudTrail → alert on `DeleteSecurityGroup` or `AuthorizeSecurityGroupIngress` from unexpected sources.
> - **Compliance:** Prove to auditors that infrastructure changes were authorized and documented.
> - **Detecting credential theft:** Alert if an IAM key is used from an unexpected region or IP."

---

## 16. Behavioural / HR Questions

### Q: Tell me about a time you improved system reliability.

**Situation:** Our retail EKS platform had no proactive monitoring — issues surfaced via developer Slack messages, not automated alerts. MTTD was ~15 minutes.

**Task:** Implement SLI-based monitoring to detect incidents within 2 minutes.

**Action:**
- Deployed ADOT with Java and Node.js auto-instrumentation — zero code changes required
- Configured Grafana dashboards for error rate, p99 latency, JVM heap, and HikariCP connections
- Set CloudWatch Alarms with SNS: error rate > 1% for 3 consecutive minutes → immediate page
- Added X-Ray sampling for all requests slower than 200ms — automatic capture of slow outliers
- Filtered health-check traces — reduced X-Ray cost 85%

**Result:** MTTD dropped from ~15 minutes to under 2 minutes. Two subsequent incidents were detected and mitigated before any user complaint reached the team.

---

### Q: Tell me about a time you handled a critical production incident.

**Situation:** During peak traffic, the Orders service began returning HTTP 500 errors. Error rate climbed to 8% — error budget burn rate was 80x.

**Task:** Restore service, communicate status, and prevent recurrence.

**Action:**
- Declared incident at 14:32 IST, opened incident Slack channel, paged on-call
- Opened X-Ray — identified RDS PostgreSQL spans showing 8-second query times
- Checked Grafana — HikariCP `connections_pending` metric was at 12 (pool maxed out)
- Checked deployment history — a release 20 minutes earlier increased Orders replicas from 3 to 5, but didn't increase DB connection pool size
- Immediate mitigation: increased `spring.datasource.hikari.maximum-pool-size` from 10 to 20 via Helm values commit — ArgoCD rolled it out in 90 seconds
- Error rate returned to 0% at 14:41 — total incident duration 9 minutes
- Communicated resolution to stakeholders with impact summary

**Result:** 9-minute incident resolved. Post-mortem identified that load tests weren't run before scaling pod count — action item: add HikariCP pool stress test to CI pipeline for all replicas-count changes.

---

### Q: Tell me about a time you balanced delivery speed with operational risk.

**Situation:** A feature team wanted to skip staging and deploy directly to production to meet a client deadline.

**Task:** Make a risk-based recommendation.

**Action:** I explained the concrete risks: no validation of DB migration compatibility with the new schema, no readiness probe tuning under realistic traffic, no smoke test coverage. Instead of blocking, I proposed an accelerated path: I parallelized the staging deployment with the CI build (normally sequential), and wrote automated smoke tests that could run in 10 minutes instead of the usual 2-day manual validation. I also added a feature flag to the new functionality so even if something was wrong, we could disable it without a rollback.

**Result:** Staging validation took 4 hours instead of 2 days. Production deploy was clean. The feature flag gave the team a kill switch, which built their confidence in the approach.

---

### Q: Describe a time you had to escalate a problem. How did you decide when to escalate?
> "During an EKS node upgrade, we hit an unexpected incompatibility between a Kubernetes version and our VPC CNI plugin version — pods in the new nodes couldn't get IPs. I spent 30 minutes diagnostically — checking CNI logs, IPAMD logs, EC2 ENI limits — but the root cause wasn't clear and the upgrade was blocking other teams.
>
> I escalated to the platform architect because: time-boxed effort wasn't producing results, the blast radius was growing (other teams blocked), and the root cause might have required AWS Support engagement which I needed approval to initiate.
>
> The architect identified it was an IAM permission missing from the CNI role — a known issue in that EKS version combination. Resolution took 10 more minutes.
>
> Decision to escalate: when individual effort time-boxed at 30 minutes isn't converging on a solution, when impact is growing, or when the fix requires authority I don't have."

---

### Q: Tell me about a time you disagreed with a team decision and how you handled it.
> "My team proposed disabling ArgoCD self-heal because a developer was frustrated that his manual `kubectl` changes kept being reverted. I disagreed — self-heal is what enforces GitOps and prevents undocumented changes from persisting silently.
>
> I didn't dismiss the concern. I acknowledged that the developer's workflow was genuinely interrupted. I proposed an alternative: create a dedicated non-production namespace where self-heal is disabled and developers can experiment freely. Production namespaces keep self-heal enabled.
>
> I made the case with data: showed two past incidents where self-heal caught and reverted configuration drift before it caused an outage. The team adopted my proposal. The developer got a sandbox, and production GitOps integrity was preserved."

---

### Q: What is your biggest weakness? (SRE-safe answer)
> "I sometimes go too deep into diagnosing root causes before communicating status to stakeholders. During an incident my instinct is to fully understand the problem before I speak — but in a real incident, stakeholders need frequent brief updates even while investigation is ongoing.
>
> I've been actively working on this by separating investigation and communication — I now set calendar reminders to send status updates every 10 minutes during incidents, regardless of whether I've found the root cause. It's a discipline I've built deliberately."

---

### Q: Where do you see yourself in 3 years?
> "In 3 years I want to have deepened my SRE expertise to the point where I'm driving reliability strategy — defining the SLO framework across multiple services, leading post-mortem culture, and mentoring junior engineers in reliability engineering practices. I'm also interested in the platform engineering track — building internal developer platforms that make reliability the default for development teams, not an afterthought."

---

### Q: How do you keep up with technology changes in this fast-moving field?
> "I follow a structured approach:
> - Weekly: KubeWeekly newsletter, AWS blog (filter: EKS, observability), CNCF blog
> - Monthly: SREcon and KubeCon session recordings on YouTube
> - Quarterly: re:Invent sessions (especially AWS containers and observability track)
> - Hands-on: I maintain a personal AWS lab where I experiment — Karpenter v1 GA, Cilium as CNI, OpenFeature for feature flags
> - Community: AWS User Group Bangalore, CNCF Slack channels
>
> I focus on depth over breadth — I'd rather deeply understand Kubernetes internals than have shallow knowledge of 20 tools."

---

## 17. Scenario-Based / Problem-Solving

### Q: Your SLO states 99.9% availability. You've had 50 minutes of downtime this month and it's the 20th. What do you do?
> "50 minutes used out of 43.8 allowed — we've already exceeded the monthly error budget by 6 minutes. Actions:
>
> 1. **Declare budget exhausted:** Communicate to the engineering team and product — error budget is burned for the month.
> 2. **Feature freeze:** No non-critical releases until next month or until reliability improves. The SLO framework is the business justification — not my opinion.
> 3. **Root cause deep dive:** What caused the 50 minutes? Are they from one incident or multiple? If one incident, fix the root cause. If multiple small incidents, find the common pattern.
> 4. **Reliability sprint:** Redirect sprint capacity to: improved alerting, runbook automation, better rollback speed, or infrastructure redundancy — wherever the post-mortem points.
> 5. **Revise SLO if needed:** If this is a pattern across months, the SLO might be too aggressive for the current system state. Discuss with stakeholders whether to invest in reliability or adjust expectations."

---

### Q: A critical service is down. Your rollback fails. What do you do?
> "Cascade of options:
>
> 1. **Diagnose the rollback failure first:** Is ArgoCD failing to sync? Is the previous image missing from ECR? Is the DB schema incompatible with the old code?
> 2. **Manual rollback:** If ArgoCD can't sync, do a direct `helm rollback <release> <revision>` or `kubectl set image deployment/orders orders=<old-image-tag>`.
> 3. **Forward fix:** If rollback is truly blocked (e.g., DB migration already ran that's incompatible with old code), a forward fix may be faster — deploy a hotfix branch that patches the regression.
> 4. **Traffic diversion:** If the service can't be fixed quickly, can traffic be routed to a backup or degraded mode? Serve cached results, disable the feature, or return a friendly error.
> 5. **Escalate:** If I've been working for 20 minutes without progress, escalate to the platform architect and consider AWS Support if it's an EKS/AWS issue.
> 6. **Communicate constantly:** Stakeholders get an update every 10 minutes regardless of progress."

---

### Q: You notice Kubernetes nodes are running at 90% memory consistently. What do you do?
> "This is a capacity and stability risk — one memory spike on any node will cause OOMKill evictions across multiple pods, potentially cascading.
>
> Step 1 — Investigate which pods are consuming the most memory:
> ```bash
> kubectl top pods -n retail --sort-by=memory | head -20
> ```
>
> Step 2 — Check if pods have accurate resource limits or if any are missing limits (memory unlimited pods can consume everything).
>
> Step 3 — Check if this is a memory leak trend (growing over time) or stable high utilisation.
>
> Step 4 — Short-term: add more nodes to the Karpenter NodePool or increase `maxNodes`. For on-demand stability: adjust the Karpenter consolidation policy to keep more buffer.
>
> Step 5 — Long-term: right-size pod memory limits (reduce over-provisioning), identify and fix any memory leaks in application code, adjust JVM heap sizing."

---

### Q: You need to deploy a configuration change to 200 microservice instances with zero downtime. How?
> "GitOps rolling approach:
> 1. Make the configuration change in the Helm chart's `values.yaml` or `ConfigMap`.
> 2. For ConfigMap changes, Kubernetes doesn't automatically restart pods — need to trigger a rolling restart: `kubectl rollout restart deployment/orders`.
> 3. With `maxUnavailable: 0` in rolling update strategy, pods are replaced one at a time.
> 4. Each new pod reads the updated ConfigMap on startup.
> 5. Readiness probes ensure the pod is healthy before the next pod is replaced.
> 6. Grafana monitors error rate throughout — if it spikes, ArgoCD rollback is triggered.
>
> For a config change that's truly zero-risk (no code change, no restart needed), consider using an external config store like AWS AppConfig with live reload — the app polls for changes without any restart."

---

### Q: How would you design the monitoring for a brand new payment processing service?

**SLIs to define first:**
- Payment success rate (successful transactions / total transaction attempts)
- Payment latency p99 (end-to-end time from request to response)
- Payment idempotency failure rate (duplicate payment detection)

**Alerting:**
- Error rate > 0.1% → page immediately (financial impact is severe)
- p99 latency > 500ms → page (user experience in payments is critical)
- Any HTTP 500 on payment endpoints → immediate investigation

**Dashboards:**
- Transaction volume (RPS), success rate, error rate by error type
- End-to-end latency percentiles (p50/p95/p99/p999)
- DB connection pool (critical — payment DB must never be saturated)
- External payment gateway call success rate and latency
- Queue depth (if async) — time since oldest unpaid message

**Additional for financial context:**
- Audit log completeness — every payment attempt logged, no gaps
- Idempotency key collision rate
- Circuit breaker state (open/closed) for payment gateway dependency

---

## 18. NatWest / Financial Services Specific

### Q: What SRE challenges are unique to financial services?

1. **Regulatory compliance:** Changes must be auditable — GitOps solves this with PR history.
2. **Zero-downtime requirement is stricter:** A banking outage has regulatory, reputational, and financial consequences. SLO targets must be tighter (99.99% vs 99.9%).
3. **Data integrity over availability:** In a financial system, returning stale data is worse than returning an error. Design for consistency.
4. **Strict change management:** CAB processes, change freeze windows, evidence of testing.
5. **Incident communication is regulated:** DORA (Digital Operational Resilience Act in EU) and FCA in UK require structured incident reporting.
6. **Security is non-negotiable:** Encryption at rest and in transit, access auditing, secret management — all must be proven, not assumed.

---

### Q: What is the FCA and DORA? How do they relate to SRE?
> "The FCA (Financial Conduct Authority) regulates financial firms in the UK. DORA (Digital Operational Resilience Act) is EU regulation that mandates operational resilience standards for financial entities — covering incident management, testing, third-party risk, and ICT risk.
>
> SRE alignment:
> - **Incident reporting:** DORA requires classification and reporting of major incidents to regulators. SRE post-mortem culture produces exactly the documentation needed.
> - **Resilience testing:** DORA mandates threat-led penetration testing and resilience exercises — chaos engineering is the SRE equivalent.
> - **Third-party risk:** Cloud providers (AWS) are classified as ICT third parties — their reliability directly impacts NatWest's DORA obligations.
> - **Operational metrics:** SLO tracking and error budget management provide the quantitative resilience data regulators require."

---

### Q: What is an SRE's responsibility during a major banking outage? How is it different from a regular incident?

> "A major outage in a bank involves:
> 1. **Immediate notification chain:** Not just on-call engineer — senior management, compliance, potentially regulators if the outage exceeds thresholds.
> 2. **War room:** Multiple teams join — SRE, development, infrastructure, security, comms.
> 3. **Designated incident commander:** SRE takes the IC role — coordinates investigation, assigns work streams, controls communication.
> 4. **Customer communication:** External status page updated, social media monitored, call centre briefed.
> 5. **Regulatory timeline:** If major systems are down > 2 hours, FCA notification may be required.
> 6. **Evidence preservation:** All actions logged with timestamps — logs must not be altered, for regulatory review.
>
> SRE's specific role: restore service (MTTR focus) and provide technical status updates to the IC every 5 minutes."

---

### Q: How would you handle a security breach discovered during an incident?

> "Immediately shift from incident response to security incident response — the priority changes from 'restore service' to 'contain the breach':
>
> 1. **Isolate:** Isolate the affected pods/services — remove from service endpoints, apply restrictive NetworkPolicy.
> 2. **Preserve evidence:** Do not restart pods or clean logs — preserve state for forensics.
> 3. **Escalate:** Page security team and CISO immediately — this is no longer an SRE-only incident.
> 4. **Assess scope:** What data was accessed? What IAM roles? Check CloudTrail for API calls made from the compromised identity.
> 5. **Rotate credentials:** Revoke all affected IAM keys, rotate secrets in Secrets Manager.
> 6. **Regulatory obligation:** If customer data was exposed, GDPR breach notification may be required within 72 hours.
> 7. **Do not attempt to fix silently:** Transparency with legal and compliance teams is mandatory."

---

### Q: Why is auditability important in SRE at a bank? How does your toolchain support it?

> "In financial services, you must be able to answer: 'Who changed what, when, and why?' for any infrastructure or configuration change.
>
> My toolchain provides full auditability:
> - **Terraform:** Every infrastructure change is a Git commit with author, timestamp, PR reviewers, and `terraform plan` output.
> - **ArgoCD:** Every deployment records the Git commit hash deployed, who merged the PR, and when.
> - **CloudTrail:** Every AWS API call — who assumed which IAM role, from which IP, at what time.
> - **Secrets Manager:** Every secret read is logged with the identity and timestamp.
> - **GitHub Actions:** Every pipeline run has an audit log — who triggered it, what commit it built, which tests passed.
>
> This satisfies both internal audit requirements and regulator requests for evidence of controlled change management."

---

## 19. Questions to Ask the Interviewer

> Choose 3–4 based on how the interview went. Asking zero questions signals disinterest.

1. **"How does NatWest define and track SLOs today? Is there a platform-wide framework or does each team define their own?"**

2. **"What does the on-call rotation look like for this role? How mature is the tooling for alert routing and escalation?"**

3. **"What is the biggest reliability challenge the SRE team is working on right now? Is it more about detection, recovery speed, or prevention?"**

4. **"How are SRE teams embedded with feature teams here — dedicated SRE model, or a consulting/embedded model?"**

5. **"What does the observability stack currently look like? Are you standardizing on any tools across teams?"**

6. **"How much of the platform is already cloud-native vs legacy systems? What's the modernization roadmap?"**

7. **"What does a typical on-call incident look like in terms of tooling — how does an alert reach the on-call engineer and what do they have access to for diagnosis?"**

8. **"What does success look like for someone in this role after 6 months?"**

---

## QUICK REVISION CHEAT SHEET

### SRE Formulas
```
Error Budget     = 1 - SLO target
                 = 1 - 0.999 = 0.001 = 43.8 min/month (for 99.9%)

Availability     = Uptime / (Uptime + Downtime)

MTTD             = Total detection time / Number of incidents
MTTR             = Total recovery time / Number of incidents

Burn Rate        = Error rate / Error budget rate
                 = (1/0.001) × error_rate_fraction
```

### Availability Table
| SLO | Monthly downtime | Annual downtime |
|---|---|---|
| 99% | 7.3 hours | 3.65 days |
| 99.5% | 3.6 hours | 1.83 days |
| 99.9% | 43.8 minutes | 8.76 hours |
| 99.95% | 21.9 minutes | 4.38 hours |
| 99.99% | 4.38 minutes | 52.6 minutes |
| 99.999% | 26.3 seconds | 5.26 minutes |

### Key kubectl Commands for Incidents
```bash
kubectl get pods -A                              # all pods all namespaces
kubectl describe pod <name> -n <ns>             # events and state
kubectl logs <pod> --previous                   # last crash logs
kubectl top pods -n <ns> --sort-by=cpu          # resource consumers
kubectl rollout status deployment/<name>        # is rollout complete?
kubectl rollout undo deployment/<name>          # rollback
kubectl get events -n <ns> --sort-by=.metadata.creationTimestamp
kubectl exec -it <pod> -- /bin/sh               # enter pod
kubectl port-forward svc/<name> 8080:8080       # test service locally
```

### ArgoCD Commands
```bash
argocd app list                          # list all apps and sync status
argocd app sync <app-name>              # force sync
argocd app rollback <app-name> <rev>    # rollback to revision number
argocd app history <app-name>           # deployment history
argocd app diff <app-name>              # show drift between git and cluster
```

### Golden Signals (Google SRE Book)
| Signal | What it measures | Alert threshold example |
|---|---|---|
| Latency | Time to serve a request | p99 > 500ms for 3 minutes |
| Traffic | Demand on the system | RPS > 10,000 (capacity alert) |
| Errors | Rate of failed requests | Error rate > 1% for 3 minutes |
| Saturation | System resource usage | CPU > 80%, Memory > 85% |

**Monitor errors and latency for user-facing SLOs. Monitor saturation for capacity planning.**
