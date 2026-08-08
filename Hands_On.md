# Hands-On Scenario Steps — Production Practiced Way

> **Production Rules applied throughout:**
> - Always verify current state BEFORE making any change
> - Use `--dry-run=client` or `terraform plan` before every apply
> - Always have a rollback command ready BEFORE executing the change
> - Work on DEV → STAGING → PROD in sequence — never skip
> - Set your kubectl context explicitly before every command — never assume
> - Every production change requires a CR reference number
> - Notify the team in Slack before and after every production change

---

## SCENARIO 1: ArgoCD ApplicationSet — Managing 50 Clusters (Production Way)

### Pre-Change Checklist
```bash
# 1. Set context to management cluster where ArgoCD runs
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/mgmt-cluster
kubectl config current-context   # confirm before proceeding

# 2. Verify ArgoCD is healthy
kubectl get pods -n argocd
# All pods must be Running before proceeding

# 3. Verify you have access to the GitOps repo
git clone https://github.com/org/gitops-repo && cd gitops-repo
git status

# 4. Check how many clusters are currently managed
argocd login argocd.internal.example.com \
  --username admin \
  --password $(kubectl get secret argocd-initial-admin-secret -n argocd \
               -o jsonpath='{.data.password}' | base64 -d) \
  --insecure
argocd cluster list   # note current count
```

### Step 1: Register clusters in ArgoCD (one-time per cluster)
```bash
# Load all cluster contexts into kubeconfig first
aws eks update-kubeconfig --region us-east-1 --name cluster-us-east-1 --alias cluster-us-east-1
aws eks update-kubeconfig --region eu-west-1 --name cluster-eu-west-1 --alias cluster-eu-west-1

# Register each cluster in ArgoCD (DO NOT run on prod cluster kubeconfig)
# This creates a ServiceAccount in each cluster for ArgoCD to use
argocd cluster add cluster-us-east-1 \
  --name cluster-us-east-1 \
  --label env=prod \
  --label region=us-east-1

argocd cluster add cluster-eu-west-1 \
  --name cluster-eu-west-1 \
  --label env=prod \
  --label region=eu-west-1

# Verify registration
argocd cluster list
argocd cluster get cluster-us-east-1   # check CONNECTION STATUS = Successful
```

### Step 2: GitOps repo structure — set it up once, reuse for all clusters
```bash
cd gitops-repo

# Structure: shared templates + per-cluster overrides only
mkdir -p charts/retail-store/templates
mkdir -p apps/

# Global defaults (applies to ALL clusters)
cat > charts/retail-store/values.yaml << 'EOF'
image:
  repository: 123456789.dkr.ecr.us-east-1.amazonaws.com/retail-ui
  tag: v1.0.0
  pullPolicy: IfNotPresent

replicaCount: 2

resources:
  requests:
    cpu: 250m
    memory: 256Mi
  limits:
    cpu: 500m
    memory: 512Mi

service:
  type: ClusterIP
  port: 8080
EOF

# Production environment overrides
cat > charts/retail-store/values-prod.yaml << 'EOF'
replicaCount: 3
resources:
  requests:
    cpu: 500m
    memory: 512Mi
  limits:
    cpu: 1000m
    memory: 1Gi
EOF

# Cluster-specific overrides (only what's different per cluster)
cat > charts/retail-store/values-cluster-us-east-1.yaml << 'EOF'
ingress:
  hostname: retail.us-east-1.example.com
  certificateArn: arn:aws:acm:us-east-1:123:certificate/abc
config:
  region: us-east-1
  rdsEndpoint: catalog-db.us-east-1.rds.amazonaws.com
EOF

cat > charts/retail-store/values-cluster-eu-west-1.yaml << 'EOF'
ingress:
  hostname: retail.eu-west-1.example.com
  certificateArn: arn:aws:acm:eu-west-1:123:certificate/xyz
config:
  region: eu-west-1
  rdsEndpoint: catalog-db.eu-west-1.rds.amazonaws.com
EOF
```

