# Hardware

> Every homelab starts with the hardware. Here's a full breakdown of the gear that keeps Cozyplace running.

---

## Networking Gear

| Role | Device |
|------|--------|
| Router | Ubiquiti DreamRouter 7 |
| Switch | Ubiquiti USW Pro Max 16 |
| Wi-Fi AP | FRITZ!Box 7530 AX |
| Wi-Fi Extender | FRITZ!Repeater 1200 AX |

The Ubiquiti stack handles all routing, switching, and VLAN segmentation. The FRITZ! devices fill in the Wi-Fi coverage with a mesh setup — a solid, no-fuss combo for a home environment.

---

## Compute

### Beelinq EQR6 — `PVE-01`

The secondary Proxmox node. Quiet and efficient — runs Traefik, Jellyfin, and PostgreSQL.

| Spec | Details |
|------|---------|
| CPU | AMD Ryzen 7 6800U |
| RAM | 24 GB — Integrated |
| Storage | 2x500 GB SSD — Crucial P3 Plus M.2 |

### Minisforum MS-01 — `PVE-02`

The primary Proxmox host and workhorse of the lab. Runs the observability stack, GitLab, Harbor, Technitium DNS, Traefik, and a Talos Kubernetes cluster.

| Spec | Details |
|------|---------|
| CPU | 12th Gen Intel i9-12900H |
| RAM | 96 GB — 2x48 GB Crucial DDR5-5600 CL46 |
| Storage | 2x1 TB SSD — Samsung 9100 PRO M.2 |

### Raspberry Pi 5 — 8 GB — `RPI-5`

The trusty Pi — handles lightweight services like it-tools and Shlink. Low power, always on.

| Spec | Details |
|------|---------|
| RAM | 8 GB |
| Storage | 1x500 GB SSD |

---

## NAS - Storage

### UGREEN DXP4800 Plus — 4 Bay NAS

Handles bulk storage, backups, and media. Upgraded RAM makes it surprisingly capable.

| Spec | Details |
|------|---------|
| RAM | 48 GB — 2x24 GB Crucial DDR5-5600 |
| Drives | 2x8 TB Seagate IronWolf HDD + 700 GB HDD |
| Cache | 2x1 TB SSD — Samsung 990 PRO M.2 |

---

## What's Next

Hardware evolves. Future plans include:

- Adding more IronWolf drives to fill all four NAS bays
- Exploring 10 GbE networking between the MS-01 and the NAS
- Potentially adding a dedicated GPU for transcoding or AI workloads

---

[Back to Home](../README.md)
