# Alternative Interview Questions

---

## 1. Linux / Shell

- How do you print the first column of a file using `awk`?
- What is the difference between `awk`, `sed`, and `grep`?
- How do you find all files modified in the last 7 days using `find`?
- What does `2>&1` mean in shell commands?
- How do you count the number of lines in a file?
- What is the difference between `>` and `>>` in shell?
- How do you pass environment variables to a script?
- What does `chmod 644` mean? When would you use it?
- What is the difference between `chmod 755` and `chmod 777`?
- What are sticky bit, setuid, and setgid permissions?
- How do you change file ownership using `chown`?
- What is `umask` and how does it affect file permissions?
- How do you recursively change permissions on a directory?
- What permission should a private SSH key (`.pem`) have and why?

---

## 2. AI / Concepts

- What is the difference between an AI model and an AI agent?
- What are the core components of an agentic AI system?
- What is RAG (Retrieval-Augmented Generation)?
- What is prompt engineering?
- What is the difference between LLM and a fine-tuned model?
- How does tool/function calling work in LLMs?
- What is an AI orchestration framework? (LangChain, AutoGen, etc.)

---

## 3. Docker

- What is a Docker image and how is it different from a Docker container?
- What is containerization and how is it different from virtualisation?
- What are the advantages of containers over virtual machines?
- How does a container share the host OS kernel?
- What is the difference between Docker image and Docker container?
- What is a Docker volume and when would you use it?
- How do you reduce Docker image size?
- What is the difference between `COPY` and `ADD` in a Dockerfile?
- What is `docker-compose` and how is it different from Kubernetes?
- How do you pass secrets securely into a Docker container?
- What happens when you run `docker build`? Explain layer caching.
- What is a distroless image? Why use it?
- How do you inspect a running container?
- What is the difference between `docker stop` and `docker kill`?
- Given a 10-line and a 20-line Dockerfile — which is better and why?
- How do you minimise Docker image layers?
- What is the impact of layer ordering on Docker build cache?
- How do you run a container as a non-root user?
- What is `.dockerignore` and why is it important?
- What is the difference between `scratch`, `alpine`, and `distroless` base images?
- How do you scan a Docker image for vulnerabilities? (Trivy, Snyk, etc.)

---

## 4. Terraform

- What is the difference between `terraform plan` and `terraform apply`?
- What are Terraform modules and why use them?
- How do you version and share Terraform modules across teams?
- What is the difference between a root module and a child module?
- How do you pass outputs from one module to another?
- What is the Terraform Registry?
- What is a Terraform provider?
- How does Terraform handle dependency between resources?
- What is `terraform import` used for?
- What happens if the state file is deleted?
- What is state locking and why is it important?
- How do you handle sensitive values/secrets in Terraform?
- What is the difference between `local` and `remote` backend?
- What is `terraform taint` / `terraform untaint`?
- What is infrastructure drift and how does Terraform detect it?
- A colleague ran `terraform destroy` on production by mistake — how do you recover?
- How do you prevent manual changes to infrastructure managed by Terraform?
- What is `terraform plan -refresh-only` and when would you use it?
- How do you use `terraform state rm` and when is it needed?
- What is the difference between `terraform refresh` and `terraform plan`?
- How would you enforce that no one can delete critical infrastructure (e.g., RDS) using Terraform?

---

## 5. JFrog / Artifact Management

- What is the difference between local, remote, and virtual repositories in Artifactory?
- How does JFrog Xray work?
- What is artifact promotion and why is it important?
- How do you integrate JFrog Artifactory with Jenkins / GitHub Actions?
- What is the difference between JFrog Artifactory and Nexus?
- How do you enforce license compliance using JFrog?
- What is a Bill of Materials (SBOM) and how does JFrog support it?
- How does Artifactory proxy an external registry like DockerHub?

---

## 6. Git

- What is the difference between `git merge` and `git rebase`?
- What is `git stash` and when would you use it?
- How do you revert a commit that has already been pushed?
- What is the difference between `git reset --soft`, `--mixed`, and `--hard`?
- What is a detached HEAD state in Git?
- How do you resolve a merge conflict?
- What is `git bisect` used for?
- What is the difference between `git fetch` and `git pull`?
- How do you delete a remote branch?
- What is a Git hook? Give an example use case.

---

## 7. Networking

