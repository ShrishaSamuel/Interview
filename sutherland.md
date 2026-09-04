# Sutherland AWS DevOps Interview Prep

## 1. What This Role Is About

This role is looking for an AWS DevOps engineer who can:

- Build AWS infrastructure.
- Run container applications.
- Secure access and data.
- Monitor production systems.
- Process messages reliably.
- Support AWS AI services.
- Automate infrastructure with Terraform.
- Work through client change-control processes.
- Design systems that continue working during failures.

Your strongest resume matches are AWS, Terraform, EKS, Docker, ECR, IAM, Secrets Manager, Bedrock, CloudWatch, Prometheus, Grafana, incident support, and client projects.

Your main bridge topics are ECS/Fargate, SNS/SQS, DynamoDB, DLQs, idempotency, regulated-client controls, and disaster recovery.

## 2. Very Basic AWS Understanding

### AWS account

An AWS account is the main boundary where resources, users, permissions, billing, and security controls are managed.

In a client-owned account, engineers usually need approved access and must follow the client's change process.

### Region and Availability Zone

- A **Region** is a geographical AWS location, such as Mumbai or Frankfurt.
- An **Availability Zone**, or AZ, is an independent data-center area inside a Region.
- For high availability, production workloads are normally spread across more than one AZ.

### VPC

A VPC is a private network in AWS.

A simple VPC design contains:

- Public subnets for internet-facing resources such as load balancers.
- Private subnets for applications and databases.
- Route tables to control where traffic goes.
- Internet Gateway for public internet access.
- NAT Gateway for private resources that need outbound internet access.
- Security Groups to control network traffic.
- Network ACLs for subnet-level traffic control.

### IAM

IAM controls who or what can access AWS resources.

- A **user** is normally a human identity.
- A **role** is temporary access used by people, applications, or AWS services.
- A **policy** defines allowed or denied actions.
- **Least privilege** means granting only the access that is required.

Never put AWS access keys or passwords directly in source code or container images.

### S3

S3 is object storage. It is used for files, backups, logs, documents, and artifacts.

Important controls include:

- Block public access.
- Bucket policies and IAM policies.
- Encryption.
- Versioning.
- Lifecycle rules.
- Access logging or CloudTrail data events when required.

### DynamoDB

DynamoDB is a managed NoSQL database.

The application chooses a partition key to distribute data. DynamoDB is useful when the application needs scalable, low-latency key-value or document access.

Important topics include:

- Partition key.
- Sort key.
- Read and write capacity.
- On-demand or provisioned capacity.
- Encryption.
- Point-in-time recovery.
- Global tables for multi-Region needs.

### ECR

ECR is a private AWS container registry. Docker images are pushed to ECR and then pulled by ECS, Fargate, or EKS.

### ECS and Fargate

ECS runs containers using AWS concepts:

- A **task definition** describes the container image, CPU, memory, ports, environment variables, and IAM roles.
- A **task** is a running copy of that task definition.
- A **service** keeps the required number of tasks running.
- A **cluster** groups ECS services and tasks.
- **Fargate** runs the containers without the team managing EC2 worker servers.

EKS uses Kubernetes objects such as pods and deployments. ECS/Fargate uses task definitions and services. The container, networking, IAM, logging, and monitoring principles are similar.

### SNS and SQS

- **SNS** is a publish/subscribe notification service. One message can be sent to multiple subscribers.
- **SQS** is a queue. A consumer reads and processes messages from the queue.
- A **DLQ** stores messages that failed after the configured retry limit.

Common pattern:

```text
Application -> SNS topic -> SQS queue -> Worker -> Database or external service
                                      -> DLQ after repeated failure
```

### CloudWatch, Prometheus, and Grafana

- **CloudWatch** collects AWS metrics, logs, events, and alarms.
- **Prometheus** collects time-series metrics, especially from applications and Kubernetes.
- **Grafana** displays dashboards and can send alerts.
- **AMP** is managed Prometheus.
- **AMG** is managed Grafana.

The three basic observability signals are:

- **Metrics:** numerical measurements such as CPU, latency, and error rate.
- **Logs:** detailed event records.
- **Traces:** a request's journey through multiple services.

### Bedrock and Textract

- **Amazon Bedrock** provides managed access to foundation models through AWS APIs.
- **Amazon Textract** extracts text, forms, and table data from documents.

An AI workload still needs normal production controls:

- IAM permissions.
- Network connectivity.
- VPC endpoints where required.
- Service quotas.
- Monitoring and logging.
- Cost controls.
- Data protection.
- Security approval.

Your resume directly supports Bedrock, Amazon Nova Pro, CrewAI, Docker, and AWS deployment experience.

### Terraform

Terraform is Infrastructure as Code. Instead of manually creating resources in the console, the infrastructure is described in code.

Basic Terraform workflow:

```text
terraform init -> terraform plan -> review -> terraform apply
```

Important practices:

- Store code in Git.
- Review changes through pull requests.
- Run plan before apply.
- Keep state secure.
- Use reusable modules.
- Separate environments carefully.
- Avoid uncontrolled manual changes.

### High availability and disaster recovery

- **High availability** keeps a service running when one component fails.
- **Disaster recovery** restores service after a larger failure.

Basic controls include multiple AZs, load balancers, autoscaling, backups, health checks, monitoring, documented recovery steps, and regular testing.

Two important terms:

