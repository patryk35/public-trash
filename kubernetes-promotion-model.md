# Kubernetes changes promotion model

## Repository structure

```
├── kube-configuration-repository
│   ├── some-helm-application
│   │   ├── base
│   │   │   ├── ingress-route.yaml
│   │   │   ├── kustomization.yaml
│   │   │   ├── namespace.yaml
│   │   ├── values
│   │   │   ├── values.yaml
│   │   │   └── variants
│   │   │       ├── values.develop.yaml
│   │   │       ├── values.extensions.yaml
│   │   │       ├── values.non-prod.yaml
│   │   │       └── values.prod.yaml
│   │   ├── components
│   │   │   └── default-patches
│   │   │   |   ├── kustomization.yaml
│   │   │   |   └── network-policies.yaml
│   │   │   └── non-prod-resources
│   │   │       ├── kustomization.yaml
│   │   │       └── network-policies.yaml
│   │   └── envs
│   │       └── local
│   │           ├── kustomization.yaml
│   │           └── values.yaml
│   ├── some-non-helm-application
│   │   ├── base
│   │   │   ├── ingress-route.yaml
│   │   │   ├── kustomization.yaml
│   │   │   ├── namespace.yaml
│   │   ├── components
│   │   │   └── default-patches
│   │   │   |   ├── kustomization.yaml
│   │   │   |   └── network-policies.yaml
│   │   │   └── non-prod-resources
│   │   │       ├── kustomization.yaml
│   │   │       └── network-policies.yaml
│   │   └── envs
│   │       └── local
│   │           ├── kustomization.yaml
│   ├── some-community-non-helm-application
│   │   ├── base
│   │   │   ├── ingress-route.yaml
│   │   │   ├── kustomization.yaml
│   │   │   ├── namespace.yaml
│   │   │   └── community-sources
│   │   │       ├── README.MD
│   │   │       └── v0.0.1
│   │   │       |   └── ...
│   │   │       └── v0.0.2
│   │   │           └── ...
│   │   ├── components
│   │   │   └── default-patches
│   │   │       ├── kustomization.yaml
│   │   │       └── network-policies.yaml
│   │   └── envs
│   │       └── local
│   │           ├── kustomization.yaml

```
## Assumptions

1. Repository is continuously monitored by ArgoCD to keep cluster state up to date. ArgoCD is configured to monitor all directories `*\envs\{env_name}` for specific env.
2. Everyting is managed by Kustomize, cause:
  - Kustomize allows to work on top of Helm Charts, Kuztomization configuration, and plain Kubernetes manifests
  - Kustomize can be used out of the box. With Kustomize you can simply deploy changes locally as well as with ArgoCD or FluxCD, etc.
3. Promotion between envs is done by operations on files/directories or configuring `envs/{env_name}/kustomization.yaml` files
4. By design branches are not used to promote changes
5. You can use branches as an additional point of privileges control and changes verification
6. A single commit can change only a single env or a single group of envs (all PRODUCTION envs or all NON-PRODUCTION envs)
7. Separate branches for PRODUCTION envs (if used) should:
   - block direct user commits and force PR review
   - force running automatic code validation, checking required policies, and running other quality/security scans
   - allow direct commits by authorized bots/mechanisms
8. Direct commits by authorized bots/mechanisms:
  - should be done only when other options are not possible or cannot be done without adding complexity
  - should change only PRODUCTION configuration (to minimalize the risk of conflicts)
  - changes should be merged to "default" as soon as possible (e.g. using the cron mechanism to check if changes were done, then merge to "default" branch or alert about conflicts)
  - example: deploy pipeline changes image version on PRODUCTION branch for specific application to trigger ArgoCD Sync operation
9. Direct committing to the "default" branch should be blocked (PR with all checks and review required) or at least verified with automated steps
10. Deploy to the PRODUCTION branch(es) is possible only from the "default" branch (all commits must be merged to the "default" branch first)
11. Changes regarding PROD environments must be labeled “PRODUCTION” in PRs during merging to develop. It should protect against merging to the "default" branch commits changing PRODUCTION that are not yet ready to be deployed to PRODUCTION   
12. To apply all these rules use repo scope mechanisms like branch protection rules, hooks, GH Actions, and other 
13. To allow more flexibility in specific cases (e.g. test changes) use a mechanism to temporarily override ArgoCD Application source branch (max. 24h, only for a single app, not whole repository)

### Branching model

1. Use only 2 branches `master` and `develop`. 
  - `master` -> protected branch, changes affect only on PRODUCTION environments
  - `develop` -> default branch, changes affect only on NON-PRODUCTION environments
2. (alternative option for addons) Use a single branch for all projects NON-PRODUCTION envs and separate branches for each project PRODUCTION env
  - `develop` or `master` ->  default branch, changes affect only on NON-PRODUCTION environments
  - `release/{project_name}` -> protected branch, changes affect only on PRODUCTION environments for a specific project

## Changes promotion procedure [TODO]

### Assumptions [TODO]


## Suggested Pull Requests Actions (+ git pre-commit hook locally) 

### Preview changes [TODO]

1. Run `kustomize build` for all changed apps (all envs)
2. For all changed apps (all envs -> develop, only PROD env for -> master) run argocd diff and add as PR comment (e.g. https://codefresh.io/blog/argo-cd-preview-diff/)

### Custom checks [TODO]

1. Helm Capabilities.APIVersions verification (it is performed in helm template, so kustomize and argocd don’t support it; it can be problematic)
2. Check if there are any PROD changes. When true check if the label Production is added to PR (or “[PRODUCTION]” in PR name) 

### Quality check [TODO]

1. Kubernetes Linter
2. Other tools

### Security check [TODO]

1. Trivy/Snyk manifests scan
2. Docker images vulnerabilities scan
3. (addons repo) Check if newer Helm charts (and/or Docker images) are available

### Verify policies compliance [TODO]

1. Kyverno static scan?
2. Other policy verification tools?

### Manual Review (required for PROD)