- What is the difference between TCP and UDP?
- What happens during a TLS handshake?
- What is a CDN and how does it improve performance?
- What is the difference between HTTP/1.1, HTTP/2, and HTTP/3?
- What is DNS and how does it resolve a domain name?
- What is the difference between a load balancer and a reverse proxy?
- What is CORS and why does it exist?
- What is the OSI model? Explain each layer briefly.
- What is the difference between a public IP and a private IP?
- What is NAT (Network Address Translation)?

---

## 8. Kubernetes (Bonus — Common in DevOps Interviews)

- What is a Pod in Kubernetes and why does it exist?
- What is the difference between a Pod and a container?
- Can a Pod have multiple containers? When would you use that pattern?
- What is a sidecar container and when would you use one?
- How do containers inside the same Pod communicate with each other?
- What is Kubelet and what is its role in a Kubernetes cluster?
- What does Kubelet do when a container crashes?
- What is the difference between Kubelet, Kube-proxy, and the API server?
- What container runtime does Kubelet interact with?
- What is the difference between a Pod, Deployment, and StatefulSet?
- What is a Kubernetes Service? Types?
- How does Kubernetes handle self-healing?
- What is a ConfigMap vs Secret?
- How does HPA (Horizontal Pod Autoscaler) work?
- What is an Ingress controller?
- What happens when a pod is in `CrashLoopBackOff`?
- What is the difference between `kubectl apply` and `kubectl create`?
- What is a namespace in Kubernetes?
- How do you roll back a Kubernetes deployment?
- What is a Kubernetes NetworkPolicy and why is it important?
- What is the default network behaviour between pods in Kubernetes?
- How do you implement a default-deny-all NetworkPolicy?
- What CNI plugins support NetworkPolicy enforcement? (Calico, Cilium, Weave)
- How would you allow only a frontend pod to communicate with a backend pod?
- What is the difference between Ingress and NetworkPolicy in Kubernetes?
- How do you restrict egress traffic from a pod?
- What is mTLS and how does a service mesh (Istio/Linkerd) improve pod-to-pod security over NetworkPolicy?

---

## 9. Security / Secrets Management (Bonus)

- What is the difference between AWS Secrets Manager and AWS Parameter Store?
- How do you rotate secrets automatically in AWS Secrets Manager?
- How do you audit who accessed a secret in AWS Secrets Manager?
- What is AWS CloudTrail and what events does it capture?
- How do you set up a CloudWatch alarm for suspicious Secrets Manager access?
- What is AWS GuardDuty and how does it detect threats?
- How do you implement least-privilege access for IAM roles?
- What is a resource-based policy in Secrets Manager?
- How do you inject secrets into a Kubernetes pod securely?
- What is the AWS Secrets Manager CSI driver for Kubernetes?
- How do you prevent hardcoded secrets in your codebase?
- What is SAST and how does it help detect secrets in code?

---

## 10. CI/CD — GitHub Actions (Bonus)

- The CI/CD pipeline is green but the app is down — where do you start investigating?
- How do you check application logs in a Kubernetes cluster?
- What is the difference between `kubectl logs` and `kubectl describe`?
- How do you view logs from a previously crashed container?
- What does `CrashLoopBackOff` mean and how do you debug it?
- What is the difference between a liveness probe and a readiness probe?
- How does a passing pipeline not guarantee application health?
- What is the difference between CI and CD?
- What is a pipeline as code?
- How do you handle secrets in a CI/CD pipeline?
- What is a blue-green deployment?
- What is a canary deployment?
- How do you trigger a pipeline on a pull request?
- What is the difference between a build artifact and a deployment artifact?
- What is the difference between GitHub Actions and Jenkins?
- How does GitHub Actions authenticate with AWS securely? (OIDC vs access keys)
- What is a self-hosted runner in GitHub Actions? When would you use one?
- What is the difference between `jobs.<job>.needs` and `jobs.<job>.if`?
- How do you implement a manual approval gate in GitHub Actions?
- What is a reusable workflow and how is it different from a composite action?
- How do you cache dependencies in GitHub Actions to speed up builds?
- How do you pass data between jobs in GitHub Actions?
- What is a matrix strategy in GitHub Actions?
- How do you roll back a deployment automatically if a health check fails in GitHub Actions?

---

## 11. Cloud Services (AWS)

