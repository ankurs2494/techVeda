---
title: "DevOps Technical Interview"
date: 2026-02-28
type: page
---

DevOps Technical Interview – Kubernetes, Architecture, Security & CI/CD

This document captures a two-round DevOps technical interview covering Kubernetes fundamentals, networking concepts, AWS architecture design, security best practices, and CI/CD integration. 

Round 1 focuses on Kubernetes services, ingress, networking flow, and cluster internals. 
Round 2 expands into high-availability AWS architecture, routing strategies, infrastructure migration, SonarQube integration, and Terraform drift management. 

The content reflects real-world DevOps experience with production-grade systems and cloud-native best practices.

---

## ⚠️ Important Note

This document is intended for learning and interview preparation purposes only.  
Infrastructure designs, security configurations, and CI/CD practices should be thoroughly reviewed, tested, and adapted to your specific use case.

Do not apply configurations directly to production systems without proper validation and risk assessment.

------------------------------------------------------------------------


## Round 1: Kubernetes & Networking Foundation

----

### 1. What are the different types of services in Kubernetes?
### <font color="BlueViolet">Explanation 1:</font>

**Kubernetes has four types of services:**
- ClusterIP (default) — exposes the service on an internal IP within the cluster. Only accessible from within the cluster. Used for internal microservice communication.
- NodePort — exposes the service on each Node's IP at a static port (range 30000–32767). External traffic can reach the service via `<NodeIP>:<NodePort>`. It's not production-friendly but useful for quick testing.
- LoadBalancer — provisions an external load balancer from the cloud provider (AWS ELB, GCP LB, etc.). It's the standard way to expose services to the internet in cloud environments. Internally it also creates a NodePort and ClusterIP automatically.
- ExternalName — maps a service to a DNS name (like my-db.example.com). It doesn't proxy traffic; it just returns a CNAME record. Useful for integrating external services into cluster DNS.

### <font color="teal">Explanation 2:</font>

**ClusterIP (Default):**
- Exposes the service on an internal IP within the cluster
- Only accessible from within the cluster
- Used for internal microservice communication

**NodePort:**
- Exposes the service on each Node's IP at a static port (range 30000–32767)
- External traffic can reach the service via <NodeIP>:<NodePort>
- Not production-friendly but useful for quick testing

**LoadBalancer:**
- Provisions an external load balancer from the cloud provider (AWS ELB, GCP LB, etc.)
- Standard way to expose services to the internet in cloud environments
- Internally also creates a NodePort and ClusterIP automatically

**ExternalName:**
- Maps a service to a DNS name (like my-db.example.com)
- Doesn't proxy traffic — just returns a CNAME record
- Useful for integrating external services into cluster DNS

----

### 2. There is an EC2 instance in a public subnet; how will you migrate it to a private subnet?
### <font color="BlueViolet">Explanation 1:</font>

**Here's how I approach it:**
First, I never do this blindly — I assess what the instance is serving. If it's internet-facing, I need to ensure a load balancer or NAT gateway is ready before the move.

**The Steps are:**\
create a private subnet in the same VPC and AZ if it doesn't exist, then configure a route table for that private subnet pointing outbound traffic to a NAT Gateway (placed in the public subnet). Then I stop the EC2 instance, go to the network interface settings, and — since AWS doesn't allow directly moving an instance between subnets — I create an AMI snapshot of the instance, launch a new instance from that AMI into the private subnet with the same security groups and IAM role, and then terminate the old one. I update any Elastic IPs, DNS records, or target group registrations pointing to the old instance.
If the instance had a public IP for inbound traffic, I place an Application Load Balancer in the public subnet and point it to the new private instance. This is actually the more secure and correct architecture.

### <font color="teal">Explanation 2:</font>

**Pre-Migration Assessment:**

- Never do this blindly — assess what the instance is serving
- If internet-facing, ensure a Load Balancer or NAT Gateway is ready before the move

**Step-by-Step Migration:**

- Create a private subnet in the same VPC and AZ if it doesn't exist
- Configure a route table for the private subnet pointing outbound traffic to a NAT Gateway (placed in the public subnet)
- Stop the EC2 instance
- Since AWS doesn't allow directly moving an instance between subnets:
    - Create an AMI snapshot of the instance
    - Launch a new instance from that AMI into the private subnet with the same Security Groups and IAM role
    - Terminate the old instance
