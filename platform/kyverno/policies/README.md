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