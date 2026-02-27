# Services

> What's the point of a homelab without services? Here's everything running across the Cozyplace infrastructure — spread across three hosts and a Kubernetes cluster.

---

## Overview

![Services Overview](../assets/services.png)

Services are distributed across **Proxmox PVE-01**, **Proxmox PVE-02**, and a **Raspberry Pi 5**, with a Talos Kubernetes cluster running on PVE-01 for container-native workloads.

---

## Services by Host

### Proxmox PVE-01 (Minisforum MS-01)

The primary workhorse — runs most of the infrastructure.

| Service | Category | Purpose |
|---------|----------|---------|
| Grafana | Observability | Dashboards & visualization |
| Prometheus | Observability | Metrics collection & alerting |
| InfluxDB | Observability | Time-series data storage |
| Technitium | DNS | Self-hosted DNS server |
| Traefik | Reverse Proxy | Routes traffic to internal services |
| GitLab CE | Git & CI/CD | Source code management & pipelines |
| Harbor | Registry | Container image registry |

#### Talos Kubernetes Cluster (on PVE-01)

| Service | Purpose |
|---------|---------|
| Onetimesecret | Secure, self-destructing secret sharing |
| Hugo | Static site hosting |
| *More planned* | Cluster is ready for additional workloads |

### Proxmox PVE-02 (Beelinq EQR6)

A focused, secondary node for media and proxying.

| Service | Category | Purpose |
|---------|----------|---------|
| Traefik | Reverse Proxy | Second proxy instance |
| Jellyfin | Media | Media streaming server |
| PostgreSQL | Database | Backend database for services |

### Raspberry Pi 5

Lightweight services that don't need heavy compute.

| Service | Category | Purpose |
|---------|----------|---------|
| it-tools | Utilities | Developer toolbox (converters, generators, etc.) |
| Shlink | Utilities | Self-hosted URL shortener |

---

## Service Categories at a Glance

### Observability Stack
- **Grafana** + **Prometheus** + **InfluxDB** — Full metrics pipeline with dashboards, alerting, and long-term storage.

### DNS & Reverse Proxies
- **Technitium** — Internal DNS resolution and ad blocking.
- **Traefik** (x2) — Reverse proxy running on both PVE-01 and PVE-02 for redundancy and traffic routing.

### Git & CI/CD
- **GitLab CE** — Self-hosted Git with built-in CI/CD pipelines.
- **Harbor** — Container registry for storing and scanning Docker images.

### Media
- **Jellyfin** — Free, open-source media streaming (movies, TV, music).

### Utilities
- **it-tools** — A handy web-based developer toolbox.
- **Shlink** — Short URL service, self-hosted.
- **Onetimesecret** — Share secrets securely with auto-expiring links.

### Web
- **Hugo** — Static site generator, served from the Kubernetes cluster.

---

## Deployment Strategy

<!-- Describe how you deploy your services: Docker Compose, Kubernetes, LXC, etc. -->

*Coming soon.*

---

[Back to Home](../README.md)