- Update any Elastic IPs, DNS records, or Target Group registrations pointing to the old instance

**For Internet-Facing Instances:**

- Place an Application Load Balancer in the public subnet
- Point ALB to the new private instance
- This is the more secure and correct architecture

----

### 3. What is Ingress in Kubernetes, and why do we need it?
### <font color="BlueViolet">Explanation 1:</font>
Ingress is a Kubernetes API object that manages external HTTP/HTTPS access to services within the cluster. It acts as a smart reverse proxy or a Layer 7 load balancer.
Without Ingress, to expose 10 microservices externally, you'd need 10 LoadBalancer services — that means 10 cloud load balancers, which is expensive and unmanageable. Ingress solves this by using a single entry point with routing rules.
For example, you can route api.example.com/users to the users-service and api.example.com/orders to the orders-service — all through one Ingress resource. It also handles TLS termination, so you manage SSL certificates in one place rather than at every service.
Ingress requires an Ingress Controller to actually implement the rules — popular ones are NGINX Ingress Controller, AWS ALB Ingress Controller, and Traefik. The Ingress object itself is just a spec; the controller is what does the actual work.

### <font color="teal">Explanation 2:</font>

**What is Ingress:**

- A Kubernetes API object that manages external HTTP/HTTPS access to services within the cluster
- Acts as a smart reverse proxy or Layer 7 load balancer

**Why We Need It:**

- Without Ingress, exposing 10 microservices externally requires 10 LoadBalancer services → 10 cloud load balancers → expensive and unmanageable
- Ingress provides a single entry point with path and host-based routing rules
- Example:
    - `api.example.com/user`s` → routes to users-service
    - `api.example.com/orders` → routes to orders-service
- Handles TLS termination in one place rather than at every individual service

**Ingress Controller:**

- Ingress object is just a spec/definition — it does nothing on its own
- Requires an Ingress Controller to implement the rules
- Popular controllers: NGINX Ingress Controller, AWS ALB Ingress Controller, Traefik

----

### 4. Why should we use Kubernetes? What specific challenges does it solve?
### <font color="BlueViolet">Explanation 1:</font>
Before Kubernetes, running containerized apps at scale was chaotic. Here's what it concretely solves:
Container orchestration — manually managing which container runs on which server doesn't scale. Kubernetes schedules containers across nodes based on resource availability automatically.
Self-healing — if a container crashes, Kubernetes restarts it. If a node dies, it reschedules pods on healthy nodes. No manual intervention needed at 3 AM.
Horizontal scaling — with HPA (Horizontal Pod Autoscaler), it scales pods up and down based on CPU, memory, or custom metrics automatically.
Rolling deployments and rollbacks — you can deploy new versions with zero downtime and roll back instantly if something goes wrong.
Service discovery and load balancing — services find each other via DNS names within the cluster without hardcoding IPs.
Configuration and secret management — ConfigMaps and Secrets decouple config from code, which is a twelve-factor app principle.
Resource efficiency — bin packing across nodes means you use your infrastructure more efficiently compared to dedicated VMs per service.
In short, Kubernetes lets small teams operate large-scale, resilient infrastructure that previously required massive ops teams.

### <font color="teal">Explanation 2:</font>

**Container Orchestration:**

- Manually managing which container runs on which server doesn't scale
- Kubernetes **automatically schedules containers** across nodes based on resource availability

**Self-Healing:**
- If a container crashes → Kubernetes **automatically restarts it**
- If a node dies → **reschedules pods** on healthy nodes
- No manual intervention needed at 3 AM

**Horizontal Scaling:**
- **HPA (Horizontal Pod Autoscaler)** scales pods up and down automatically
- Based on CPU, memory, or custom metrics

**Rolling Deployments & Rollbacks:**
- Deploy new versions with **zero downtime**
- **Instant rollback** if something goes wrong

**Service Discovery & Load Balancing:**
- Services find each other via **DNS names** within the cluster
- No hardcoding of IPs required

**Configuration & Secret Management:**
- **ConfigMaps and Secrets** decouple configuration from code
- Follows the twelve-factor app principle

**Resource Efficiency:**
- **Bin packing** across nodes means better infrastructure utilization
- Compared to dedicated VMs per service — significant cost savings


Kubernetes lets small teams operate large-scale, resilient infrastructure that previously required massive ops teams.

----

### 5. Explain the detailed flow: How does a user request travel from a browser to a Kubernetes cluster?
### <font color="BlueViolet">Explanation 1:</font>
This is end-to-end and I'll walk through every layer:
The user types https://app.example.com in the browser. DNS resolves that domain to the IP of a cloud Load Balancer (like AWS ALB or NLB). This load balancer sits outside the cluster in the public subnet.
The load balancer forwards the request to the Ingress Controller pod running inside the cluster — typically NGINX or ALB controller. The Ingress Controller evaluates the host and path rules defined in the Ingress resource and determines which backend Service to forward to.
The request hits the Kubernetes Service (ClusterIP). The Service uses iptables rules (managed by kube-proxy on each node) or IPVS to load balance across healthy Pod endpoints. It selects one of the matching pods based on the selector labels.
The request reaches the application Pod — the container inside processes it, queries a database or other internal service if needed, and sends a response back through the same chain.
TLS termination typically happens at the Ingress Controller layer, so the internal cluster traffic can be plain HTTP.

### <font color="teal">Explanation 2:</font>

**Step 1 — DNS Resolution:**
- User types `https://app.example.com` in the browser
- DNS resolves the domain to the IP of a **cloud Load Balancer** (AWS ALB or NLB)
- Load Balancer sits **outside the cluster** in the public subnet