### Step 3: Create ApplicationSet — one file manages all 50 clusters
```bash
cat > apps/retail-store-applicationset.yaml << 'EOF'
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: retail-store
  namespace: argocd
  annotations:
    cr-reference: "CHG0012345"       # CR number for audit trail
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
    - list:
        elements:
          - cluster: cluster-us-east-1
            env: prod
            url: https://AAAAABBBBB.gr7.us-east-1.eks.amazonaws.com
          - cluster: cluster-eu-west-1
            env: prod
            url: https://CCCCCDDDD.gr7.eu-west-1.eks.amazonaws.com
          # Add new clusters here — ApplicationSet auto-creates the Application
  template:
    metadata:
      name: '{{ .cluster }}-retail-store'
      labels:
        cluster: '{{ .cluster }}'
        env: '{{ .env }}'
    spec:
      project: retail-prod            # ArgoCD Project with RBAC policies
      source:
        repoURL: https://github.com/org/gitops-repo
        targetRevision: main
        path: charts/retail-store
        helm:
          valueFiles:
            - values.yaml
            - values-{{ .env }}.yaml
            - values-{{ .cluster }}.yaml
      destination:
        server: '{{ .url }}'
        namespace: retail
      syncPolicy:
        automated:
          prune: false             # NEVER enable prune without manual review in prod
          selfHeal: true           # revert manual kubectl changes
        syncOptions:
          - CreateNamespace=true
          - PrunePropagationPolicy=foreground
          - ApplyOutOfSyncOnly=true  # only apply changed resources, not everything
      ignoreDifferences:            # avoid spurious diffs from AWS auto-injected annotations
        - group: apps
          kind: Deployment
          jsonPointers:
            - /spec/template/metadata/annotations/kubectl.kubernetes.io~1last-applied-configuration
EOF

# Dry run first — see what ArgoCD would create
kubectl apply --dry-run=client -f apps/retail-store-applicationset.yaml

# Apply after review
kubectl apply -f apps/retail-store-applicationset.yaml

# Watch Applications being created (one per cluster entry)
watch "kubectl get applications -n argocd | grep retail-store"
```

### Step 4: Verify sync across all clusters
```bash
# All apps should be Synced + Healthy within 3-5 minutes
argocd app list | grep retail-store

# Check any app that is not healthy
argocd app list | grep -v "Synced.*Healthy" | grep retail-store

# Get detailed status for one specific cluster app
argocd app get cluster-us-east-1-retail-store

# Force sync a specific cluster if auto-sync hasn't fired yet
argocd app sync cluster-us-east-1-retail-store --prune=false

# Diff what ArgoCD will change before syncing
argocd app diff cluster-us-east-1-retail-store
```

### Step 5: Add a new 51st cluster (Day 2 — production procedure)
```bash
# STEP A: Register new cluster in ArgoCD
aws eks update-kubeconfig --region ap-southeast-1 \
  --name cluster-ap-southeast-1 \
  --alias cluster-ap-southeast-1

argocd cluster add cluster-ap-southeast-1 \
  --name cluster-ap-southeast-1 \
  --label env=prod

# Verify connection before proceeding
argocd cluster get cluster-ap-southeast-1
# STATUS must be: Successful

# STEP B: Create cluster-specific values file
cat > charts/retail-store/values-cluster-ap-southeast-1.yaml << 'EOF'
ingress:
  hostname: retail.ap-southeast-1.example.com
  certificateArn: arn:aws:acm:ap-southeast-1:123:certificate/pqr
config:
  region: ap-southeast-1
  rdsEndpoint: catalog-db.ap-southeast-1.rds.amazonaws.com
EOF

# STEP C: Add to ApplicationSet elements list in git
# Edit apps/retail-store-applicationset.yaml — add:
#   - cluster: cluster-ap-southeast-1
#     env: prod
#     url: https://EEEEFFFFF.gr7.ap-southeast-1.eks.amazonaws.com

git add .
git commit -m "feat: add ap-southeast-1 cluster to ApplicationSet [CHG0012346]"
git push origin main

# STEP D: ArgoCD auto-detects the new element and creates the Application
# Wait ~3 minutes for auto-sync, or force it:
argocd appset get retail-store   # check ApplicationSet reconciliation
argocd app get cluster-ap-southeast-1-retail-store   # new app appears
argocd app sync cluster-ap-southeast-1-retail-store --prune=false
```

### Rollback
```bash
# If ApplicationSet causes issues — pause auto-sync first
argocd app set cluster-us-east-1-retail-store --sync-policy none

# Revert git commit
git revert HEAD && git push

# ArgoCD will detect the revert and sync back
# Or force sync to previous revision:
argocd app rollback cluster-us-east-1-retail-store 3  # roll to revision 3
```

---

## SCENARIO 2: Kubernetes Node AMI Patching (Production Way)

### Prerequisites
- EKS cluster managed by Terraform
- AWS CLI configured
- `kubectl` connected to cluster

### Step-by-Step

**Step 1: Find the latest patched AMI release version**
```bash
# Check available EKS optimized AMI versions for your K8s version
aws ssm get-parameter \
  --name /aws/service/eks/optimized-ami/1.30/amazon-linux-2/recommended/release_version \
  --query Parameter.Value \
  --output text
# Output example: 1.30.2-20240703
```

**Step 2: Raise CR (in ServiceNow/Jira) — document:**
- Current AMI version vs new AMI version
- CVEs addressed
- Rollback plan: `terraform apply` with old `ami_release_version` value
- Maintenance window per environment

**Step 3: Update Terraform and patch DEV first**
```bash
# Edit terraform/eks/main.tf
# Change: ami_release_version = "1.30.0-20240501"
# To:     ami_release_version = "1.30.2-20240703"

terraform plan   # review — should show node group update
terraform apply  # triggers rolling node replacement
```