- What is the difference between EC2 and Lambda?
- What is the difference between ECS and EKS?
- What is an IAM role vs IAM user vs IAM policy?
- What is VPC peering and when would you use it?
- What is the difference between S3 and EBS and EFS?
- What is the difference between ALB and NLB?
- How does Auto Scaling work in AWS?
- What is AWS CloudFormation and how is it different from Terraform?
- What is the difference between SQS and SNS?
- What is the difference between RDS and DynamoDB?
- How do you secure an S3 bucket from public access?
- What is AWS Config and how is it different from CloudTrail?
- What is a NAT Gateway and why is it needed in a private subnet?
- What is the difference between a Security Group and a NACL?
- How does AWS IAM OIDC work with GitHub Actions?

---

## 12. Behavioral — Automation

- Have you ever automated a manual process? Walk me through it.
- What tools have you used for automation?
- How do you decide what to automate vs what to do manually?
- Have you written any scripts to reduce toil? What did they do?
- Describe a time when automation you built saved significant time or prevented a failure.
- What would you automate first if you joined a new team?
- How do you test automation scripts before running them in production?
- Have you used Ansible or other configuration management tools? For what?
- How have you automated security scanning in your CI/CD pipeline?
- What is infrastructure as code and have you used it for automation?

---

## 13. Change Request (CR) — ITSM / DevOps Process

- What is a Change Request (CR) and why is it used in IT operations?
- What are the three types of Change Requests? When is each used?
- What is the difference between a Standard CR and a Normal CR?
- What is CAB (Change Advisory Board) and who sits on it?
- What information must be in a CR before it can be approved?
- Why is a rollback plan mandatory in a CR?
- What is a maintenance window and why does it matter in a CR?
- How does a CR fit into a CI/CD pipeline? At what stage is it raised?
- What is the difference between a CR and a ticket/incident?
- In which scenarios would you raise an Emergency CR vs a Normal CR?
- What tools are used to manage Change Requests? (ServiceNow, Jira SM, BMC Remedy)
- How do you handle a situation where a production deployment fails mid-CR?
- What is post-implementation review (PIR) in the context of an Emergency CR?
- How do you ensure a CR doesn't cause a production outage?
- What happens if someone deploys to production without raising a CR?
- How does GitOps (ArgoCD) integrate with the CR process?
- What is the difference between ITIL Change Management and Agile deployment practices?
- How do you track CR compliance across multiple teams in a large organisation?

---

## 14. Microservices Architecture (Retail Store Project)

- Walk me through a microservices architecture you have worked on.
- What is the Database-per-Service pattern and why is it used?
- What is polyglot persistence? Give an example from your project.
- How do microservices communicate synchronously vs asynchronously?
- When would you use SQS over direct REST calls between services?
- How do services discover each other inside a Kubernetes cluster?
- What is the difference between ClusterIP, NodePort, and LoadBalancer service types?
- How does a UI service aggregate data from multiple backend services?
- What are the trade-offs of a microservices architecture vs a monolith?
- How do you handle distributed transactions across microservices?
- What is the strangler fig pattern?
- How do you handle service failures in a microservices system? (circuit breaker, retry, timeout)
- What is an API gateway and where does it sit in a microservices architecture?
- How do you manage different technology stacks across microservices in one Kubernetes cluster?

---

## 15. Terraform — Deep Dive

### CLI & Basics
- What does `terraform init` do exactly? What files does it create?
- What is the difference between `terraform validate` and `terraform plan`?
- How do you preview changes without applying them?
- What is the `.terraform.lock.hcl` file and why should it be committed to git?
- What is `terraform fmt` and when should it be run?
- How do you target a specific resource in `terraform apply`? (`-target` flag)
- What is `terraform state mv` and when would you use it?
- What is `terraform state rm` and when would you use it?
- What is `terraform import` and what are its limitations?
- What happens if you run `terraform apply` when no changes are detected?

### Language Syntax & Blocks
- What are the main block types in Terraform HCL?
- What goes inside the `terraform {}` settings block?
- What is the difference between `required_version` and `required_providers`?
- What does `~> 5.0` mean in a provider version constraint?
- What is the difference between a `resource` block and a `data` block?
- How do you configure multiple AWS regions in one Terraform config? (provider aliases)
- What is a `locals` block? How is it different from a `variable` block?
- Can you call a function inside a `locals` block?

### Meta-Arguments
- What are Terraform meta-arguments? List all of them.
- When do you need `depends_on`? Why doesn't Terraform always detect dependencies automatically?
- What is the difference between `count` and `for_each`?
- Why is `for_each` preferred over `count` for most use cases?
- What is the problem with deleting index 0 when using `count`?
- What is `lifecycle { create_before_destroy = true }` and when do you use it?
- What is `lifecycle { prevent_destroy = true }` — what does it protect against?
- What is `lifecycle { ignore_changes = [...] }` and give a real use case?

