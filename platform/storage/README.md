# Real Production Flow (EKS)

In Amazon EKS:

Pod starts
   ↓
PVC requested
   ↓
EBS created dynamically
   ↓
Attached to node
   ↓
Mounted in container


-------------------------

# Add storageclass.kubernetes.io/is-default-class
✅ Add this:
annotations:
  argocd.argoproj.io/sync-wave: "1"
  storageclass.kubernetes.io/is-default-class: "true"

🧠 Why?
If not default → PVC must always specify storageClass
If default → auto picked ✅


-----------

# IMPORTANT (DON’T MISS THIS)

👉 This ONLY works if:

EBS CSI Driver installed ✅