- **RTO:** how quickly service must be restored.
- **RPO:** how much data loss is acceptable, measured in time.

## 3. Your Basic Introduction

> I am an AWS DevOps Engineer with more than four years of experience. I work with Terraform, AWS, Docker, Kubernetes, EKS, CI/CD, and monitoring. In my current role at Capgemini for the Renault Group client, I provision AWS infrastructure using Terraform, manage EKS workloads, containerize applications, support Amazon Bedrock-based AI work, and implement monitoring with CloudWatch, Prometheus, AMP, and Grafana. I also handle production troubleshooting and RCA. My strongest experience is with EKS, but the same container, networking, IAM, security, and monitoring concepts apply to ECS and Fargate.

## 4. How to Answer Honestly

Use this structure:

1. Explain the concept simply.
2. Give the experience from your resume.
3. State the difference if the technology is not direct experience.
4. Explain how you would approach it.

Example for ECS/Fargate:

> My direct production container experience is with EKS, Docker, and ECR. ECS and Fargate use different orchestration objects, such as task definitions and services, but the core concepts are the same: container images, IAM roles, networking, logging, scaling, and health checks. I would apply my EKS experience while learning the ECS-specific configuration.

Do not claim direct SNS, SQS, DynamoDB, or Fargate production experience unless you have actually used them.

# 5. Interview Questions and Basic Answer Points

## A. General and Resume Questions

### 1. Tell me about yourself.

Mention four years of AWS DevOps experience, Terraform, EKS, Docker, CI/CD, monitoring, Bedrock, and production troubleshooting. End by connecting your experience to this role.

### 2. Explain your current project.

Explain the Renault Group client, the AWS infrastructure, EKS workloads, Docker/ECR, Terraform, CI/CD, Bedrock AI work, monitoring, and incident support.

### 3. What are your day-to-day responsibilities?

Mention infrastructure provisioning, deployment support, Kubernetes operations, monitoring, troubleshooting, security, documentation, and coordination with application or security teams.

### 4. What was the most difficult production issue you solved?

Use a real incident. Explain the symptom, investigation, root cause, fix, validation, and preventive action. Do not invent metrics or details.

### 5. Describe a time you made a mistake.

Choose a controlled technical mistake. Explain how you detected it, restored service, communicated it, and added a check to prevent recurrence.

### 6. Why are you interested in Sutherland?

Say that the role combines AWS infrastructure, DevOps automation, AI workload support, observability, security, and client-facing production operations.

### 7. Why should we hire you?

Emphasize hands-on AWS, Terraform, containers, EKS, Bedrock, monitoring, troubleshooting, and your ability to work within controlled client processes.

### 8. What is your strongest technical skill?

A good answer is AWS infrastructure automation with Terraform and production container operations on EKS.

### 9. What is a skill you are still developing?

Use ECS/Fargate or AWS messaging if accurate. Explain that your direct experience is EKS but you understand the underlying container and reliability concepts.

## B. AWS and VPC Questions

### 10. What is a VPC?

A logically isolated network in AWS containing subnets, routes, gateways, and security controls.

### 11. What is the difference between a public and private subnet?

A public subnet has a route to an Internet Gateway. A private subnet does not directly accept internet traffic. Private resources can use a NAT Gateway for outbound access.

### 12. Where would you place a load balancer?

An internet-facing load balancer normally uses public subnets. Application tasks or pods can run in private subnets.

### 13. What is a Security Group?

A stateful virtual firewall attached to resources such as EC2 instances, load balancers, and some network interfaces.

### 14. What is the difference between a Security Group and a Network ACL?

Security Groups apply to resources and are stateful. Network ACLs apply to subnets and are stateless, so inbound and outbound rules must both be considered.

### 15. What is a NAT Gateway used for?

It allows resources in private subnets to make outbound connections without allowing unsolicited inbound internet connections.

### 16. How would you design a production VPC?

Use multiple AZs, public subnets for load balancers, private subnets for applications and databases, controlled routes, restricted security groups, VPC endpoints where useful, and centralized logging.

### 17. How would you troubleshoot an application that cannot reach a database?

Check DNS, route tables, security groups, NACLs, database status, port configuration, subnet placement, network logs, and application credentials.

### 18. What is a VPC endpoint?

It provides private connectivity from a VPC to supported AWS services without sending traffic through the public internet. Gateway endpoints are commonly used for S3 and DynamoDB; interface endpoints use private network interfaces.

### 19. How do you secure AWS infrastructure?

Use least-privilege IAM, private subnets, restricted security groups, encryption, Secrets Manager, logging, vulnerability scanning, patching, and controlled changes.

## C. IAM and Security Questions

### 20. What is IAM?

AWS Identity and Access Management controls authentication and authorization.

### 21. What is least privilege?

Giving an identity only the permissions required for its specific task, for the shortest practical time.

### 22. What is the difference between an IAM user and an IAM role?

A user is a long-lived identity. A role provides temporary credentials and is preferred for AWS services, applications, and federated access.

### 23. What is an IAM policy?

A JSON document that allows or denies actions on specified resources, optionally under conditions.

### 24. How would you give an ECS task access to S3?

Create a task role with only the required S3 actions and bucket resources, then attach that role to the ECS task definition. Do not put access keys in the container.

### 25. What is the difference between a task role and an execution role?

The execution role allows ECS/Fargate to perform actions such as pulling an image from ECR and sending logs. The task role gives the application inside the container permission to call AWS services.