**Step 4: Monitor the rolling replacement**
```bash
# Watch nodes being replaced (in another terminal)
watch kubectl get nodes

# Check node ages — new nodes will have fresh timestamps
kubectl get nodes -o wide

# Check all pods are still running
kubectl get pods -A | grep -v Running | grep -v Completed

# Check for any eviction events
kubectl get events -A --sort-by='.lastTimestamp' | grep -i evict
```

**Step 5: Verify patch applied**
```bash
# Check kubelet version on nodes
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.nodeInfo.kubeletVersion}{"\n"}{end}'

# Check AMI ID on running instances
aws ec2 describe-instances \
  --filters "Name=tag:eks:cluster-name,Values=my-cluster" \
  --query 'Reservations[*].Instances[*].[InstanceId,ImageId]' \
  --output table

# Run Trivy node scan (optional security validation)
trivy image --severity HIGH,CRITICAL $(kubectl get node -o jsonpath='{.items[0].status.nodeInfo.containerRuntimeVersion}')
```

**Step 6: Repeat for staging then prod**
```bash
# Same steps, but in prod use a maintenance window
# Before applying in prod — verify no deployments in progress
kubectl get deployments -A | grep -v "1/1\|2/2\|3/3"
argocd app list | grep -v Synced

---

## SCENARIO 3: EKS Version Upgrade 1.28 → 1.29 (Production Way)

### Phase 0: Research and Planning (1–2 days before)
```bash
# Check current version
aws eks describe-cluster --name prod-cluster --query 'cluster.version' --output text

# Check deprecated API usage in your cluster before upgrading
kubectl get --raw /metrics | grep apiserver_requested_deprecated_apis

# Check all Helm chart API versions for deprecations
helm list -A -o json | jq '.[].name' -r | \
  xargs -I{} bash -c 'helm get manifest {} 2>/dev/null | grep "^apiVersion"' | \
  sort -u

# Check add-on versions compatible with target version
aws eks describe-addon-versions \
  --kubernetes-version 1.29 \
  --query 'addons[*].{name:addonName,latest:addonVersions[0].addonVersion}' \
  --output table
```

### Phase 1: Pre-upgrade checks
```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# ALL nodes must be Ready before starting — no exceptions
kubectl get nodes
[ $(kubectl get nodes | grep -c NotReady) -gt 0 ] && echo "FIX NODES FIRST" && exit 1

# All system pods healthy
kubectl get pods -n kube-system | grep -v "Running\|Completed"
# Output must be empty

# Save current add-on versions for rollback reference
aws eks list-addons --cluster-name prod-cluster --output text
aws eks describe-addon --cluster-name prod-cluster \
  --addon-name aws-ebs-csi-driver --query 'addon.addonVersion' --output text
```

### Phase 2: Upgrade DEV — Control Plane first
```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/dev-cluster

# Update cluster version in Terraform
# terraform/envs/dev/eks.tf: cluster_version = "1.29"

terraform -chdir=terraform/envs/dev plan -out=dev-cp-upgrade.tfplan 2>&1 | tee cp-plan.log
# Verify plan shows ONLY cluster_version change — if it shows more, STOP and review

terraform -chdir=terraform/envs/dev apply dev-cp-upgrade.tfplan
# Control plane upgrade: ~15-20 min. AWS manages it. No downtime to workloads.

# Confirm upgraded
aws eks describe-cluster --name dev-cluster --query 'cluster.version' --output text
kubectl version --short
# Server Version: v1.29.x
```

### Phase 3: Upgrade add-ons to 1.29-compatible versions
```bash
# Update all add-ons in Terraform (terraform/envs/dev/addons.tf)
# Get compatible versions first:
aws eks describe-addon-versions --kubernetes-version 1.29 \
  --query 'addons[*].{name:addonName,latest:addonVersions[0].addonVersion}' \
  --output table

# coredns:     v1.11.1-eksbuild.4
# kube-proxy:  v1.29.3-eksbuild.2
# vpc-cni:     v1.18.1-eksbuild.1
# ebs-csi:     v1.28.0-eksbuild.1

terraform -chdir=terraform/envs/dev plan -out=dev-addons-upgrade.tfplan
terraform -chdir=terraform/envs/dev apply dev-addons-upgrade.tfplan

# Verify all system pods healthy after add-on upgrade
kubectl rollout status daemonset/aws-node -n kube-system
kubectl rollout status daemonset/kube-proxy -n kube-system
kubectl rollout status deployment/coredns -n kube-system
kubectl get pods -n kube-system | grep -v "Running\|Completed"
# Must be empty
```

### Phase 4: Upgrade node groups to 1.29
```bash
# Get 1.29 AMI release version
aws ssm get-parameter \
  --name /aws/service/eks/optimized-ami/1.29/amazon-linux-2023/recommended/release_version \
  --query Parameter.Value --output text

# Update Terraform (terraform/envs/dev/node_groups.tf):
#   kubernetes_version  = "1.29"
#   ami_release_version = "1.29.3-20240703"  ← value from above SSM query