**Step 2 — Ingress Controller:**
- Load Balancer forwards the request to the **Ingress Controller pod** inside the cluster (NGINX or ALB Controller)
- Ingress Controller evaluates **host and path rules** defined in the Ingress resource
- Determines which **backend Service**to forward the request to


**Step 3 — Kubernetes Service (ClusterIP):****
- Request hits the **ClusterIP Service**
- Service uses **iptables rules** (managed by kube-proxy) or **IPVS** to load balance across healthy Pod endpoints
- Selects one of the matching pods based on **selector labels**


**Step 4 — Application Pod:**
- Request reaches the **application Pod**
- Container processes the request
- Queries a database or other internal services if needed
- Sends **response back through the same chain**


**Step 5 — TLS Termination:**
- TLS termination happens at the **Ingress Controller layer**
- Internal cluster traffic can be **plain HTTP**

----

### 6. What happens internally when you run `kubectl apply -f deployment.yaml`?
### <font color="BlueViolet">Explanation 1:</font>
This is a great question that tests deep Kubernetes knowledge.
kubectl reads the YAML file and sends an HTTP PATCH or POST request to the kube-apiserver REST API endpoint. Before processing, the API server performs authentication (who are you — via kubeconfig cert or token), authorization (are you allowed — via RBAC), and admission control (mutating webhooks may modify the object, validating webhooks may reject it).
The API server validates the object schema and then persists the desired state to etcd — the cluster's key-value store. This is the source of truth.
The Deployment Controller inside the kube-controller-manager is watching etcd for Deployment changes. It detects the new or updated Deployment and creates or updates a ReplicaSet to match the desired replica count.
The ReplicaSet Controller then ensures the correct number of Pods exist. It creates Pod objects in etcd with status "Pending."
The kube-scheduler is watching for unscheduled pods. It evaluates all nodes based on resource requests, node selectors, taints/tolerations, and affinity rules, then assigns the pod to the most suitable node by writing the node name to the Pod spec in etcd.
The kubelet on the selected node is watching the API server for pods assigned to its node. It sees the new pod, calls the Container Runtime (containerd or Docker) via the CRI interface to pull the image and start the container. It also sets up networking via the CNI plugin (like Calico or Flannel), which assigns a pod IP.
The kubelet then continuously reports pod status back to the API server, and the pod transitions from Pending → ContainerCreating → Running.
That entire flow — from your command to a running container — typically completes in seconds.

### <font color="teal">Explanation 2:</font>

**Step 1 — kubectl sends API Request:**
- `kubectl` reads the YAML file
- Sends an **HTTP PATCH or POST** request to the `kube-apiserver` REST API endpoint

**Step 2 — API Server Validation (3 stages):**
- Authentication — Who are you? (via kubeconfig cert or token)
- Authorization — Are you allowed? (via RBAC)
- Admission Control — Mutating webhooks may modify the object; Validating webhooks may reject it