### 26. How do you manage secrets?

Store secrets in AWS Secrets Manager or Parameter Store, grant access through IAM roles, inject them securely, rotate them where possible, and never commit them to Git.

### 27. How do you audit AWS activity?

Use CloudTrail for API activity, CloudWatch for logs and alarms, S3 access logging or CloudTrail data events when needed, and centralized retention with access controls.

### 28. How would you handle access in a client-owned account?

Use approved identities, least privilege, ticketed access, MFA or federation, change approvals, security review, audit logging, and documented rollback steps.

## D. ECS and Fargate Questions

### 29. What is ECS?

A managed AWS service for running and orchestrating containers.

### 30. What is Fargate?

A serverless compute option for ECS. AWS manages the underlying servers, while the team defines the container task requirements.

### 31. Explain an ECS task definition.

It defines the image, CPU, memory, ports, environment variables, logging, networking mode, and IAM roles for a task.

### 32. What is an ECS service?

It maintains the desired number of running tasks and replaces unhealthy tasks.

### 33. How would you deploy a Docker application to Fargate?

Build the image, scan it, push it to ECR, create a task definition, configure IAM and networking, create an ECS service, attach a load balancer if needed, and configure logging and alarms.

### 34. How does ECS service scaling work?

The service can maintain a desired task count and use target tracking or step scaling based on CPU, memory, request count, or custom metrics.

### 35. How would you troubleshoot a Fargate task that stops immediately?

Check ECS stopped-task reason, container exit code, application logs, image availability, task role, execution role, environment variables, CPU/memory, health checks, and network access.

### 36. How would you troubleshoot an ECS task that cannot pull an image?

Check the execution role, ECR permissions, image name and tag, network route, NAT or VPC endpoints, and whether the image exists in the expected Region.

### 37. How would you perform a zero-downtime ECS deployment?

Use a load balancer, health checks, rolling deployment settings, minimum healthy percentage, maximum percentage, and rollback on failed health checks. Blue/green deployment can reduce release risk further.

### 38. EKS versus ECS: what is the difference?

EKS runs Kubernetes and provides Kubernetes flexibility and portability. ECS is AWS-native and uses ECS task and service concepts. Fargate can be used with ECS and also with EKS for some workloads.

### 39. What are the advantages and disadvantages of Fargate?

Advantages include no server management and simpler scaling. Considerations include cost at sustained high utilization, task startup time, networking requirements, and less host-level control.

## E. SNS, SQS, and Reliable Processing Questions

### 40. What is SNS?

A managed publish/subscribe service used to fan out notifications to multiple subscribers.

### 41. What is SQS?

A managed queue that decouples producers and consumers and stores messages until a consumer processes them.

### 42. Standard queue versus FIFO queue?

Standard queues provide high throughput with at-least-once delivery and possible reordering. FIFO queues support ordering and deduplication features but have different throughput limits.

### 43. What is a visibility timeout?

After a consumer receives a message, the message is hidden from other consumers for the visibility timeout. The consumer should delete it after successful processing.

### 44. What happens if a consumer crashes during processing?

The message becomes visible again after the visibility timeout and can be retried. The processing code must therefore be idempotent.

### 45. What is a DLQ?

A dead-letter queue stores messages that fail repeatedly, allowing the main queue to continue processing and allowing engineers to investigate failed messages.

### 46. What is idempotency?

An operation is idempotent when repeating it produces the same final result as performing it once.

Example: before creating an order, check a unique transaction ID. If the ID was already processed, do not create a second order.

### 47. Does SQS guarantee exactly-once processing?

Standard SQS provides at-least-once delivery, so duplicates are possible. FIFO provides deduplication features, but application-level idempotency is still important. In practice, build consumers to safely handle duplicates.

### 48. How would you design reliable message processing?

Use retries, a suitable visibility timeout, DLQ, idempotency key, transaction status, structured logs, queue-depth alarms, and a replay or reprocessing procedure.

### 49. How do you choose a visibility timeout?

It should be longer than the normal processing time, with room for variation. If processing can take longer, extend the timeout or split the work. Do not set it blindly.

### 50. What would you monitor for SQS?

Approximate age of the oldest message, visible messages, messages in flight, sent messages, deleted messages, failed processing, DLQ depth, and consumer errors.

### 51. How can SNS and SQS work together?

SNS publishes an event to one or more SQS subscriptions. Each subscriber gets its own queue and can process messages independently.

## F. DynamoDB Questions

### 52. What is DynamoDB?

A fully managed NoSQL database designed for scalable, low-latency key-value and document access.

### 53. What is a partition key?

The key used to distribute items across DynamoDB partitions. A good key distributes traffic evenly.

### 54. What is a sort key?

An optional second key used to organize related items and support range queries within a partition key.

### 55. On-demand versus provisioned capacity?

On-demand automatically handles changing traffic and charges based on usage. Provisioned capacity sets read and write capacity and can be more predictable for stable workloads.

### 56. How do you secure DynamoDB?

Use IAM least privilege, encryption, private connectivity where appropriate, backups, point-in-time recovery, logging, and careful table policies.

### 57. How would DynamoDB support an idempotent consumer?

Store the message or transaction ID as a unique record, use a conditional write, and skip processing if that ID already exists.

### 58. How would you troubleshoot DynamoDB throttling?

