## Health-probes policy

Kubernetes does NOT know:
- App is dead
- App is ready

App crashes → Kubernetes thinks it's running ❌
Traffic goes → failed app ❌


In real systems (like microservices):

- DB takes time to start
- App needs warmup
- Dependencies load slowly


# Real Use in Amazon EKS

Companies enforce this because:

✔ Prevent downtime
✔ Auto-healing
✔ Better scaling
✔ Safer deployments

# Real Use in Amazon EKS

Companies enforce this because:

✔ Prevent downtime
✔ Auto-healing
✔ Better scaling
✔ Safer deployments

-------------------------------------------------------------------------

# require-resource-limits policy
--------------------

1. What Problem This Solves?

Without resource limits:

One container can consume ALL CPU / Memory ❌


❌ Without Limits
App A uses too much memory
        ↓
App B crashes ❌
        ↓
Whole node unstable ❌


# What is "?*" ?
Kyverno wildcard = "must exist"

👉 We are NOT checking value
👉 Only checking presence ✅


# Real Use in Amazon EKS

Companies enforce this because:

✔ Prevent noisy neighbor problem
✔ Better scheduling
✔ Cost optimization
✔ Cluster stability


------------------------------------------------------------------------------------------
# require-run-as-non-root policy
--------------------------------------

What Problem This Solves?

By default:

Containers run as root ❌
❌ Risk of Root User

If container is hacked:
→ attacker gets root access ❌
→ can control node ❌
→ full cluster compromise ❌

✅ With Non-Root
App runs as limited user ✅
Even if hacked → damage is limited ✅

# FINAL UNDERSTANDING
This policy ensures:
→ Containers do NOT run as root
→ Security risk reduced
→ Production compliance maintained


-----------------------------------------------------------------------------------------------
# Namespace policy


What Problem This Solves?

Without this:

Anyone can deploy anywhere ❌
❌ Bad Scenario

Dev accidentally deploys to production ❌
Testing app goes to wrong namespace ❌
Security risk ❌

✅ With Policy
Only allowed namespaces:
✔ dev
✔ qa
✔ prod

# FINAL UNDERSTANDING
This policy ensures:
→ Apps deploy only in correct environments
→ Prevents accidental production issues
→ Improves governance



======================================================

# Intall controller 
ArgoCD will:

1. Pull Helm chart from kyverno repo
2. Install it into cluster
3. Create:
   - kyverno namespace
   - kyverno pods
   - CRDs
   - admission webhooks

kubectl get pods -n kyverno

You’ll see:

kyverno-admission-controller
kyverno-background-controller
kyverno-cleanup-controller

👉 THIS = Kyverno Engine (Controller)

THEN YOUR POLICIES WORK

Now your second app:

2️⃣ kyverno-policies-app.yaml

❗ IMPORTANT DEPENDENCY

👉 Policies depend on Engine

So:

kyverno-engine → MUST deploy first
kyverno-policies → AFTER
=========================================
Git repo
   ↓
ArgoCD watches repo
   ↓
Sees kyverno-engine-app.yaml
   ↓
Installs Helm chart
   ↓
Controller runs in cluster
   ↓
Then policies applied
   ↓
Cluster gets secured

=====================================================================================

🧠 WHAT HAPPENS INTERNALLY

When you apply your root app, ArgoCD does this:

1️⃣ ArgoCD Reads Git Repo
platform-root → apps → kyverno-engine-app.yaml

👉 It sees:
👉 “Oh, I need to install a Helm chart”

2️⃣ ArgoCD Calls HELM (Internally)

ArgoCD basically runs something like:

helm repo add kyverno https://kyverno.github.io/kyverno
helm install kyverno kyverno/kyverno --namespace kyverno

👉 You didn’t run this
👉 ArgoCD did it automatically ✅

3️⃣ HELM CHART CONTENTS

That Kyverno chart contains:

Deployment (controllers)
ServiceAccounts
ClusterRoles
CRDs (ClusterPolicy etc)
Webhooks (admission controller)
4️⃣ Kubernetes Creates Resources

Cluster now has:

kubectl get pods -n kyverno

👉 Output:

kyverno-admission-controller
kyverno-background-controller
kyverno-cleanup-controller
5️⃣ CONTROLLER STARTS RUNNING

Now Kyverno is:

Watching Kubernetes API 🔍
Intercepting pod creation 🚫
Applying policies 🔐


===============================================

🎯 WHY I ADDED PDB FOR YOU

Because:

👉 Kyverno is a critical component

If it goes down:

No policy validation
No security enforcement
Cluster becomes unsafe ❌

👉 In enterprise:

Controllers MUST be highly available


============================================================

2️⃣ backgroundController 🔁

👉 Works in background (not real-time)

🧠 What problem it solves?

👉 Suppose:

You already have 100 pods running

Then you add a policy:

"All pods must have resource limits"

👉 Question:

Will old pods be checked?

👉 Without backgroundController:

NO ❌
✅ With backgroundController:
It scans existing resources 🔍
Applies policy retroactively 🔥


================================================================ 

3️⃣ cleanupController 🧹

👉 Deletes resources automatically

Example:

Delete old jobs
Delete unused resources
4️⃣ reportsController 📊

👉 Generates reports

Which pods violate policies?
Compliance status

=====================================================================
🎯 WHY I CONFIGURED ALL CONTROLLERS

Because in real enterprise setup:

Controller	Why needed
admissionController	real-time enforcement
backgroundController	scan old resources
cleanupController	auto cleanup
reportsController	compliance visibility

=================================================
# Commands 

kubectl get crd | grep kyverno

kubectl get clusterpolicy




=================================


### Remaing 

1. ReadOnly Root Filesystem

👉 Prevent container from modifying filesystem

readOnlyRootFilesystem: true

✔ Stops malware persistence
✔ Very common in production

2. Disallow HostPath
hostPath:

👉 Dangerous because:

Mounts host filesystem inside container 😨

✔ Must be blocked

3. Disallow Host Network / PID / IPC
hostNetwork: true
hostPID: true
hostIPC: true

👉 Gives access to:

Node network
Processes of other pods

✔ Block in production

4. Image Registry Restriction

👉 Allow only trusted registry:

Only allow:
- ECR
- company registry

✔ Prevents malicious images

5. Require Labels (Governance)
labels:
  app:
  env:
  owner:

✔ Used for:

cost tracking
monitoring
debugging
6. Require Resource Quotas (Namespace level)

👉 Prevents cluster overuse

7. Network Policies (VERY IMPORTANT 🔥)

👉 Control traffic between services

Example:

auth-service → can talk to DB
frontend → cannot talk to DB
8. PodDisruptionBudget (Availability)

👉 Prevent downtime during:

node restart
upgrades
9. HPA (Auto Scaling)

👉 Based on CPU/memory

10. Secrets Security

👉 Avoid plain text secrets
👉 Use:

AWS Secrets Manager
External Secrets