terraform -chdir=terraform/envs/dev plan -out=dev-ng-upgrade.tfplan
terraform -chdir=terraform/envs/dev apply dev-ng-upgrade.tfplan

# Watch rolling node replacement — do not leave unattended
watch -n10 "kubectl get nodes && echo '' && \
  kubectl get pods -A | grep -v 'Running\|Completed' | head -5"
```

### Phase 5: Full validation before moving to staging/prod
```bash
# All nodes on new version
kubectl get nodes -o custom-columns='NAME:.metadata.name,VERSION:.status.nodeInfo.kubeletVersion'

# All deployments healthy
kubectl get deployments -A -o json | \
  jq '.items[] | select(.spec.replicas != .status.readyReplicas) | .metadata.name' -r
# Must return nothing

# Application smoke tests
curl -sf https://dev.retail.example.com/health && echo "PASS" || echo "FAIL"
curl -sf https://dev.retail.example.com/api/catalog && echo "PASS" || echo "FAIL"

# Wait 24 hours minimum, then repeat all phases for staging, then prod
```

### Rollback Note
> EKS control plane version cannot be downgraded. Prevention is the only option.
> Always upgrade DEV first and observe for 24+ hours before moving to staging/prod.

---

## SCENARIO 4: Node Scaling + Troubleshooting (Production Way)

### Increase nodes — validate before and after
```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# WHY are we scaling? Always justify with data first.
kubectl get pods -A | grep Pending
kubectl top nodes
kubectl describe nodes | grep -A 6 "Allocated resources"

# If using Karpenter — check if it's already handling it
kubectl get nodeclaim -A
# If NodeClaims are Launching — Karpenter is already provisioning, wait 60s

# If scaling managed node group (Terraform):
# terraform/envs/prod/node_groups.tf:
#   scaling_config { desired_size = 10 }  # was 5

terraform -chdir=terraform/envs/prod plan -out=scale-prod.tfplan
# Verify ONLY desired_size changed in the plan
grep "desired_size" scale-prod.tfplan || terraform show scale-prod.tfplan | grep desired

terraform -chdir=terraform/envs/prod apply scale-prod.tfplan

# Watch nodes join — new nodes: (not listed) → NotReady → Ready
watch -n10 kubectl get nodes
```

### Full troubleshooting runbook for NotReady node
```bash
NODE=ip-10-0-2-50.ec2.internal

# Step 1: Read the exact failure reason
kubectl describe node $NODE | grep -A 20 "Conditions:"
kubectl describe node $NODE | grep -A 20 "Events:"

# Step 2: Find EC2 instance
INSTANCE_ID=$(aws ec2 describe-instances \
  --filters "Name=private-dns-name,Values=${NODE}" \
  --query 'Reservations[0].Instances[0].InstanceId' --output text)

# Step 3: Check instance system status checks
aws ec2 describe-instance-status --instance-ids $INSTANCE_ID \
  --query 'InstanceStatuses[0].[InstanceStatus.Status,SystemStatus.Status]'
# Both must be: ok

# Step 4: Check bootstrap logs (SSM — no SSH key needed)
aws ssm start-session --target $INSTANCE_ID
  sudo journalctl -u kubelet --no-pager | tail -50
  sudo cat /var/log/cloud-init-output.log | grep -iE "error|fail|denied" | tail -30
exit

# Step 5: Check IAM policies on node role
ROLE=$(aws ec2 describe-instances --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text | \
  awk -F'/' '{print $NF}')
aws iam list-attached-role-policies --role-name $ROLE \
  --query 'AttachedPolicies[*].PolicyName' --output table
# Must include: AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy

# Step 6: Check subnet IPs — exhaustion is common in large clusters
SUBNET=$(aws ec2 describe-instances --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].SubnetId' --output text)
aws ec2 describe-subnets --subnet-ids $SUBNET \
  --query 'Subnets[0].AvailableIpAddressCount'
# If < 10 — subnet near exhaustion. Request CIDR expansion via Change Request.

# Step 7: Security group — node must reach control plane on 443
SG=$(aws ec2 describe-instances --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].SecurityGroups[0].GroupId' --output text)
aws ec2 describe-security-groups --group-ids $SG \
  --query 'SecurityGroups[0].IpPermissionsEgress' | grep -A 3 '"443"'
# Must exist. If not — add outbound 443 rule via Terraform.

# Step 8: Remove unrecoverable node and let ASG/Karpenter replace it
kubectl cordon $NODE
kubectl drain $NODE --ignore-daemonsets --delete-emptydir-data --timeout=120s
kubectl delete node $NODE
aws ec2 terminate-instances --instance-ids $INSTANCE_ID
watch -n10 kubectl get nodes   # new replacement node should appear within 2 minutes
```

---

## SCENARIO 5: HPA + Karpenter — End-to-End Production Test

```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# Step 1: Confirm Metrics Server is working
kubectl top nodes && kubectl top pods -n retail
# If error — Metrics Server not installed or not ready

