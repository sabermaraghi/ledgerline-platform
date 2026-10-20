# GitOps design

## Flow
1. Push/merge to `ledgerline-backend` -> CI: lint, test, govulncheck, build.
2. CI pushes image `ghcr.io/<user>/<service>:<git-sha>`.
3. CI opens a PR in this repo changing the image tag in `apps/overlays/<env>`.
4. Merge -> ArgoCD detects the change -> syncs the cluster. Rollback = `git revert`.

## Components and order of installation
1. OpenTofu/Terraform: kind cluster (no default CNI, kube-proxy disabled), repo settings, emulated AWS.
2. Bootstrap (helm, once): Gateway API CRDs (pinned) -> Cilium (kubeProxyReplacement, Gateway API enabled, Hubble) -> ArgoCD.
3. ArgoCD root app syncs everything else: cert-manager, Gateway + HTTPRoutes, observability, data services, Ledgerline apps.

## Open decisions (see ledgerline-backend ADR-0003)
OpenTofu vs Terraform, AWS emulator, secrets tool, Kustomize vs Helm per component.