**Step 3 — Persisted to etcd:**
- API server validates the object schema
- Persists the **desired state to etcd** — the cluster's key-value store and single source of truth

**Step 4 — Deployment Controller:**
- Deployment Controller inside `kube-controller-manager` watches etcd for Deployment changes
- Detects the new/updated Deployment
- Creates or updates a **ReplicaSet** to match the desired replica count

**Step 5 — ReplicaSet Controller:**
- Ensures the correct number of **Pods exist**
- Creates Pod objects in etcd with status **"Pending"**

**Step 6 — kube-scheduler:**
- Watches for **unscheduled pods**
- Evaluates all nodes based on resource requests, node selectors, taints/tolerations, and affinity rules
- Assigns the pod to the most suitable node by **writing the node name** to the Pod spec in etcd

**Step 7 — kubelet:**
- `kubelet` on the selected node watches the API server for **pods assigned to its node**
- Calls the **Container Runtime** (containerd or Docker) via CRI interface to pull the image and start the container
- Sets up networking via **CNI plugin** (Calico or Flannel) which assigns a Pod IP

**Step 8 — Pod Status Update:**
- `kubelet` continuously reports pod status back to the API server
- Pod transitions: **Pending → ContainerCreating → Running**


> The entire flow — from your `kubectl` command to a running container — typically completes in seconds.

----

## Round 2: Architecture, Security & CI/CD

----

### 1. What is the difference between an API Gateway and Kubernetes Ingress?
### <font color="BlueViolet">Explanation 1:</font>

Both sit at the edge and route traffic, but they operate at fundamentally different layers and solve different problems.
Kubernetes Ingress is a cluster-internal construct. It operates at Layer 7 and handles HTTP/HTTPS routing to services within the cluster based on host and path rules. It's lightweight, Kubernetes-native, and managed via YAML manifests. It has no concept of authentication, rate limiting, or API monetization out of the box.
API Gateway (like AWS API Gateway, Kong, or Apigee) is a full-featured API management platform. It handles authentication and authorization (OAuth, JWT, API keys), rate limiting and throttling, request/response transformation, caching, usage analytics, and developer portals. It sits in front of your entire backend, which could be Lambda, EC2, ECS, or even a Kubernetes cluster.
In real architectures I've built, both are used together — API Gateway handles cross-cutting concerns like auth and rate limiting at the edge, then routes to an ALB which hits the Ingress Controller inside the cluster. Ingress then handles internal service routing. Each does what it's best at.

### <font color="teal">Explanation 2:</font>

**Kubernetes Ingress:**

- Cluster-internal construct operating at Layer 7
- Handles HTTP/HTTPS routing to services within the cluster based on host and path rules
- Lightweight, Kubernetes-native, managed via YAML manifests
- No built-in authentication, rate limiting, or API monetization
- Managed by an Ingress Controller (NGINX, AWS ALB Controller, Traefik)

**API Gateway (AWS API Gateway, Kong, Apigee):**

- Full-featured API management platform
- Handles Authentication & Authorization (OAuth, JWT, API keys)
- Provides rate limiting, throttling, request/response transformation
- Built-in caching, usage analytics, and developer portals
- Sits in front of entire backend — Lambda, EC2, ECS, or Kubernetes

**Real-world usage:**

- Both are used together in production
- API Gateway handles cross-cutting concerns at the edge → routes to ALB → hits Ingress Controller → internal service routing
- Each tool does what it does best

----

### 2. How would you build a highly available and scalable architecture on AWS?
### <font color="BlueViolet">Explanation 1:</font>