# Step 2: Confirm PodDisruptionBudgets are in place before load test
kubectl get pdb -n retail
# If missing for any critical service — create before proceeding

# Step 3: Check existing HPA configuration
kubectl get hpa -n retail
kubectl describe hpa retail-ui -n retail
# Verify: minReplicas, maxReplicas, targetCPU threshold

# Step 4: Check Karpenter NodePool limits won't block scale-out
kubectl get nodepool -o yaml | grep -A 3 "limits:"
# Ensure cpu/memory limits are high enough for the test

# Step 5: Run controlled load test (do NOT use production namespace for real load)
kubectl run load-test \
  --image=williamyeh/wrk \
  --restart=Never \
  --namespace=retail \
  -- wrk -t4 -c100 -d180s \
     http://retail-ui.retail.svc.cluster.local:8080/

# Step 6: Observe in separate terminals (open 3 terminals)
# Terminal A — HPA decisions
watch -n5 kubectl get hpa -n retail

# Terminal B — Pod count
watch -n5 "kubectl get pods -n retail -l app=retail-ui | wc -l && \
           kubectl get pods -n retail -l app=retail-ui"

# Terminal C — Node provisioning
watch -n10 kubectl get nodes && \
  kubectl logs -n karpenter -l app.kubernetes.io/name=karpenter \
  --tail=5 | grep -E "launched|provisioned|disrupted"

# Step 7: Stop load and confirm scale-down
kubectl delete pod load-test -n retail
# HPA scale-down: ~5 min after CPU drops (cooldown period)
# Karpenter consolidation: ~30s after nodes become underutilized
watch -n15 "echo '=NODES=' && kubectl get nodes && \
            echo '=HPA=' && kubectl get hpa -n retail"
```

---

## SCENARIO 6: Debug Production 500 Errors — Full Runbook

```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster
# Incident ticket opened — start timer for SLA

