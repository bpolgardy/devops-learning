# Kubernetes Deployments

## Exercise 01 - First Deployment Manifest

**Goal:** Create a Kubernetes Deployment declaratively using a YAML manifest.

**Specs:**
- Name: `nginx-deploy`
- Image: `nginx:1.19-alpine`
- Namespace: `default`
- Replicas: `1`
- Labels: `app=nginx-deploy`

**Approach:** Initially written as a declarative YAML manifest from scratch. After validation failed, used `kubectl create deployment --dry-run=client -o yaml` to identify the missing fields, then corrected the original manifest.

---

### Mistakes & Lessons Learned

| # | Mistake | Symptom | Fix |
|---|---------|---------|-----|
| 1 | Missing `labels` on the Deployment object itself (only had labels on the pod template) | Validation failed despite deployment running fine | Added `app: nginx-deploy` under `metadata.labels` at the Deployment level |
| 2 | Missing explicit `replicas` field (relied on Kubernetes default of 1) | Potential validation mismatch | Added `replicas: 1` explicitly under `spec` |
| 3 | Container named `nginx-container` instead of `nginx` | Validation failed | Renamed container to `nginx` |

---

### Key Takeaways

- **Deployment-level labels ≠ Pod template labels** — `metadata.labels` labels the Deployment object itself; `spec.template.metadata.labels` labels the Pods it creates. Both may be required.
- **Use `--dry-run=client -o yaml` as a scaffold** — `kubectl create deployment <name> --image=<img> --dry-run=client -o yaml` generates a spec-compliant starting point quickly
- **Be explicit, even when defaults would suffice** — field-level matching means that stating `replicas: 1` explicitly is safer than omitting it
- **"Running correctly" ≠ "meeting spec requirements"** — a workload can be healthy while still failing validation against specific metadata or field expectations

---

### Files
- [`nginx-deploy.yaml`](./nginx-deploy.yaml) - final working manifest

