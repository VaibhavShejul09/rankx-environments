Root App
   ↓
platform/apps
   ↓
Applications created:
   - namespaces
   - storage
   - projects
   - kyverno
   - applicationsets  
           ↓
ApplicationSet runs
           ↓
Creates Applications dynamically
           ↓
Deploys your microservices + DB

------------------------------------------

# kubectl apply root app
   ↓
ArgoCD reads platform/apps
   ↓
Creates all child apps:
   - namespaces
   - storage
   - kyverno
   - projects
   - applicationsets
   ↓
Everything deployed automatically 🚀




-----------------------------------------------------------

Application (root)
   ↓
Application (applicationsets)
   ↓
ApplicationSet
   ↓
Applications (auto-generated)
   ↓
Pods / StatefulSets / Services



---------------------------------------------
Project: 

🎯 5. How It Connects to Your System
Flow
ApplicationSet
   ↓
Creates Application
   ↓
Application uses:
   project: microservices
   ↓
ArgoCD checks rules
   ↓
Allowed? → Deploy
Not allowed? → FAIL ❌

🔥 6. Real Failure Example

If project allows only:

namespace: dev

But app tries:

namespace: prod

👉 Result:

Deployment FAILED ❌

------------------------------------

Resources for mysql,
resource quata for that namespace 

-----------------------------------------------------

# advance 

🎯 ⚠️ OPTIONAL (CAN DO LATER — NOT BLOCKING)
🔸 HPA (Auto Scaling)

Not needed now

🔸 PodDisruptionBudget

Not needed for dev

🔸 NetworkPolicy

Advanced — later

🔸 Observability (Prometheus/Grafana)

Later

🔸 SecurityContext

Later




=====================================================================
Date : 17-04-2026
Kyverno Policies
=====================================================================

=====================================================================
Date : 17-04-2026
Network Policies
=====================================================================

## How to Enable NetworkPolicy in EKS

EKS + AWS VPC CNI + Calico

# Intallation

helm repo add projectcalico https://docs.projectcalico.org/charts
helm install calico projectcalico/tigera-operator

----------------------------

# With Calico:
Pod → Calico → Policy check → Allow/Deny



# 🚨 7. Common Mistakes (You must avoid)
❌ Mistake 1:

Applying NetworkPolicy without Calico
👉 It won’t work

❌ Mistake 2:

Blocking DNS

👉 Always allow:

Pod → kube-dns
❌ Mistake 3:

Applying policy without testing


## Production Security Model (VERY IMPORTANT)

We follow:

DEFAULT: DENY EVERYTHING
ALLOW: ONLY REQUIRED TRAFFIC

This is called Zero Trust Networking


----------------------------------------------
When you install Calico:

calico-node DaemonSet on each node
calico controllers


Git YAML
 ↓
ArgoCD sync
 ↓
Kubernetes API stores NetworkPolicy
 ↓
Calico watches API
 ↓
Calico programs iptables/eBPF rules
 ↓
Traffic allowed/blocked


-----------------------------

Platform Layer First:
Order	Component
1	EKS Cluster
2	OIDC / IAM addons
3	EBS CSI
4	Calico / Cilium
5	ArgoCD
6	Kyverno
7	NetworkPolicies
8	Applications


---------------------------------
Bootstrap manually:
Cluster
CNI
Storage
ArgoCD

Then everything else via GitOps.

-----------------------------------
## Policies 

1. Default Deny Policies
2. DNS Allow Policies
3. Ingress Access Policies
4. Database Isolation Policies
5. Egress Control Policies
6. Namespace Isolation Policies
7. Monitoring / Observability Policies
8. Ingress Controller / Load Balancer Policies
9. Batch / Job Policies
10. External Dependency Policies
11. Multi-Tenant Team Isolation Policies
12. Emergency Break-Glass Policies