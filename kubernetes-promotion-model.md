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

1. Promotion between envs is done by operations on files/directories or configuring `envs/{env_name}/kustomization.yaml` files
2. By design branches are not used to promote changes
3. You can use branches as an additional point of privileges control and changes verification
4. A single commit can change only single env or single group of envs (all PRODUCTION envs or all NON-PRODUCTION envs)
5. Separate branches for PRODUCTION envs (if used) should:
   - block direct user commits and force PR review
   - force running automatic code validation, checking required policies and running other quality/security scans
   - allow direct commits by authorized bots/mechanisms
6. 


### Branching model

1. Use only 2 branches `master` and `develop`. 
  - `master` -> only PROD configuration
  - `develop` -> only NON-PROD configuration
2. (alternative option for addons) Use a single branch for all projects NON-PROD envs and separate branches for each project PRODUCTION env
  - `develop` or `master` ->  only NON-PROD configuration
  - `release/{project_name}` -> PROD configuration for specific {project_name}

## Changes promotion procedure

### Assumptions



    
