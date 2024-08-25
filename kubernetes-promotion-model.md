# Kubernetes changes promotion model

## Repository structure

```shell
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
