# Kubernetes Basics

## Exercise 01 - First Pod Manifest

**Goal:** Create a Kubernetes pod declaratively using a YAML manifest.

**Specs:**
- Name: `nginx-pod`
- Image: `nginx:alpine`
- Label: `app=nginx`
- Namespace: `default`
- Container port: `80`

**Approach:** Written as YAML manifest rather than imperatively with `kubectl run`, to practice manifest structure from the start.

---

### Mistakes & Lessons Learned

| # | Mistake | Error message | Fix |
|---|---------|---------------|-----|
| 1 | Missing `name` field under `metadata` | `error: resource may not be empty` | Added `name: nginx-pod` under `metadata` |
| 2 | Used `label:` instead of `labels:` | `unknown field 'metadata.label'` | Corrected key name to `labels:` |
| 3 | Used array syntax for `labels` (`- app: nginx`) | `cannot unmarshal array into Go struct field...` | Changed to dictionary syntax (`app: nginx`) |
| 4 | Missing `ports` section | Verification failed | Added `ports.containerPort: 80` under `spec.containers` |
| 5 | Tried to update running pod with port change | `pod updates may not change fields other than...` | Deleted pod and recreated it |

---

### Key Takeaways

- A pod manifest requires at minimum: `apiVersion`, `kind`, `metadata` (with `name`), and `spec`
- `labels` is a **dictionary** (key-value pairs), not an array
- `containerPort` goes under `spec.containers[].ports[]`
- Some pod fields (like `ports`) are **immutable** on a running pod - you must delete and recreate it (`kubectl delete pod <name> -> `kubectl apply -f`)
- `kubectl apply -f` can be used for both creating and updating resources

---

### Files
- [`nginx-pod.yaml`](./nginx-pod.yaml) - final working manifest
