ArgoCD Sync
   ↓
Wave 0 → namespaces created
   ↓
Wave 1 → storage + kyverno engine
   ↓
Wave 2 → projects + policies
   ↓
Wave 3 → apps deployed


--------------------------------

# FINAL TABLE
File	                                   Sync Wave 	      Reason
namespaces-app.yaml	                           0	        base infra
storage-app.yaml	                           1	        volumes
kyverno-engine-app.yaml	                       1	        install engine
projects-app.yaml	                           2	        app permissions
kyverno-policies-app.yaml	                   2	        security rules
applicationsets-app.yaml	                   3	        deploy apps

-------------------------------------------
# WHAT HAPPENS IF WRONG ORDER?

Example:

Policies before engine ❌ → FAIL
Apps before namespace ❌ → FAIL
PVC before storage ❌ → FAIL



