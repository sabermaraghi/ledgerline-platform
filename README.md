# ledgerline-platform

GitOps repository for **Ledgerline**: infrastructure as code and the desired state of every cluster.
Application code lives in the `ledgerline-backend` repo; this repo only describes how and where it runs.

**Status:** skeleton only. Real content arrives in Phase 4-5 (see `ledgerline-backend/docs/ROADMAP.md`).
A light "walking skeleton" (kind + ArgoCD running v0.1) is built at Day 23.

## Layout
| Path | Purpose |
|---|---|
| `terraform/modules/` | Reusable OpenTofu/Terraform modules |
| `terraform/envs/local/` | Local environment: kind cluster, GitHub settings, emulated AWS resources |
| `clusters/local/bootstrap/` | One-time bootstrap: Cilium, Gateway API CRDs, ArgoCD (the only non-GitOps step) |
| `clusters/local/apps/` | ArgoCD root "App of Apps" for the local cluster |
| `apps/base/` | Kustomize bases per service (Deployment, Service, HTTPRoute, ...) |
| `apps/overlays/local/` | Environment-specific patches (image tags, replicas, config) |
| `argocd/projects/` | ArgoCD AppProjects (RBAC boundaries) |
| `argocd/applicationsets/` | ApplicationSets generating one Application per service |
| `scripts/` | Helper scripts (bootstrap, teardown) |

## Rules
1. After bootstrap, the cluster changes only through merged PRs to `main`.
2. Image tags are bumped by automated PRs from the `ledgerline-backend` CI, never by hand.
3. Every tool version (Cilium, ArgoCD, Gateway API CRDs, OpenTofu) is pinned and recorded in `docs/VERSIONS.md`.