I'll describe the architecture I've built and recommend in production environments.
Networking foundation — a VPC spanning at least two (ideally three) Availability Zones. Public subnets host load balancers and NAT Gateways. Private subnets host application servers. Isolated subnets host databases. Never put application or database servers in a public subnet.
Traffic entry — Route 53 for DNS with health check-based failover. An Application Load Balancer in public subnets across multiple AZs. The ALB terminates SSL using ACM certificates.
Compute layer — EC2 instances inside an Auto Scaling Group spread across multiple AZs, with target tracking policies based on CPU or request count. If containerized, I use EKS with managed node groups or Fargate for serverless pods.
Database layer — RDS with Multi-AZ deployment for automatic failover, or Aurora which is inherently multi-AZ with up to 15 read replicas. ElastiCache (Redis) in cluster mode for session management and caching.
Storage — S3 with versioning and cross-region replication for static assets. EFS for shared file systems across instances.
Observability — CloudWatch for metrics and alarms, CloudWatch Logs Insights for log analysis, X-Ray for distributed tracing. I also set up dashboards in Grafana pulling from CloudWatch.
Security — WAF attached to the ALB, Security Groups with least-privilege rules, IAM roles instead of access keys for EC2, secrets in AWS Secrets Manager, and VPC Flow Logs enabled.
The result is an architecture with no single point of failure at any layer.

### <font color="teal">Explanation 2:</font>

**Networking Foundation:**

- VPC spanning minimum 2 AZs (ideally 3)
- Public subnets → Load Balancers & NAT Gateways
- Private subnets → Application Servers
- Isolated subnets → Databases
- Never expose app or DB servers in public subnets

**Traffic Entry:**

- Route 53 for DNS with health check-based failover
- Application Load Balancer (ALB) in public subnets across multiple AZs
- SSL termination at ALB using ACM certificates

**Compute Layer:**

- EC2 inside Auto Scaling Groups spread across AZs
- Target tracking policies based on CPU or request count
- For containers → EKS with managed node groups or Fargate for serverless pods

**Database Layer:**

- RDS Multi-AZ for automatic failover
- Aurora — inherently multi-AZ with up to 15 read replicas
- ElastiCache (Redis) in cluster mode for session management and caching

**Storage:**

- S3 with versioning and cross-region replication for static assets
- EFS for shared file systems across instances

**Observability:**

- CloudWatch for metrics, alarms, and log analysis
- X-Ray for distributed tracing
- Grafana dashboards pulling from CloudWatch

**Security:**

- WAF attached to the ALB
- Security Groups with least-privilege rules
- IAM roles instead of access keys on EC2
- Secrets stored in AWS Secrets Manager
- VPC Flow Logs enabled

----

### 3. What is the difference between a NAT Instance and a NAT Gateway?

### <font color="BlueViolet">Explanation 1:</font>

A NAT Instance is a regular EC2 instance configured to do Network Address Translation. You manage it yourself — patching, scaling, high availability, and you must disable source/destination checks on it. It's a single point of failure unless you build your own HA setup with scripts and Elastic IPs. The only reason to use it today is cost — in very low-traffic environments it can be cheaper.
A NAT Gateway is a fully managed AWS service. AWS handles availability, scaling (up to 45 Gbps), patching, and redundancy within a single AZ. You don't manage any underlying infrastructure. For high availability across AZs, you deploy one NAT Gateway per AZ — each private subnet's route table points to the NAT Gateway in its own AZ to avoid cross-AZ data transfer costs.
In production, I always use NAT Gateway. The operational overhead of managing a NAT Instance is not worth the marginal cost savings. I've seen teams burn engineering hours debugging a failed NAT Instance at 2 AM — NAT Gateway eliminates that class of problem.

### <font color="teal">Explanation 2:</font>

| Feature            | NAT Instance                     | NAT Gateway                          |
|--------------------|----------------------------------|---------------------------------------|
| Management         | Self-managed EC2 instance        | Fully managed by AWS                 |
| Availability       | Single point of failure (unless you design HA) | Highly available within an AZ |
| Scaling            | Manual scaling (change instance type) | Automatically scales (up to ~45 Gbps) |
| Patching           | You must handle OS updates & security patches | AWS handles maintenance & updates |
| Cost               | Cheaper at low traffic           | Higher cost but predictable billing  |
| Source/Dest Check  | Must disable manually            | Not required                         |

**Best Practice:**

- Always use NAT Gateway in production
- Deploy one NAT Gateway per AZ — each private subnet's route table points to NAT Gateway in its own AZ to avoid cross-AZ data transfer costs
- NAT Instances are only justifiable for extreme cost optimization in non-critical, low-traffic environments

----

### 4. What is SonarQube? How do you integrate it with Jenkins, and what real-life issues have you faced during integration?
### <font color="BlueViolet">Explanation 1:</font>