Check consumed versus provisioned capacity, hot partition keys, request patterns, retry behavior, table mode, indexes, and whether adaptive capacity or redesign is needed.

## G. Bedrock, Textract, and AI Workload Questions

### 59. What is Amazon Bedrock?

A managed AWS service that lets applications use foundation models through APIs without managing model infrastructure directly.

### 60. What experience do you have with Bedrock?

Your resume states that you migrated an AI chatbot PoC from a Mistral-based solution to Amazon Bedrock, integrated Amazon Nova Pro with CrewAI, and containerized the application for AWS deployment.

### 61. What is Amazon Textract?

A managed service that extracts text, forms, and tables from scanned documents and images.

### 62. How would you give an application access to Bedrock?

Use an IAM role with only the required Bedrock actions and model resources, configure the correct Region and model access, and avoid hardcoded credentials.

### 63. What network issues can affect Bedrock access?

Private subnet routing, NAT access, VPC endpoints where supported, DNS, security groups, endpoint policies, Region selection, and client account restrictions.

### 64. What are quotas in AI services?

Quotas limit requests, tokens, throughput, or concurrent operations. Monitor usage, handle throttling with backoff, request increases early, and design queue-based processing when appropriate.

### 65. How would you monitor an AI workload?

Track request count, latency, errors, throttling, token usage, model response failures, application logs, cost, and business-level success metrics. Do not log sensitive prompts or responses without approval.

### 66. How would you protect sensitive data sent to an AI service?

Minimize data, mask sensitive fields, restrict access, encrypt data, control logging, define retention, use approved models and Regions, and obtain security approval.

### 67. How would you control AI cost?

Set quotas and budgets, monitor token usage, choose suitable models, cache safe results, limit unnecessary prompts, apply timeouts, and alert on abnormal usage.

## H. Terraform and IaC Questions

### 68. Why use Terraform?

It makes infrastructure repeatable, reviewable, consistent, and easier to recreate or change safely.

### 69. Explain the Terraform workflow.

Initialize providers and modules, format and validate code, create a plan, review the plan, apply it through an approved process, and verify the result.

### 70. What is Terraform state?

State records the relationship between Terraform configuration and real infrastructure. It must be protected, backed up, and shared safely for team use.

### 71. How do you manage Terraform state in a team?

Use a remote backend with encryption, access control, versioning, locking where supported, and separate state for appropriate environments or components.

### 72. What is a Terraform module?

A reusable package of Terraform resources and variables that standardizes infrastructure patterns.

### 73. How do you prevent dangerous Terraform changes?

Use pull requests, `terraform plan`, peer review, policy checks, separate environments, approvals, backups, and documented rollback procedures.

### 74. What would you do if someone changed infrastructure manually?

Run a plan to identify drift, understand whether the change was approved, either update the code or revert the manual change through change control, and improve process controls if needed.

### 75. How do you handle secrets in Terraform?

Avoid putting secret values in code or state where possible, use Secrets Manager or a secure secret pipeline, restrict state access, and mark sensitive variables appropriately.

## I. Monitoring, Observability, and Incidents

### 76. What is observability?

The ability to understand a system's internal state from its outputs: metrics, logs, and traces.

### 77. What would you monitor for a production service?

Availability, request rate, latency, error rate, saturation, CPU, memory, restarts, deployment health, dependency health, queue depth, and business transactions.

### 78. What makes a good alert?

It should indicate meaningful user or system impact, have a clear threshold, avoid excessive noise, identify the owner, and link to a runbook or action.

### 79. What is the difference between a metric, log, and trace?

A metric shows a measurement over time. A log records an event. A trace follows one request across services.

### 80. How would you investigate high latency?

Confirm the alert, check latency by endpoint and time, compare error rate and traffic, inspect dashboards and logs, trace requests, check dependencies and resource saturation, then mitigate and perform RCA.

### 81. How would you investigate repeated pod or container restarts?

Check events, exit codes, logs, health probes, resource limits, OOM kills, configuration, dependency connectivity, and recent deployments.

### 82. How do you approach a production incident?

Detect, acknowledge, assess impact, stabilize or roll back, communicate, resolve, validate, document, and complete a blameless RCA with preventive actions.

### 83. What is RCA?

Root Cause Analysis identifies why an incident happened, why it was not prevented or detected earlier, and what actions will reduce recurrence.

### 84. What is the difference between an alert and an event?

An event is something that happened. An alert is a notification generated when a condition requires attention.

## J. Client Governance and Regulated Environments

### 85. What is change control?

A process for reviewing, approving, scheduling, implementing, and documenting infrastructure or application changes.

### 86. How do you work under client change control?

Create a change request, document risk and implementation steps, obtain approvals, schedule the change, execute it, validate it, and record the outcome.

### 87. What does least privilege mean in a regulated environment?

Each person, service, and pipeline receives only the access required for its responsibilities. Access is reviewed and removed when no longer needed.

### 88. What evidence might an auditor request?

Access records, IAM policies, CloudTrail logs, change tickets, approvals, deployment records, Terraform plans, monitoring alerts, incident reports, backup evidence, and security scan results.

### 89. What is data residency?

The requirement that data is stored and processed only in approved geographical locations or AWS Regions.

### 90. How would you ensure data residency?

Use approved Regions, restrict resource creation with policies, verify service and backup locations, control replication, and document the data flow.

