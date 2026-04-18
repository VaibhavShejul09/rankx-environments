===========================
Date: 18-04-2026
===========================

# What We Need Before First Policy
Checklist
✅ Cluster ready
✅ Calico/CNI installed
✅ Labels standardized
✅ Namespace known
✅ Traffic map ready
✅ GitOps folder created


# Production Labeling Standard
app.kubernetes.io/name
app.kubernetes.io/instance
app.kubernetes.io/component
app.kubernetes.io/part-of
app.kubernetes.io/managed-by
app.kubernetes.io/version

env
team
tier
role


# Meaning of Each Label

Label	                    Meaning
name	             App name
instance	         Helm release name
component	         backend / gateway / database
part-of	             Full system name
managed-by           Helm
version	             image tag
env	                 dev/qa/prod
team	             owner team
tier	             app/db/cache


-----------------------------------------------------------
{{- define "microservice.labels" -}}
app.kubernetes.io/name: {{ .Values.name | quote }}
app.kubernetes.io/instance: {{ .Release.Name | quote }}
app.kubernetes.io/component: {{ .Values.labels.component | quote }}
app.kubernetes.io/part-of: "rankx"
app.kubernetes.io/managed-by: "Helm"
app.kubernetes.io/version: {{ .Values.image.tag | quote }}

env: {{ .Values.labels.env | quote }}
team: {{ .Values.labels.team | quote }}
tier: {{ .Values.labels.tier | quote }}
{{- end }}

------------------------------------------------------
app.kubernetes.io/name: "auth-service"
app.kubernetes.io/instance: "auth-release"
app.kubernetes.io/component: "backend"
app.kubernetes.io/part-of: "rankx"
app.kubernetes.io/managed-by: "Helm"
app.kubernetes.io/version: "1.0.0"

env: "dev"
team: "backend"
tier: "app"

--------------------------------------------------

name: auth-service

image:
  repository: myrepo/auth-service
  tag: "1.0.0"

labels:
  component: backend
  env: dev
  team: backend
  tier: app