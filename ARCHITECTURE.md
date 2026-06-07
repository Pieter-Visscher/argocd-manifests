# Architecture

## Goal

Central GitOps repository for all application deployments on the homelab Kubernetes cluster. ArgoCD watches this repository and reconciles the cluster state to match what is defined here.

## Design

ArgoCD is the deployment engine. Each application lives in its own subdirectory and is managed as an ArgoCD `Application` resource. The Kubernetes resources for those `Application` objects themselves are provisioned by Terraform (in the `talos-cluster` repo via the `apps` module).

Most applications use Kustomize overlays to assemble their manifests. Where upstream Helm charts are not suitable, raw YAML manifests are used directly.

## Applications

| Directory | Application |
|---|---|
| `awx` | AWX — Ansible automation platform |
| `dawarich` | Dawarich — self-hosted location history tracker |
| `forgejo` | Forgejo — self-hosted Git server |
| `grafana` | Grafana — metrics dashboards |
| `home-assistant` | Home Assistant — home automation |
| `immich` | Immich — self-hosted photo library |
| `intel-device-plugin` | Intel GPU/device plugin for Kubernetes |
| `kubevirt-manager` | kubevirt-manager — web UI for KubeVirt VMs |
| `metrics` | metrics-server for Kubernetes resource metrics |
| `multus` | Multus CNI — multiple network interfaces on pods |
| `network` | Gateway API resources, TLS certificates, Cloudflare cert-manager config |
| `nextcloud` | Nextcloud — self-hosted file sync and collaboration |
| `omni-tools` | omni-tools — collection of self-hosted web utilities |
| `opa` | Open Policy Agent — policy enforcement |
| `paperless-ngx` | Paperless-ngx — document management |
| `pieter-fish` | pieter.fish — personal website |
| `powerdns` | PowerDNS — authoritative DNS server |
| `prometheus` | Prometheus — metrics collection |
| `prometheus-operator` | Prometheus Operator — CRD-based Prometheus management |
| `shadowbroker` | shadowbroker |
| `vikunja` | Vikunja — self-hosted task/todo manager |
| `woodpecker-ci` | Woodpecker CI — lightweight CI/CD pipeline runner |
| `zot-registry` | Zot — OCI-compliant container image registry |

## Repository Structure

```
argocd-manifests/
├── <app>/
│   ├── kustomization.yaml   # Kustomize overlay
│   └── *.yaml               # Kubernetes manifests
├── network/                 # Gateway, routes, TLS certs
└── postgres.yml             # Shared PostgreSQL manifest
```

## Secrets

Application secrets are stored separately in the `argocd-secrets` repository as encrypted YAML files. ArgoCD decrypts and applies them at deploy time.