### 91. How would you coordinate with security or IT teams?

Share the design early, provide required permissions and data-flow details, address findings, obtain formal sign-off, and keep the approval and implementation evidence documented.

### 92. How do you separate duties?

Separate development, approval, deployment, and audit responsibilities where required. Use pull requests and controlled pipelines instead of unrestricted production access.

## K. High Availability, Resilience, and DR

### 93. What does highly available mean?

The system remains usable despite failure of an individual component or Availability Zone.

### 94. How would you make an application highly available?

Use multiple AZs, load balancing, multiple application tasks or pods, health checks, autoscaling, resilient dependencies, backups, and tested recovery procedures.

### 95. What is the difference between backup and disaster recovery?

A backup is a copy of data. Disaster recovery includes the complete people, process, infrastructure, and steps required to restore service.

### 96. What are RTO and RPO?

RTO is the maximum acceptable recovery time. RPO is the maximum acceptable data loss measured by time.

### 97. How would you design DR for an AWS application?

Start with business requirements, identify dependencies, choose backup or replication, define RTO/RPO, automate recovery where possible, document runbooks, and test regularly.

### 98. What happens if one AZ fails?

Traffic should move to healthy resources in other AZs. This requires multi-AZ deployment, healthy load-balancer targets, working data replication or failover, and enough capacity elsewhere.

### 99. How do you test resilience?

Run controlled failure tests such as stopping tasks, testing dependency failures, restoring backups, validating failover, measuring recovery time, and documenting gaps.

## L. Practical Scenario Questions

### 100. A production ECS service is returning 5xx errors. What do you do?

Check impact and recent changes, review load-balancer and application metrics, inspect task logs and health checks, verify dependencies and IAM, roll back if appropriate, communicate, and perform RCA.

### 101. An SQS queue is growing continuously. What do you check?

Consumer health, processing time, concurrency, visibility timeout, downstream dependency failures, permissions, message format, throttling, and DLQ activity.

### 102. Bedrock requests are being throttled. What do you do?

Check quota usage and Region, add controlled retries with exponential backoff, reduce unnecessary requests, queue work if appropriate, request a quota increase, and alert on throttling.

### 103. Terraform plan wants to destroy a production database. What do you do?

Stop and do not apply. Review the plan, state, configuration, provider changes, and lifecycle settings. Confirm the intended change with the owner, take backups, obtain approval, and use a safer migration approach.

### 104. A secret was accidentally committed to Git. What do you do?

Treat it as exposed, revoke or rotate it immediately, investigate access, remove it from active history according to policy, update the application to use a secret store, and document the incident.

### 105. A client asks you to make an urgent production change without approval. What do you do?

Follow the emergency-change process. Confirm authorization, document risk and rollback, involve the required IT or security contacts, implement the smallest safe change, validate, and record everything.

### 106. An application in a private subnet cannot reach Bedrock or S3. What do you check?

Check the Region, IAM role, DNS, route tables, NAT or VPC endpoints, endpoint policy, security groups, and whether the client network policy allows the connection.

### 107. Duplicate messages created duplicate orders. How do you fix it?

Add an idempotency key, store processing status, use a conditional database write, handle retries safely, inspect the queue and consumer behavior, and reconcile already duplicated records.

### 108. A deployment passed CI but failed in production. What do you investigate?

Compare environments, image and configuration versions, IAM, secrets, network access, resource limits, health checks, deployment logs, and dependency versions. Add a test or gate for the missing condition.

## 6. Questions You Can Ask the Interviewer

- Is the container platform primarily ECS/Fargate, EKS, or both?
- Which AWS AI services are currently in production?
- How are Terraform state and approvals managed?
- What monitoring and incident-management tools are used?
- What are the main production SLOs and DR requirements?
- How does the client handle AWS access and emergency changes?
- Which messaging patterns use SNS, SQS, or DLQs?
- What would success look like in the first three months?

## 7. Final Preparation Order

Study in this order:

1. VPC, subnets, security groups, IAM, and S3.
2. ECS, Fargate, ECR, task definitions, and services.
3. SNS, SQS, DLQs, retries, and idempotency.
4. Terraform workflow and state.
5. CloudWatch, Prometheus, Grafana, logs, and alerts.
6. Bedrock access, quotas, networking, and monitoring.
7. Client change control, audit logging, and least privilege.
8. Multi-AZ design, RTO, RPO, backups, and DR.
9. Your real project examples and incident stories.

Keep every answer connected to something you genuinely did, supported, learned, or would do. That is stronger than claiming tools that are not on your resume.

# 8. First-Level Interview Questions and Answers

These answers are written in a natural speaking style. Do not memorise every word. Remember the meaning and connect it to your real project experience.

## 1. What is Terraform import?

**Answer:**

> Terraform import is used to bring an existing infrastructure resource under Terraform management. For example, if an S3 bucket or EC2 instance was created manually, I can import its ID into the Terraform state. Import does not automatically create the complete Terraform configuration, so I must write the matching resource block in the `.tf` file and run `terraform plan` to verify that Terraform does not propose unwanted changes.

**Basic command:**

```bash
terraform import aws_s3_bucket.example my-existing-bucket
```

**Follow-up: Does import create the `.tf` code?**

> Traditionally, import mainly updates the state. I still need to create and verify the configuration. Newer Terraform versions also support import blocks and configuration generation, but I would still review the generated configuration carefully before applying it.

