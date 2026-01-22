---
title: "Kubernetes Production Issues"
date: 2026-01-22
type: page
---

Kubernetes Production Issues (Most Common → Rare & Critical)

---

### 1️⃣ Pods Stuck in `CrashLoopBackOff` (MOST COMMON)

**Symptoms** 
- Pod keeps restarting
- Application never becomes healthy

**Common Causes** 
- App crashes on startup
- Wrong environment variables
- Dependency unavailable (DB, API)
- Incorrect command/args

**Troubleshooting Steps** 
```bash
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous
```

**Production Fix** 
- Fix application config
- Add readiness/liveness probes
- Add retries & graceful error handling
- Use initContainers for dependencies

**Interview Tip** 
> “CrashLoopBackOff is often an application issue, not Kubernetes.”

---

### 2️⃣ Pods Stuck in `Pending`

**Symptoms** 
- Pod never schedules
- No container starts

**Common Causes** 
- Insufficient CPU/memory
- Node selector / taints mismatch
- PVC not bound

**Troubleshooting Steps** 
```bash
kubectl describe pod <pod>
kubectl get nodes
kubectl describe node <node>
```

**Production Fix** 
- Adjust resource requests
- Add more nodes / autoscaling
- Fix node selectors & tolerations

---

### 3️⃣ Service Not Reachable

**Symptoms** 
- Pod is running
- Service returns timeout / connection refused

**Common Causes** 
- Wrong service selector
- App listening on different port
- NetworkPolicy blocking traffic

**Troubleshooting Steps** 
```bash
kubectl get svc
kubectl describe svc <service>
kubectl get endpoints <service>
kubectl exec -it <pod> -- curl <service>:<port>
```

**Production Fix** 
- Fix selectors
- Ensure app binds to 0.0.0.0
- Adjust NetworkPolicies

---

### 4️⃣ High Pod Restarts Due to OOMKilled

**Symptoms** 
- Pods restart under load
- Status: OOMKilled

**Root Cause** 
- Memory limit too low
- Memory leak in app

**Troubleshooting Steps** 
```bash
kubectl describe pod <pod>
kubectl top pod
```

**Production Fix** 
- Increase memory limits
- Add memory profiling
- Use Vertical Pod Autoscaler (VPA)

---

### 5️⃣ Node Goes `NotReady`

**Symptoms** 
- Pods evicted
- Traffic disruption

**Causes** 
- Disk pressure
- Memory pressure
- Kubelet crash
- Network issues

**Troubleshooting Steps** 
```bash
kubectl get nodes
kubectl describe node <node>
journalctl -u kubelet
```

**Production Fix** 
- Add monitoring & alerts
- Disk cleanup automation
- Node auto-replacement (ASG)

---

### 6️⃣ Rolling Deployment Causes Downtime

**Symptoms** 
- Users see 5xx errors during deploy

**Root Cause** 
- Bad readiness probe
- maxUnavailable misconfigured
- App not graceful on shutdown

**Troubleshooting Steps** 
```bash
kubectl describe deployment <deploy>
```

**Production Fix** 
```yaml
strategy:
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

- Add readiness probes
- preStop hooks
- Graceful shutdown

---

### 7️⃣ DNS Resolution Fails Inside Cluster

**Symptoms** 
- Services can’t reach each other by name
- NXDOMAIN errors

**Root Cause** 
- CoreDNS crash
- Node network issue
- Excessive DNS load

**Troubleshooting Steps** 
```bash
kubectl get pods -n kube-system
kubectl logs -n kube-system coredns
kubectl exec -it <pod> -- nslookup kubernetes.default
```

**Production Fix** 
- Scale CoreDNS
- Add node-local DNS cache
- Limit DNS queries

---

### 8️⃣ HPA Not Scaling When Traffic Increases

**Symptoms** 
- High CPU usage
- No new pods created

**Root Cause** 
- Metrics server missing
- Wrong resource requests
- Custom metrics misconfigured

**Troubleshooting Steps** 
```bash
kubectl get hpa
kubectl describe hpa <hpa>
kubectl top pods
```

**Production Fix** 
- Install metrics-server
- Correct CPU requests
- Validate custom metrics pipeline

---

### 9️⃣ Cluster Becomes Extremely Slow (RARE & CRITICAL)

**Symptoms** 
- kubectl commands lag
- Pods take minutes to start
- API server latency spikes

**Root Cause** 
- etcd latency
- Disk I/O saturation
- Too many CRDs/events

**Troubleshooting Steps** 
```bash
kubectl get --raw /metrics
etcdctl endpoint health
```

**Production Fix** 
- Move etcd to SSD
- Limit event retention
- Reduce CRD size
- Regular etcd compaction

---

### 🔟 etcd Quorum Loss (VERY RARE)

**Symptoms** 
- Cluster read-only
- Leader election failures

**Root Cause** 
- Network partition
- Loss of majority etcd members

**Troubleshooting Steps** 
```bash
etcdctl member list
etcdctl endpoint status
```

**Production Fix** 
- Restore from snapshot
- Odd number of etcd nodes
- Automated backups

---

### 1️⃣1️⃣ NetworkPolicy Blocks Traffic

**Symptoms** 
- Partial connectivity
- Silent failures

**Troubleshooting Steps** 
```bash
kubectl get networkpolicy
kubectl describe networkpolicy <policy>
```

**Production Fix** 
- Explicit allow rules
- Test policies in staging

---

### 1️⃣2️⃣ Certificate Expiry Brings Down Cluster

**Symptoms** 
- API server unreachable
- kubelet auth failures

**Troubleshooting Steps** 
```bash
kubeadm certs check-expiration
```

**Production Fix** 
```bash
kubeadm certs renew all
```

- Calendar alerts
- Automated renewal

---

### Interview Answer Structure

1. Symptom
2. Impact
3. Root cause
4. Troubleshooting
5. Fix
6. Prevention
