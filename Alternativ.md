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