## 2. What are Terraform modules?

**Answer:**

> A Terraform module is a reusable collection of Terraform resources. Instead of writing the same VPC, security group, or ECS configuration repeatedly, I can create a module with variables and outputs and reuse it for different environments. Modules improve standardisation, reduce duplication, and make infrastructure easier to maintain.

**Example:**

> I could create a VPC module with variables for CIDR ranges, availability zones, and subnet counts. Development and production can call the same module with different values.

**Follow-up: What are variables and outputs?**

> Variables are inputs passed into a module. Outputs expose useful values such as a VPC ID, subnet IDs, or load-balancer DNS name to the calling configuration.

## 3. What is Terraform state?

**Answer:**

> Terraform state is a file that records the resources Terraform manages and maps the configuration to real infrastructure. During a plan, Terraform compares the `.tf` configuration, the state, and the actual provider resources to calculate changes.

> In a team, I would store state remotely in a secured backend, enable encryption and versioning, restrict access, and use state locking where supported. I would not commit `terraform.tfstate` to Git because it can contain sensitive information and resource details.

**Follow-up: What happens if state is lost?**

> Terraform loses its mapping to the existing resources and may try to recreate them. I would protect state with a remote backend, versioning, backups, and controlled access. If necessary, resources can be re-imported, but prevention is much safer.

## 4. What is a Terraform `.tf` file?

**Answer:**

> A `.tf` file is a Terraform configuration file written in HashiCorp Configuration Language, or HCL. It can contain providers, resources, variables, outputs, data sources, locals, modules, and backend configuration. Terraform reads all `.tf` files in the working directory together, so teams often separate them into files such as `provider.tf`, `variables.tf`, `main.tf`, and `outputs.tf` for organisation.

## 5. What is Kubernetes Ingress?

**Answer:**

> Kubernetes Ingress is a set of rules for routing external HTTP or HTTPS traffic to Kubernetes Services. It can route based on host names or URL paths and can also define TLS termination. In Amazon EKS, an Ingress controller such as the AWS Load Balancer Controller watches the Ingress resource and creates or configures an Application Load Balancer.

**Traffic flow:**

```text
Client -> DNS -> Application Load Balancer -> Ingress rules -> Kubernetes Service -> Pods
```

**Follow-up: What is the difference between Ingress and Service?**

> A Service provides stable internal access to a group of pods. Ingress defines external HTTP or HTTPS routing to one or more Services.

## 6. Mention one production issue and how you resolved it.

Use a real example and do not invent technical details. A safe answer based on your resume is:

> One production issue I handled involved a deployment or CI/CD failure that prevented a reliable release. I first confirmed the impact and checked the GitLab pipeline logs, deployment output, Kubernetes events, pod status, and application logs. I compared the failing configuration with the last successful version and identified the configuration or deployment error. I corrected the issue through the repository and approved pipeline process, redeployed, and verified the pod health, service response, and application metrics. Afterward, I documented the RCA and added a validation step so the same type of issue would be detected earlier.

**If they ask for more detail, use this structure:**

1. What was the user or business impact?
2. How did you detect it?
3. What logs or metrics did you check?
4. What was the root cause?
5. What was the immediate fix?
6. What preventive action did you take?

## 7. Which AWS services have you used hands-on?

**Answer based on your resume:**

> I have hands-on experience with EC2, VPC, S3, EBS, ALB, Auto Scaling Groups, EKS, ECR, RDS, Route 53, IAM, Security Groups, CloudWatch, AWS Secrets Manager, Amazon Managed Service for Prometheus, and Amazon Managed Grafana. I have also worked with Amazon Bedrock and Amazon Nova Pro for an AI chatbot proof of concept. I provisioned and managed much of the infrastructure using Terraform.

**Be careful:**

> My strongest direct container experience is EKS. I understand ECS and Fargate concepts and can transfer my Docker, ECR, IAM, networking, monitoring, and deployment experience to them.

## 8. Explain a private subnet and why it is used.

**Answer:**

> A private subnet is a subnet whose route table does not provide a direct route from the internet through an Internet Gateway. We use private subnets for application servers, Kubernetes worker resources, containers, and databases because they should not be directly reachable from the public internet. If they need outbound access for updates or external APIs, they can use a NAT Gateway or approved VPC endpoints. Public load balancers receive user traffic and forward only the required traffic to private resources.

**Follow-up: Are private subnet resources completely isolated?**

> No. They can still communicate through configured routes, security groups, NAT, VPC endpoints, or private connectivity. Private means no direct public internet route, not that all traffic is automatically blocked.

## 9. Which CI/CD tools have you used?

**Answer based on your resume:**

> I have used GitLab CI/CD for build, testing, security scanning, and deployment workflows. I have also used Argo CD for GitOps-based Kubernetes deployment. Git is used for source control and merge requests. In the delivery process, Docker builds the image, ECR stores the image, and EKS runs the application.

## 10. Explain your complete end-to-end CI/CD pipeline.

**Answer:**