What it is — SonarQube is a static code analysis platform that measures code quality across dimensions like bugs, vulnerabilities, code smells, duplications, test coverage, and technical debt. It enforces quality gates that can block deployments if thresholds aren't met.
Integration with Jenkins — I install the SonarQube Scanner plugin in Jenkins and configure the SonarQube server URL and authentication token in Jenkins global settings. In the pipeline, I add a stage that runs the scanner. For a Java/Maven project it looks roughly like running mvn sonar:sonar with the project key and server URL passed as parameters. After the scan, I add a waitForQualityGate() Step that pauses the pipeline and fails the build if the quality gate doesn't pass.

**Real issues I've faced:**\
The most common one is quality gate failures blocking releases when SonarQube is introduced to a legacy codebase. The code has thousands of pre-existing issues and everything fails on day one. The fix is to use the "New Code" baseline feature — you only gate on issues introduced in new code, and create a separate tech debt reduction backlog for legacy issues.
Another issue was test coverage drops causing false failures. A developer would delete a test file while refactoring and coverage would drop below the threshold, breaking unrelated pipelines. We solved this by setting coverage thresholds at the project level carefully and adding coverage trend monitoring rather than absolute thresholds initially.
I also faced scanner timeout issues on large monorepos. The scanner was timing out in Jenkins because the analysis was taking 20+ minutes. We solved it by running the scan in parallel with tests rather than sequentially, and by excluding auto-generated code directories from analysis.
Finally, token expiration in SonarQube would silently break the integration. The Jenkins job would fail with an authentication error mid-pipeline. We solved this by storing the token in Jenkins credentials with a rotation reminder and adding clear error handling to catch auth failures specifically.


### <font color="teal">Explanation 2:</font>

**What is SonarQube:**

- Static code analysis platform measuring code quality
- Tracks bugs, vulnerabilities, code smells, duplications, test coverage, and technical debt
- Enforces **Quality Gates** that can block deployments if thresholds aren't met

**Jenkins Integration Steps:**

- Install **SonarQube Scanner plugin** in Jenkins
- Configure SonarQube server URL and auth token in Jenkins global settings
- Add a scan stage in the pipeline (e.g., `mvn sonar:sonar` for Maven projects)
- Use `waitForQualityGate()` step to pause pipeline and fail build if quality gate doesn't pass

**Real Issues Faced:**

- **Quality gate failures on legacy codebases** — thousands of pre-existing issues cause everything to fail on day one
  - Fix: Use "New Code" baseline — gate only on issues in new code, create separate tech debt backlog for legacy issues

- **Test coverage drops causing false failures** — deleting a test file drops coverage below threshold, breaking unrelated pipelines
  - Fix: Set coverage thresholds carefully, monitor coverage trends rather than hard absolute thresholds initially

- **Scanner timeouts on large monorepos** — analysis taking 20+ minutes causing Jenkins job timeouts
  - Fix: Run scan in parallel with tests, exclude auto-generated code directories from analysis

- **Token expiration silently breaking integration** — Jenkins jobs fail mid-pipeline with auth errors
  - Fix: Store token in Jenkins credentials with rotation reminders, add explicit error handling to catch auth failures

----

### 5. What are the different Routing Policies available in Route 53?
### <font color="BlueViolet">Explanation 1:</font>

Simple Routing — one record, one or more IP values returned randomly. No health checks. Used for single resources.
Weighted Routing — distribute traffic across multiple resources by percentage. I use this for canary deployments — send 5% of traffic to the new version, monitor, then gradually increase.
Latency-based Routing — routes users to the AWS region that provides the lowest latency. Route 53 measures latency between the user's location and AWS regions and picks the best one automatically.
Failover Routing — active-passive setup. Primary record serves traffic normally; if its health check fails, Route 53 automatically routes to the secondary. Used for disaster recovery.
Geolocation Routing — routes based on the geographic location of the user (country or continent level). Useful for serving region-specific content or complying with data residency laws. Different from latency-based — it's about location, not speed.
Geoproximity Routing — routes based on geographic location of both users and resources, with a bias value you can adjust to shift more or less traffic toward a region. Available through Traffic Flow.
Multivalue Answer Routing — like Simple but returns multiple values with health checks. Up to 8 healthy records returned per query. It's not a replacement for a load balancer but provides basic client-side load balancing.
IP-based Routing — routes traffic based on the CIDR block of the client's IP. Useful when you know your users' IP ranges and want to send them to specific endpoints.

