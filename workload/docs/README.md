## sre documentation

### production checklist
https://github.com/lububu88/bookself/blob/main/workload/docs/production-checklists.md

### 1 Repo or 2 Repo
https://github.com/lububu88/bookself/blob/main/workload/docs/1%20Repo%20or%202%20Repo.md

### pipeline workflow
https://github.com/lububu88/bookself/blob/main/workload/docs/cicd-pipeline.md

### guideline
https://github.com/lububu88/bookself/blob/main/workload/docs/guideline.md

### k8s deploymeny by helmfile

* concept:  https://github.com/lububu88/bookself/blob/main/workload/docs/kubernetes-deploy.md
## dashboard, logging, metrics

```
# Dev:
- https://monitor-dev.vnpaytest.vn (example)

# Test:
- https://monitor-test.vnpaytest.vn (example)

# Prod:
- https://monitor.vnpayapis.com (example)

```
## k8s Cluster IP ranges
```
# Dev
Ingress: HAProxy Ingress Dev
Egress: HAProxy Egress Dev

# Test
Ingress: HAProxy Ingress Test
Egress: HAProxy Egress Test

# Prod  - RKE Cluster
Ingress: HAProxy Ingress Prod
Egress: HAProxy Egress Prod
```