> The process starts when a developer pushes code or creates a merge request. GitLab CI/CD triggers the pipeline. First, the pipeline checks out the code and installs dependencies. Next, it builds the application, for example with Maven where applicable, and runs unit tests. Then it performs code-quality and security checks, such as SonarQube and container scanning with Trivy. After approval, the pipeline builds a Docker image, tags it with a unique version such as the commit ID, and pushes it to Amazon ECR.
>
> For Kubernetes deployment, the deployment configuration or Helm values are updated in the GitOps repository. Argo CD detects the approved Git change and synchronises the desired state to the EKS cluster. Kubernetes creates or updates the Deployment and Service, and the AWS Load Balancer Controller manages external access through Ingress and an Application Load Balancer. Finally, we verify rollout status, pod health, service health, logs, metrics, and alerts. If the deployment causes an issue, we revert the Git change and allow Argo CD to restore the last known good version.

**Pipeline stages to remember:**

```text
Commit -> Build -> Unit test -> Quality check -> Security scan -> Docker build
-> ECR push -> GitOps configuration update -> Argo CD sync -> Deploy -> Verify
```

**Follow-up: Why use a unique image tag?**

> A unique tag, such as a commit SHA, gives traceability and avoids ambiguity caused by repeatedly changing the `latest` tag. We can identify exactly which source version is running and roll back to a known image.

## 11. Explain the Terraform flow.

**Answer:**

> First I write or update the Terraform configuration. I run `terraform fmt` to format it and `terraform validate` to check syntax and configuration. Then I run `terraform init` to initialise the backend, providers, and modules. Next I run `terraform plan` to preview the changes. The plan is reviewed through the pull request and approval process. After approval, I run `terraform apply`, normally using the reviewed plan in a controlled pipeline. Finally, I verify the resources in AWS and check application health.

**Commands:**

```bash
terraform fmt
terraform validate
terraform init
terraform plan -out=tfplan
terraform apply tfplan
```

**Follow-up: Why run plan before apply?**

> It shows exactly what Terraform will create, modify, or destroy. This helps catch unexpected security, networking, or production-impacting changes before they happen.

## 12. What are Terraform `count` and `for_each`?

**Answer:**

> Both are meta-arguments used to create multiple instances of a resource. `count` creates resources based on a number and uses numeric indexes. `for_each` creates resources from a map or set and uses stable keys.

```hcl
resource "aws_s3_bucket" "logs" {
    count  = 2
    bucket = "example-log-${count.index}"
}

resource "aws_iam_user" "team" {
    for_each = toset(["developer", "support"])
    name     = each.value
}
```

> I prefer `for_each` when each item has a meaningful name or different values, because stable keys reduce unnecessary resource replacement when the collection changes. I use `count` for simple identical resources controlled by a number or a boolean condition.

## 13. What is ECS and why is it used?

**Answer:**

> Amazon ECS is AWS's managed container orchestration service. It runs Docker containers as tasks and maintains them through services. A task definition describes the image, CPU, memory, ports, environment variables, logging, and IAM roles. ECS is used to deploy, scale, and maintain containers without managing the orchestration platform ourselves.
>
> Fargate is a compute option for ECS where AWS manages the underlying servers. My direct production container experience is mainly EKS, but the common concepts are Docker images, ECR, IAM, networking, health checks, scaling, logging, and monitoring.

## 14. Have you used IAM, created users, and what tasks did you perform?

**Answer:**

> Yes, I have worked with IAM permissions, roles, policies, security groups, and workload access as part of AWS infrastructure and Kubernetes operations. I follow least privilege and prefer roles and temporary or federated access over long-lived access keys. For applications, I attach only the permissions required to the workload, such as access to a specific S3 bucket or Secrets Manager secret.
>
> Where human access is required, I would create or manage access according to the client's approved process, enforce MFA, assign only the required policy or group, review access, and remove it when it is no longer needed. I would not create unrestricted administrator users for routine work.

**Important distinction:**

> IAM policies control AWS permissions. Security Groups control network traffic. They are different controls and both may be needed.

## 15. What monitoring tools have you used?

**Answer based on your resume:**

> I have used Amazon CloudWatch, Prometheus, Grafana, Amazon Managed Service for Prometheus, and Amazon Managed Grafana. CloudWatch is useful for AWS service metrics, logs, and alarms. Prometheus collects application and Kubernetes metrics. Grafana provides dashboards and alerting. I monitor CPU, memory, pod or container health, restarts, request latency, error rate, availability, and deployment health.

**Follow-up: What makes a good alert?**

> A good alert represents meaningful impact, has a clear threshold, avoids unnecessary noise, identifies the owner, and links to a runbook or clear action.

## 16. What is Multi-AZ and why is it used?

**Answer:**

> Multi-AZ means deploying resources across more than one Availability Zone in the same AWS Region. It protects the application if one AZ has a failure or becomes unavailable. For example, a load balancer can send traffic to application instances, ECS tasks, or EKS pods in multiple AZs. We also need the data layer, backups, health checks, and capacity to support failover; simply creating subnets in multiple AZs is not enough.

**Follow-up: What if one AZ fails?**

> The load balancer should stop sending traffic to unhealthy targets and route to healthy targets in other AZs. Autoscaling and capacity planning must ensure the remaining AZs can handle the load. The database and stateful services need their own replication or recovery design.

## 17. How do you handle secrets?

**Answer based on your resume:**

> I use AWS Secrets Manager with the Secret Store CSI Driver for Kubernetes workloads. Secrets are stored outside the source code and container image. The workload receives access through an IAM role with least privilege, and the secret is mounted or retrieved at runtime. I avoid hardcoding credentials in Terraform, Git, Dockerfiles, Kubernetes manifests, or pipeline variables where possible. I also use encryption, access logging, rotation where supported, and I would immediately rotate a secret if it were exposed.

