# What Happens During Sync

ArgoCD sync
   ↓
Creates:
   dev
   qa
   prod
namespaces
   ↓
Other apps can deploy safely


----------------------

# Why Labels? (IMPORTANT)
labels:
  environment: dev

👉 Used for:

✔ Kyverno policies (advanced)
✔ Network policies
✔ Monitoring filters
✔ Cost tracking


# “How do you decide ResourceQuota?”

✅ Answer:
We estimate the number of pods based on application architecture and expected load.

Then we calculate total CPU and memory using per-pod resource requests.

We add a buffer (typically 30–50%) and define ResourceQuota to prevent overconsumption while allowing scalability.