# Step 1: Scope — which service is returning 500s?
# Check ALB error rate
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApplicationELB \
  --metric-name HTTPCode_Target_5XX_Count \
  --dimensions Name=LoadBalancer,Value=<alb-suffix> \
  --start-time $(date -u -d '10 minutes ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 60 --statistics Sum

# Step 2: Check pod state — are they crashing?
kubectl get pods -n retail
kubectl get pods -n retail | grep -v "Running\|Completed"

# Step 3: Read logs — find the actual error message
kubectl logs -l app=catalog -n retail --tail=100 --timestamps
kubectl logs -l app=catalog -n retail --previous --tail=50

# Common error patterns:
# "connection refused"            → DB/Redis not reachable
# "dial tcp: i/o timeout"         → Security Group blocking
# "Access denied for user"        → Wrong credentials (rotation?)
# "too many connections"          → RDS max_connections hit
# "certificate has expired"       → TLS cert expired

# Step 4: Test DB connectivity directly from the pod
POD=$(kubectl get pod -n retail -l app=catalog -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it $POD -n retail -- sh
  nc -zv catalog-db.us-east-1.rds.amazonaws.com 3306   # TCP test
  # If this hangs → Security Group is blocking
  # If immediate refuse → DB is down or on wrong port
exit

# Step 5: Compare secret with Secrets Manager (detect stale password)
K8S_PASS=$(kubectl get secret catalog-db-secret -n retail \
  -o jsonpath='{.data.password}' | base64 -d | md5sum)
SM_PASS=$(aws secretsmanager get-secret-value \
  --secret-id prod/catalog/db-password \
  --query SecretString --output text | jq -r .password | md5sum)
echo "K8S: $K8S_PASS"
echo "SM:  $SM_PASS"
# If different → secret was rotated but pod not restarted
# Fix:
kubectl rollout restart deployment/catalog -n retail
kubectl rollout status deployment/catalog -n retail

# Step 6: Check RDS connection count
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name DatabaseConnections \
  --dimensions Name=DBInstanceIdentifier,Value=catalog-mysql-prod \
  --start-time $(date -u -d '30 minutes ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 60 --statistics Maximum

# Step 7: If still down — rollback to previous working version
kubectl rollout history deployment/catalog -n retail
kubectl rollout undo deployment/catalog -n retail
kubectl rollout status deployment/catalog -n retail

# Notify team: "@channel catalog 500s resolved — rolled back to previous version. RCA in progress."
```

---

## SCENARIO 7: Pod Stuck in Pending — Full Diagnosis

```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# Step 1: Find all Pending pods
kubectl get pods -A | grep Pending

# Step 2: Read scheduling failure — Events section is the answer
POD=retail-orders-6d8b9f-xkz4p
NS=retail
kubectl describe pod $POD -n $NS | grep -A 30 "Events:"

# Diagnose by error message:

# "Insufficient memory" or "Insufficient cpu"
kubectl top nodes
kubectl describe nodes | grep -A 8 "Allocated resources"
# Fix A: Reduce resource request in Helm values
# Fix B: Karpenter will provision a larger node — wait 60s
kubectl get nodeclaim -A   # check Karpenter is acting

# "node(s) had untolerated taint"
kubectl describe nodes | grep -i taints
kubectl get pod $POD -n $NS -o yaml | grep -A 5 tolerations
# Fix: add toleration to Helm values or Deployment spec

# "node(s) didn't match Pod's node affinity"
kubectl get pod $POD -n $NS -o yaml | grep -A 10 nodeAffinity
kubectl get nodes --show-labels | grep <required-label>
# Fix: add label to node or update NodePool in Karpenter

# "PVC not found" / PVC in Pending
kubectl get pvc -n $NS
kubectl describe pvc <pvc-name> -n $NS
kubectl get storageclass   # gp3 must exist
kubectl get pods -n kube-system | grep ebs-csi   # CSI driver must be running
kubectl get volumeattachments                     # check for stuck attachments

# "ImagePullBackOff"
kubectl describe pod $POD -n $NS | grep -A 5 "Failed to pull"
# ECR auth — check node IAM role:
aws iam list-attached-role-policies --role-name eks-node-role \
  --query 'AttachedPolicies[*].PolicyName' --output text | grep ECR
# Fix: add AmazonEC2ContainerRegistryReadOnly to node role via Terraform

# Karpenter diagnostic
kubectl get nodeclaim -A
kubectl describe nodeclaim <claim-name>
kubectl logs -n karpenter -l app.kubernetes.io/name=karpenter --tail=30 | grep -i "error\|fail"
```

---

## SCENARIO 8: Zero Downtime Deployment Verification (Production Way)

```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# Step 1: Verify deployment strategy is safe BEFORE releasing
kubectl get deployment retail-orders -n retail \
  -o jsonpath='{.spec.strategy}' | jq .
# Required: RollingUpdate with maxUnavailable=0, maxSurge=1

# Fix if missing:
kubectl patch deployment retail-orders -n retail \
  --type=strategic --patch='
{
  "spec": {
    "strategy": {
      "type": "RollingUpdate",
      "rollingUpdate": {"maxUnavailable": 0, "maxSurge": 1}
    }
  }
}'

# Step 2: Confirm PodDisruptionBudget protects the service
kubectl get pdb retail-orders-pdb -n retail
# If missing — create it before proceeding:
kubectl apply -f - << 'EOF'
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: retail-orders-pdb
  namespace: retail
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: retail-orders
EOF

# Step 3: Start health monitor BEFORE deployment (in a separate terminal)
while true; do
  STATUS=$(curl -sk -o /dev/null -w "%{http_code}" \
    https://retail.example.com/api/orders/health --max-time 3)
  echo "$(date '+%H:%M:%S') HTTP $STATUS"
  [ "$STATUS" != "200" ] && echo "ALERT: Non-200 at $(date)"
  sleep 2
done

# Step 4: Deploy via ArgoCD (GitOps — production standard)
# Commit new image tag to GitOps repo → ArgoCD auto-syncs
# Or if urgent, sync manually:
argocd app diff retail-orders-prod   # preview changes first
argocd app sync retail-orders-prod --prune=false
argocd app wait retail-orders-prod --health --sync --timeout=300

# Step 5: Verify rollout
kubectl rollout status deployment/retail-orders -n retail
watch -n3 kubectl get pods -n retail -l app=retail-orders

# Step 6: Immediate rollback if any health check alerts
kubectl rollout undo deployment/retail-orders -n retail
kubectl rollout status deployment/retail-orders -n retail
# Slack: "@channel Rolling back retail-orders. Investigating."
```

---

## SCENARIO 9: NetworkPolicy Enforcement (Production Way)

```bash
kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster

# Step 1: Confirm CNI supports NetworkPolicy (CRITICAL — verify first)
kubectl get pods -n kube-system | grep -E "calico|cilium|weave"
# If none: NetworkPolicy objects will be accepted but silently IGNORED

# Step 2: Test baseline connectivity BEFORE applying any policy
UI_POD=$(kubectl get pod -n retail -l app=ui -o jsonpath='{.items[0].metadata.name}')
kubectl exec $UI_POD -n retail -- \
  curl -s -o /dev/null -w "UI->API: %{http_code}\n" --max-time 5 \
  http://api-service.retail:8080/health
# Record the response — should be 200 before and after

# Step 3: Verify pod labels are correct
kubectl get pods -n retail --show-labels | grep -E "app=ui|app=api|app=database"

# Step 4: Apply DNS egress allowance FIRST — before any deny-all
# (Forgetting this breaks all name resolution — pods can't find services)
kubectl apply -f - << 'EOF'
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns-egress
  namespace: retail
spec:
  podSelector: {}
  policyTypes: [Egress]
  egress:
    - ports:
        - protocol: UDP
          port: 53
        - protocol: TCP
          port: 53
EOF

# Step 5: Apply specific allow policies BEFORE deny-all
kubectl apply -f - << 'EOF'
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-api-to-database
  namespace: retail
spec:
  podSelector:
    matchLabels:
      app: database
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: api
      ports:
        - protocol: TCP
          port: 5432
EOF

kubectl apply -f - << 'EOF'
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-ui-to-api
  namespace: retail
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: ui
      ports:
        - protocol: TCP
          port: 8080
EOF

# Step 6: NOW apply default deny-all
kubectl apply -f - << 'EOF'
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: retail
spec:
  podSelector: {}
  policyTypes: [Ingress, Egress]
EOF

# Step 7: Verify — expected path works, blocked path fails
# SHOULD work: UI → API
kubectl exec $UI_POD -n retail -- \
  curl -s -o /dev/null -w "UI->API: %{http_code}\n" --max-time 5 \
  http://api-service.retail:8080/health

# SHOULD fail (timeout in 3s): UI → Database
DB_IP=$(kubectl get svc database-service -n retail -o jsonpath='{.spec.clusterIP}')
kubectl exec $UI_POD -n retail -- nc -zv -w 3 $DB_IP 5432
# Expected: nc: connect to ... timed out

# List all policies
kubectl get networkpolicies -n retail -o wide
```

---

## SCENARIO 10: Secret Rotation Incident Response (Production Runbook)

```bash
# Incident: unauthorized access to Secrets Manager detected
# ACTION: Rotate IMMEDIATELY. Investigate in parallel.

# Step 1: Rotate the compromised secret NOW
aws secretsmanager rotate-secret --secret-id prod/catalog/db-password

# If rotation Lambda not configured — force new secret value:
aws secretsmanager put-secret-value \
  --secret-id prod/catalog/db-password \
  --secret-string "{\"password\":\"$(openssl rand -base64 24 | tr -d '=/+')\"}"

# Step 2: Verify rotation completed
aws secretsmanager describe-secret \
  --secret-id prod/catalog/db-password \
  --query 'VersionIdsToStages'

# Step 3: Restart pods to pick up new credentials
kubectl rollout restart deployment/catalog -n retail
kubectl rollout status deployment/catalog -n retail --timeout=120s

# Step 4: Verify application healthy after rotation
curl -sf https://retail.example.com/api/catalog/health && echo "HEALTHY" || echo "BROKEN"

# Step 5: Revoke the developer's IAM access immediately
aws iam put-user-policy \
  --user-name suspected-user \
  --policy-name EmergencyDeny \
  --policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Deny","Action":"*","Resource":"*"}]}'

# Step 6: Audit what they accessed (requires CloudTrail to be enabled)
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=Username,AttributeValue=suspected-user \
  --start-time $(date -u -d '7 days ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --query 'Events[*].{Time:EventTime,Action:EventName,Resource:Resources[0].ResourceName}' \
  --output table

# Step 7: Check GuardDuty for related findings
DETECTOR=$(aws guardduty list-detectors --query 'DetectorIds[0]' --output text)
aws guardduty list-findings --detector-id $DETECTOR \
  --finding-criteria '{"Criterion":{"severity":{"Gte":4}}}' \
  --query 'FindingIds' --output text
```

---

## SCENARIO 11: Full CI/CD Pipeline End-to-End Test

```bash
# Step 1: Create feature branch and make a code change
git checkout -b feat/catalog-v2.0.0
# Make your change
git add . && git commit -m "feat(catalog): v2.0.0 — improved search"
git push origin feat/catalog-v2.0.0

# Step 2: Open PR — CI triggers automatically
# GitHub Actions → ci.yml runs: lint → unit tests → trivy scan
# All must be green before merge

# Step 3: Merge to main — build pipeline triggers
# GitHub Actions → build.yml runs:
#   1. Build Docker image
#   2. Trivy scan (blocks on CRITICAL CVEs)
#   3. Push to ECR: 123456789.dkr.ecr.us-east-1.amazonaws.com/catalog:v2.0.0
#   4. Commit new image.tag to GitOps repo

# Verify image pushed to ECR
aws ecr describe-images \
  --repository-name retail/catalog \
  --query 'sort_by(imageDetails,&imagePushedAt)[-1].[imageTags[0],imagePushedAt]' \
  --output table

# Step 4: Verify GitOps repo updated by CI
cd gitops-repo && git pull origin main
grep "tag:" charts/catalog/values.yaml
# Must show: tag: v2.0.0

# Step 5: ArgoCD deploys to DEV
argocd app get catalog-dev          # should show OutOfSync
argocd app sync catalog-dev --prune=false
argocd app wait catalog-dev --health --sync --timeout=120

kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/dev-cluster
kubectl get pods -n retail -l app=catalog \
  -o custom-columns='POD:.metadata.name,IMAGE:.spec.containers[0].image'
# Must show: .../catalog:v2.0.0

# Step 6: Smoke test on DEV
curl -sf https://dev.retail.example.com/api/catalog/health && echo "DEV PASS"

# Step 7: After QA sign-off + CR approval → promote to prod
argocd app diff catalog-prod        # review what will change
argocd app sync catalog-prod --prune=false
argocd app wait catalog-prod --health --sync --timeout=300

kubectl config use-context arn:aws:eks:us-east-1:123456789:cluster/prod-cluster
kubectl get pods -n retail -l app=catalog \
  -o custom-columns='POD:.metadata.name,IMAGE:.spec.containers[0].image'

curl -sf https://retail.example.com/api/catalog/health && echo "PROD PASS"
# Notify: "@channel catalog v2.0.0 deployed to prod. CR CHG0012345 closed."
```

---

## SCENARIO 12: Daily Production Health Check Script

```bash
#!/bin/bash
# daily-health-check.sh — run every morning (cron: 0 8 * * * /opt/scripts/daily-health-check.sh)
# Set your values:
CLUSTER="arn:aws:eks:us-east-1:123456789:cluster/prod-cluster"
NS="retail"
APP_URL="https://retail.example.com"
SLACK_WEBHOOK="https://hooks.slack.com/services/xxx/yyy/zzz"

ISSUES=""
kubectl config use-context $CLUSTER 2>/dev/null

send_slack() {
  curl -s -X POST "$SLACK_WEBHOOK" \
    -H 'Content-type: application/json' \
    --data "{\"text\":\"$1\"}" > /dev/null
}

echo "=============================="
echo " HEALTH CHECK — $(date '+%Y-%m-%d %H:%M')"
echo "=============================="

# Node health
echo ""
echo "=== NODES ==="
kubectl get nodes
NOT_READY=$(kubectl get nodes --no-headers | grep -vc " Ready ")
[ "$NOT_READY" -gt 0 ] && ISSUES="$ISSUES\n⚠️ $NOT_READY node(s) not Ready"

# Unhealthy pods
echo ""
echo "=== UNHEALTHY PODS ==="
BAD=$(kubectl get pods -n $NS --no-headers | grep -v "Running\|Completed\|Succeeded")
[ -n "$BAD" ] && echo "$BAD" && ISSUES="$ISSUES\n⚠️ Unhealthy pods:\n$BAD" || echo "All healthy"

# Recent warnings
echo ""
echo "=== WARNING EVENTS (last 10) ==="
kubectl get events -n $NS --field-selector type=Warning \
  --sort-by='.lastTimestamp' --no-headers | tail -10

# ArgoCD
echo ""
echo "=== ARGOCD STATUS ==="
UNSYNCED=$(argocd app list 2>/dev/null | grep -v "Synced.*Healthy" | grep -vc NAME)
argocd app list 2>/dev/null | grep -v "Synced.*Healthy" | grep -v NAME || echo "All Synced+Healthy"
[ "$UNSYNCED" -gt 0 ] && ISSUES="$ISSUES\n⚠️ $UNSYNCED app(s) not Synced/Healthy"

# HPA at max
echo ""
echo "=== HPA ==="
kubectl get hpa -n $NS
AT_MAX=$(kubectl get hpa -n $NS -o json 2>/dev/null | \
  jq -r '.items[] | select(.status.currentReplicas == .spec.maxReplicas) | .metadata.name')
[ -n "$AT_MAX" ] && ISSUES="$ISSUES\n⚠️ HPA at MAX replicas: $AT_MAX"

# Pending PVCs
echo ""
echo "=== PVCS ==="
PENDING_PVC=$(kubectl get pvc -n $NS --no-headers | grep -v Bound)
[ -n "$PENDING_PVC" ] && echo "$PENDING_PVC" && ISSUES="$ISSUES\n⚠️ Pending PVCs detected" || echo "All bound"

# Karpenter
echo ""
echo "=== KARPENTER NODECLAIMS ==="
kubectl get nodeclaim -A 2>/dev/null || echo "Karpenter not available"

# Resource usage
echo ""
echo "=== NODE RESOURCE USAGE ==="
kubectl top nodes 2>/dev/null || echo "Metrics server unavailable"

echo ""
echo "=== TOP 5 CPU PODS ==="
kubectl top pods -n $NS --sort-by=cpu 2>/dev/null | head -6

# Application health endpoint
echo ""
echo "=== APP HEALTH ==="
HTTP=$(curl -sk -o /dev/null -w "%{http_code}" "$APP_URL/health" --max-time 10)
echo "Health endpoint: HTTP $HTTP"
[ "$HTTP" != "200" ] && ISSUES="$ISSUES\n🚨 App health endpoint HTTP $HTTP (not 200)"

# Summary and Slack notification
echo ""
echo "=============================="
if [ -n "$ISSUES" ]; then
  echo "ISSUES FOUND:"
  echo -e "$ISSUES"
  send_slack "🔴 *Daily Health Check FAILED — prod-cluster $(date '+%Y-%m-%d')*\`\`\`$ISSUES\`\`\`"
else
  echo "✅ All checks passed — cluster healthy"
  send_slack "✅ Daily health check PASSED — prod-cluster $(date '+%Y-%m-%d %H:%M')"
fi
echo "=============================="
```