### <font color="teal">Explanation 2:</font>

| Routing Policy      | What It Does                                                                 | Use Case                                                   |
|---------------------|------------------------------------------------------------------------------|------------------------------------------------------------|
| Simple              | Returns a single record or multiple IPs randomly (no health checks by default) | Single resource, basic DNS routing                       |
| Weighted            | Distributes traffic by assigned percentage across multiple resources         | Canary deployments, A/B testing                            |
| Latency-Based       | Routes users to the region with the lowest network latency                  | Multi-region performance optimization                      |
| Failover            | Active-passive setup; routes to secondary if primary health check fails     | Disaster Recovery (DR)                                     |
| Geolocation         | Routes traffic based on user’s country or continent                         | Region-specific content, data residency compliance         |
| Geoproximity        | Routes based on user location and resource location with adjustable bias    | Traffic shifting between regions                           |
| Multi-Value Answer  | Returns multiple healthy records for basic client-side load balancing       | Lightweight redundancy (not a replacement for ALB)        |
| IP-Based Routing    | Routes traffic based on client’s IP address or CIDR range                   | Direct specific IP ranges to dedicated endpoints           |

----

### 6. Explain the request flow from a Load Balancer to an EC2 web server.
### <font color="BlueViolet">Explanation 1:</font>
The client sends an HTTP/HTTPS request to the ALB's DNS name, which resolves to one of the ALB's nodes across AZs.
The ALB terminates the TLS connection using the ACM certificate — decrypting the traffic. It then evaluates listener rules top to bottom — checking host headers, path patterns, query strings — and determines which Target Group to forward the request to.
The ALB performs a health check against registered EC2 instances and only routes to healthy ones. It selects a target using its load balancing algorithm (round-robin for HTTP, least-outstanding-requests is also available) and forwards the request. The ALB preserves or rewrites headers as configured — importantly, it adds X-Forwarded-For with the client's real IP, since from the EC2's perspective the request comes from the ALB's IP.
The request arrives at the EC2 instance's Security Group — which must have an inbound rule allowing traffic from the ALB's Security Group (not from 0.0.0.0/0, that's a common misconfiguration I always fix). The web server (Nginx, Apache) processes the request and sends the response back to the ALB, which forwards it to the client.
If HTTPS is used end-to-end, I configure the ALB to use HTTPS to the backend as well, with the instance's own certificate — this is end-to-end encryption in transit.

### <font color="teal">Explanation 2:</font>

**Step 1 — DNS Resolution:** Client sends request to ALB's DNS name → resolves to one of the ALB nodes across AZs

**Step 2 — TLS Termination:** ALB decrypts HTTPS traffic using ACM certificate

**Step 3 — Listener Rule Evaluation:** ALB evaluates rules top-to-bottom — checks host headers, path patterns, query strings — determines the target Target Group

**Step 4 — Health Check:** ALB only routes to instances that are passing health checks

**Step 5 — Target Selection:** Selects a healthy EC2 instance using round-robin or least-outstanding-requests algorithm

**Step 6 — Header Injection:** ALB adds `X-Forwarded-For` header with the real client IP before forwarding

**Step 7 — Security Group Check:** EC2's Security Group must allow inbound traffic from ALB's Security Group (not 0.0.0.0/0 — a common misconfiguration)

**Step 8 — Web Server Processing:** Nginx/Apache processes the request and sends response back to ALB → forwarded to client

**End-to-End Encryption:** For full HTTPS, configure ALB to use HTTPS to backend as well with the instance's own certificate


----

### 7. How can you migrate a web server hosted on an EC2 instance to another AWS region?
### <font color="BlueViolet">Explanation 1:</font>

My approach follows a structured migration with zero-downtime as the goal.
First, I create an AMI of the source EC2 instance — this captures the OS, application code, and configuration. I then copy that AMI to the target region using the copy-image API. AMIs are region-specific so this step is mandatory.
In the target region, I set up the full networking stack first — VPC, subnets, route tables, Internet Gateway, Security Groups mirroring the source region's setup. If I'm using Infrastructure as Code (which I always do), I just run the same Terraform with the new region as the target, which makes this nearly instant.
I launch a new EC2 instance from the copied AMI in the target region, assign appropriate IAM roles, and configure any environment-specific settings — like updating database connection strings to point to a new RDS instance in the target region or to a global Aurora cluster.
For data migration, if there's a database, I use RDS snapshot copy for the same cross-region copy process, or AWS DMS for live migration with minimal downtime. For S3 assets, I enable cross-region replication.
Once the target environment is validated and tested, I use Route 53 weighted routing to shift traffic gradually — 10% to new region, monitor error rates and latency, then increase to 50%, then 100%. This gives me an easy rollback path at any point. Once I'm confident, I remove the old region's weight and eventually terminate the source resources.

### <font color="teal">Explanation 2:</font>

**Step 1 — Create AMI:**

- Create an AMI of the source EC2 instance (captures OS, app code, configuration)
- Copy AMI to target region using copy-image API (AMIs are region-specific)

**Step 2 — Setup Networking in Target Region:**

- Create VPC, subnets, route tables, IGW, and Security Groups mirroring source region
- If using Terraform, run same code with new region as target — nearly instant

**Step 3 — Launch New Instance:**

- Launch EC2 from copied AMI in target region
- Assign appropriate IAM roles
- Update environment-specific configs (DB connection strings, endpoints)

**Step 4 — Data Migration:**

- RDS → Use RDS snapshot cross-region copy or AWS DMS for live migration
- S3 → Enable cross-region replication

**Step 5 — Validation:**

- Fully test the target environment before shifting any traffic

**Step 6 — Traffic Cutover (Zero Downtime):**

- Use Route 53 Weighted Routing to gradually shift traffic
- 10% → new region, monitor error rates and latency
- Increase to 50% → then 100%
- Easy rollback at any point by adjusting weights
- Once stable → remove old region weight → terminate source resources

----

### 8. How do you handle "Infrastructure Drift" when using Terraform?
### <font color="BlueViolet">Explanation 1:</font>

Infrastructure drift happens when the actual state of your cloud resources diverges from what Terraform believes it to be — usually because someone made a manual change in the console, or a cloud service auto-modified a resource.
My first line of defense is prevention. I enforce a policy that no one touches infrastructure outside of Terraform. IAM permissions are restricted so most team members can't make manual changes to production resources directly. All changes go through pull requests against the Terraform codebase.
For detection, I run terraform plan in a scheduled CI/CD pipeline — even when no code has changed — and alert on any non-empty plan output. I've also used tools like Atlantis which runs plans automatically on PRs and Driftctl which specifically scans for drift and gives a detailed drift report. AWS Config with custom rules is another layer that catches configuration deviations.
When drift is detected, I have two options depending on context. If the manual change was valid and needs to be kept, I use terraform import to bring the resource under Terraform management and update the code to match. If the drift is unauthorized or incorrect, I run terraform apply to reconcile state back to the desired configuration, effectively reverting the manual change.
For state file integrity, I store the Terraform state in S3 with versioning enabled and DynamoDB for state locking — this prevents concurrent applies that could corrupt state, which is another form of drift. I never let developers have write access to the state file directly.
The key principle is treating Terraform state as the single source of truth and building guardrails around it operationally and technically.

### <font color="teal">Explanation 2:</font>

**Prevention (First Line of Defense):**

- Enforce policy — no manual changes outside of Terraform
- Restrict IAM permissions so team members cannot make manual changes to production resources
- All infrastructure changes go through Pull Requests against Terraform codebase

**Detection:**

- Run terraform plan on a scheduled CI/CD pipeline even with no code changes — alert on any non-empty plan output
- Use Atlantis — auto-runs plans on PRs
- Use Driftctl — specifically scans for drift and generates detailed drift reports
- AWS Config with custom rules as an additional detection layer

**Remediation:**

- If manual change was valid and needs to be kept → use terraform import to bring resource under Terraform management and update code to match
- If drift is unauthorized or incorrect → run terraform apply to reconcile state back to desired configuration


**State File Best Practices:**

- Store Terraform state in S3 with versioning enabled
- Use DynamoDB for state locking — prevents concurrent applies that could corrupt state
- Never give developers direct write access to the state file
- Treat Terraform state as the single source of truth with operational and technical guardrails around it