### Input Variables
- What are all the ways to assign a value to a Terraform variable?
- What is the difference between `terraform.tfvars` and `*.auto.tfvars`?
- What is a sensitive variable and what does it actually protect?
- Does `sensitive = true` prevent the value from being stored in state?
- How do you pass a list variable from the CLI?
- How do you define a map variable with a default value?
- What happens if a required variable has no default and no value is provided?
- How do you use `TF_VAR_` environment variables?

### Outputs, Locals & Functions
- What is an output value and how do modules use them?
- How do you use an output from a child module in a parent module?
- What is the `file()` function? When would you use it vs a variable?
- What does `toset()` do and why is it needed for `for_each`?
- What is the difference between `toset()` and `tolist()`?
- What does `keys()` return? Give a practical example.
- What does `tomap()` do?
- What is the difference between `lookup()` and direct map indexing?

### For Loops & Splat
- How do you write a for loop that filters elements? (with `if` condition)
- What is the difference between a for expression that returns a list vs a map?
- What is the legacy splat operator `.*`? What replaced it?
- Why is `[*]` preferred over `.*`?
- How do you get all private IPs from a `count`-based resource as a list?

### Provisioners & Null Resource
- What is a Terraform provisioner? Why are they a "last resort"?
- What is the difference between `local-exec` and `remote-exec`?
- What is the `file` provisioner used for?
- What is a `null_resource`? When would you use one?
- What are `triggers` in a `null_resource` and how do they work?
- Why do provisioners break idempotency?
- What is the alternative to provisioners for bootstrapping EC2 instances?

### Modules
- What is the difference between a public registry module and a local module?
- How do you upgrade a module to a newer version?
- What does `terraform init -upgrade` do?
- How do you pass variables into a module?
- How do you get outputs from a module?
- What is the difference between `source = "terraform-aws-modules/vpc/aws"` and a local path?
- What is the `random` provider used for? Give a real example.
- Why do you use `random_id` for S3 bucket names?

---

## 16. Kubernetes — Advanced

- What is the difference between `kubectl apply` and `kubectl replace`?
- How does Kubernetes scheduler decide which node to place a pod on?
- What is node affinity vs pod affinity vs taints and tolerations?
- What is a DaemonSet and when would you use one?
- What is a Job vs a CronJob in Kubernetes?
- What is a init container and what is it used for?
- What is the difference between `emptyDir`, `hostPath`, and PVC volumes?
- How do you debug a pod that is stuck in `Pending` state?
- How do you debug a pod that is stuck in `ImagePullBackOff`?
- What is `kubectl port-forward` and when would you use it?
- How do you exec into a running container?
- What is the difference between `kubectl delete pod` and `kubectl delete deployment`?
- What is RBAC in Kubernetes? Explain Role, ClusterRole, RoleBinding, ClusterRoleBinding.
- How do you restrict a service account to only read pods in one namespace?
- What is a mutating webhook? Give a real example (ADOT auto-instrumentation).
- What is a validating webhook?
- What is the difference between HPA and VPA?
- What is KEDA and how does it extend HPA?

---

## 17. Observability — OpenTelemetry & AWS

- What are the three pillars of observability?
- What is OpenTelemetry (OTEL)?
- What is the difference between a trace, a span, and a log?
- What is the OTEL Collector and why use it instead of sending data directly to backends?
- What is ADOT (AWS Distro for OpenTelemetry)?
- How does auto-instrumentation work without changing application code?
- What is AWS X-Ray and what problem does it solve?
- What is Amazon Managed Prometheus (AMP)?
- What is Amazon Managed Grafana (AMG)?
- How do you reduce observability costs using sampling and filtering?
- What is head-based sampling vs tail-based sampling?
- How do you filter out health check traces in the OTEL Collector?
- What is a service map in X-Ray?
- How do you correlate logs and traces for a single request?
- What is a CloudWatch Logs Insights query? Write one to find all ERROR logs.
- What is the difference between CloudWatch metrics and Prometheus metrics?

---

## 18. ArgoCD & GitOps

