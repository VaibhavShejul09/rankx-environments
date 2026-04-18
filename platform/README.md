🧠 Typical Production Bootstrap Layers

# Layer 1: Cluster Foundation
EKS cluster
Node groups / Karpenter
IAM / OIDC
Base networking


# Layer 2: Core Controllers / Add-ons
CNI enhancements (Calico/Cilium if used)
EBS CSI / EFS CSI
metrics-server
ingress controller / ALB controller
cert-manager
DNS controller (optional)

# Layer 3: GitOps Control Plane
ArgoCD (or Flux)

Once ArgoCD exists, it becomes the source of truth.

# Layer 4: Security / Governance
Kyverno / Gatekeeper
NetworkPolicies
RBAC standardization
Pod security controls
Audit configs

# Layer 5: Shared Platform Services
Observability (Prometheus/Grafana/Loki)
Secrets management
Logging
Service mesh (if used)

# Layer 6: Applications
Namespaces
Databases / Stateful workloads
Backend services
Frontend