**Follow-up: What if a secret is committed to Git?**

> I treat it as compromised. I revoke or rotate it immediately, investigate access, remove the secret from the active code path, update the application to use the approved secret store, and document the incident. Removing the line from the latest commit alone is not enough because the value may remain in Git history.

# 9. Likely Follow-Up Questions After First Level

## Terraform follow-ups

### What is the difference between `terraform plan` and `terraform apply`?

> `plan` previews the changes. `apply` makes the approved changes in the target environment.

### What is `terraform destroy`?

> It removes resources managed by the current Terraform configuration. I would never run it against production without clear approval, a reviewed plan, and confirmation of the impact.

### What is Terraform drift?

> Drift is when the real infrastructure differs from the Terraform configuration or state, usually because of a manual change. I detect it with `terraform plan` and then either update the code through review or revert the manual change through change control.

### How do you protect Terraform state?

> Use a remote encrypted backend, restricted IAM access, versioning, backup, locking, separate environment state, and never commit state to Git.

### What is a data source?

> A data source reads information about an existing resource without managing its lifecycle. For example, Terraform can read an existing VPC or AMI and use its ID in another resource.

## Kubernetes follow-ups

### What is a Pod?

> A Pod is the smallest deployable Kubernetes unit. It contains one or more closely related containers that share networking and storage.

### What is a Deployment?

> A Deployment manages the desired number and version of stateless pods and supports rolling updates and rollback.

### What is a Service?

> A Service provides stable networking and load balancing to a group of pods selected by labels.

### What is HPA?

> Horizontal Pod Autoscaler adjusts the number of pod replicas based on metrics such as CPU, memory, or custom application metrics.

### How do you troubleshoot a pod in `CrashLoopBackOff`?

> I check pod events, container logs, previous container logs, exit codes, configuration, secrets, health probes, resource limits, and recent changes. Then I fix the root cause and verify a stable rollout.

## CI/CD follow-ups

### Why scan Docker images?

> Image scanning identifies vulnerable packages and helps prevent insecure images from reaching production. I would define severity thresholds and an approved exception process.

### How do you roll back a deployment?

> In a GitOps model, I revert the deployment or Helm values change to the last known good Git version. Argo CD then synchronises the cluster back to that desired state. I verify health and document the incident.

### What is GitOps?

> Git is the source of truth for the desired infrastructure or application state. Changes go through review, and a controller such as Argo CD synchronises the approved state to Kubernetes.

## AWS follow-ups

### What is the difference between an IAM role and an IAM user?

> A user is a human identity, while a role provides temporary permissions that a user, application, or AWS service can assume. Roles are preferred for workloads and federated human access.

### What is the difference between an Internet Gateway and a NAT Gateway?

> An Internet Gateway enables public internet connectivity for resources with appropriate public routing. A NAT Gateway allows private resources to make outbound internet connections without accepting direct inbound internet connections.

### What is an ALB?

> An Application Load Balancer distributes HTTP and HTTPS traffic to healthy targets and supports host-based and path-based routing.

### What is ECR?

> ECR is a private AWS container registry where Docker images are stored, scanned, and pulled by services such as ECS and EKS.

### What is CloudTrail?

> CloudTrail records AWS API activity. It helps with auditing, security investigation, and identifying who or what changed a resource.

## Behavioural and client questions

### How do you handle a production change requested by a client?

> I understand the requirement, assess risk and dependencies, create or update the change ticket, obtain the required approvals, prepare implementation and rollback steps, execute during the approved window, validate the result, and document the outcome.

### How do you work with security teams?

> I provide the architecture, data flow, IAM permissions, network requirements, logging plan, and threat or risk details early. I address findings, obtain sign-off, and retain the approval evidence with the change record.

### How do you prioritise when several incidents happen?

> I prioritise by customer and business impact, security risk, scope, and urgency. I communicate clearly, stabilise the highest-impact service first, and involve the appropriate owners rather than trying to solve everything alone.

### How do you explain a technical issue to a non-technical client?

> I explain the customer impact first, then the current status, action being taken, expected next update, and any risk. I avoid unnecessary technical terms and provide deeper detail when the audience needs it.

# 10. Final First-Level Revision Sheet

Remember these short points before the interview:

- **Import:** brings existing resources into Terraform state; configuration still needs to be written and checked.
- **Module:** reusable Terraform code.
- **State:** Terraform's mapping of code to real resources.
- **`.tf` file:** HCL configuration read by Terraform.
- **Ingress:** routes external HTTP/HTTPS traffic to Kubernetes Services.
- **Private subnet:** no direct Internet Gateway route; protects applications and databases.
- **CI/CD:** build, test, scan, package, push, deploy, and verify.
- **Terraform flow:** format, validate, init, plan, review, apply, verify.
- **`count`:** numeric instances; **`for_each`:** stable named instances.
- **ECS:** AWS container orchestration; **Fargate:** AWS-managed compute for containers.
- **IAM:** permissions; use least privilege and roles.
- **Monitoring:** CloudWatch, Prometheus, Grafana, AMP, AMG.
- **Multi-AZ:** spread workloads across Availability Zones for availability.
- **Secrets:** store in Secrets Manager, access through IAM, never hardcode.
- **Production issue:** explain impact, investigation, root cause, fix, validation, and prevention.