- What is GitOps and how is it different from traditional CI/CD?
- What is the single source of truth in a GitOps setup?
- How does ArgoCD detect when the cluster state drifts from git?
- What is ArgoCD's sync policy? Difference between manual and auto sync?
- What is self-heal in ArgoCD?
- What is prune in ArgoCD? Why is it disabled by default?
- How do you roll back using ArgoCD?
- How does ArgoCD integrate with Helm?
- What is an ArgoCD Application? What does it define?
- How does ArgoCD handle secrets (since they shouldn't be in git)?
- What is the difference between ArgoCD and Flux?
- How do you implement multi-cluster deployments with ArgoCD?
- What is an ApplicationSet in ArgoCD?
- How do you handle database migrations in a GitOps workflow?
- What is the difference between `argocd app sync` and `argocd app diff`?
- You have 50 Kubernetes clusters — how do you manage configurations without doing it manually?
- What is an ArgoCD ApplicationSet generator? What types of generators exist?
- How do you add a new cluster to ArgoCD's management with zero manual YAML duplication?
- What is the difference between a List generator and a Cluster generator in ApplicationSet?
- How do you handle cluster-specific configuration overrides in a shared Helm chart?
- What happens in ArgoCD if you have 50 clusters and push one image tag change?
- How do you ensure all 50 clusters stay in sync with git using ArgoCD?
- What is the difference between ArgoCD `selfHeal` and `prune`?

---

## 19. Kubernetes Cluster Patching & Upgrades

- Do you patch your Kubernetes worker nodes? What triggers a patch?
- What is the difference between an OS patch and a Kubernetes version upgrade?
- How do you update node AMIs in EKS using Terraform?
- What is a rolling node replacement and how does Kubernetes ensure zero downtime during it?
- Who manages the EKS control plane (master nodes) — you or AWS?
- Can you SSH into EKS master nodes? Why or why not?
- What is the maximum number of minor versions you can skip in an EKS upgrade?
- What add-ons need to be upgraded before or alongside a Kubernetes version upgrade?
- Walk me through upgrading EKS from version 1.28 to 1.30.
- What is the compatibility matrix and why does it matter for EKS add-ons?
- How long does an EKS control plane upgrade typically take?
- How do you verify a Kubernetes upgrade was successful?
- What approvals do you need before patching a production Kubernetes cluster?
- What is the difference between a Normal CR and an Emergency CR in the context of a CVE patch?
- What CVSS score triggers an Emergency CR vs a Normal CR?
- How do you test a patch in lower environments before applying to production?
- What is your rollback plan if a Kubernetes node upgrade fails?
- How do you drain a node safely before patching it?
- What is `kubectl cordon` and `kubectl drain`? When do you use each?
- What happens to running pods when you drain a node?
- What is a PodDisruptionBudget and how does it protect availability during node patching?

---

## 20. Node Scaling & Troubleshooting

- How do you increase the number of worker nodes in an EKS cluster?
- What is the difference between manually scaling a node group vs using autoscaling?
- How do you update `desired_size` in a Terraform-managed EKS node group?
- A new node is added but stays in `NotReady` — what do you check?
- What IAM policies are required for a worker node to join an EKS cluster?
- What happens if the subnet runs out of IP addresses when new nodes try to join?
- How do you check if an EC2 instance successfully bootstrapped as a Kubernetes node?
- Where do you find the bootstrap logs on an EKS worker node?
- What security group rules are required between worker nodes and the EKS control plane?
- How do you check why a node is not registering with the API server?
- What does `kubectl describe node` tell you?
- How do you remove a failed node from the cluster cleanly?
- What is the difference between `kubectl delete node` and terminating the EC2 instance?

---

## 21. Autoscaling & Traffic Management

- What is the difference between Karpenter and an Auto Scaling Group (ASG)?
- If you're not using Cluster Autoscaler, how does your cluster handle traffic spikes?
- How does HPA + Karpenter work together to handle a sudden traffic spike?
- What happens to end users if pods scale up but new nodes take too long to provision?
- How do you set HPA thresholds to give enough buffer before nodes need to scale?
- What is Karpenter consolidation and when does it run?
- How do you prevent Karpenter from terminating nodes that have long-running jobs?
- What is a NodePool limit in Karpenter and why should you set one?
- How do you forecast traffic to size your Kubernetes cluster appropriately?
- What tools do you use to analyse historical traffic patterns?
- What is load testing and how do you use it before a major product launch?
- How do CloudWatch Anomaly Detection or Datadog forecasting help with capacity planning?
- What CloudWatch metrics or alarms tell you that the cluster is approaching its limits?
- How do you handle a sudden 10x traffic spike that wasn't predicted?
- What is the difference between horizontal and vertical scaling in Kubernetes?
- How does KEDA extend HPA beyond CPU/memory metrics?
