# Proxmox

> The hypervisor layer that brings everything together — VMs, containers, and clustering.

---

## Why Proxmox?

Proxmox VE is a free, open-source hypervisor built on Debian with first-class support for KVM virtual machines and LXC containers. It's the natural choice for a homelab: powerful, flexible, and backed by a great community.

---

## Cluster Setup

<!-- Describe your Proxmox cluster topology here -->
<!-- e.g.: Which nodes are in the cluster? How are they connected? -->

*Coming soon.*

---

## VM & Container Strategy

The general rule is simple: **use LXC containers by default, VMs when isolation or a full kernel matters.**

- **LXC Containers** — The go-to for lightweight, single-purpose services. Grafana, Prometheus, InfluxDB, and the internal Traefik reverse proxies all run as containers. They're fast to spin up, easy to snapshot, and use minimal resources.
- **VMs for GitLab** — GitLab gets a full VM due to its complexity and resource demands. A dedicated VM gives it the breathing room it needs without affecting neighboring services.
- **VMs for DMZ Traefik** — The external-facing Traefik instance that lives in the DMZ runs as a VM to maximize isolation. Since it's internet-exposed, the extra boundary between it and the rest of the infrastructure is worth the overhead.
- **VMs for Kubernetes** — Each Talos Kubernetes node runs as its own VM. Kubernetes expects full control over the OS and networking stack, making VMs the natural fit.

---

## Storage Configuration

Each Proxmox node manages its own local ZFS storage, with different redundancy strategies based on the workload and hardware:

| Node | Drives | Filesystem | RAID | Notes |
|------|--------|------------|------|-------|
| **PVE-01** (MS-01) | 2x1 TB SSD (Samsung 9100 PRO) | ZFS | None (striped/individual) | Prioritizes capacity and speed; backups handle redundancy |
| **PVE-02** (EQR6) | 2x500 GB NVMe (Crucial P3 Plus) | ZFS | RAID-1 (mirror) | Prioritizes data safety with mirrored drives |

PVE-01 runs without RAID to maximize usable space — data protection is offloaded to the backup layers (NAS + Hetzner). PVE-02 mirrors its two NVMe drives for built-in redundancy, keeping critical services safe even if a drive fails.

---

## Backup Strategy

All VMs and LXC containers are backed up daily to the NAS, with weekly off-site copies to Hetzner Storage Boxes.

For the full breakdown, see [Storage — Backup Strategy](storage.md#backup-strategy).

---

## Tips & Gotchas

<!-- Hard-won lessons from running Proxmox -->

*Coming soon.*

---

[Back to Home](../README.md)
