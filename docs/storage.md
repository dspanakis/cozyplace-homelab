# Storage

> Data is the most valuable thing in a homelab. Here's how Cozyplace handles storage, redundancy, and backups.

---

## Storage Architecture

Storage in Cozyplace is split into two tiers based on purpose:

| Tier | Media | Location | Used For |
|------|-------|----------|----------|
| **Fast / Local** | NVMe SSDs | Mini PCs (PVE-01, PVE-02) | VM & container disks — prioritizes speed and low latency |
| **Bulk / Shared** | HDDs (RAID 1) + NVMe cache | UGREEN NAS | ISOs, media files, backups — shared across nodes via NFS/SMB |

All VMs and LXC containers run their disks directly on the local NVMe SSDs inside each Proxmox host. This keeps I/O fast and avoids network bottlenecks for day-to-day workloads.

For large, shared data — like ISOs, video files, and backups — the UGREEN NAS serves as the central store, accessible by both Proxmox nodes over the network.

---

## ZFS Configuration

Both Proxmox nodes use ZFS for local storage. See [Proxmox — Storage Configuration](proxmox.md#storage-configuration) for the per-node breakdown (RAID levels, drive details).

---

## NFS & SMB Shares

The NAS exposes shares over both NFS and SMB so that both Proxmox nodes see the same data:

- **NFS** — Used by Proxmox for ISO storage and backup targets. Mounted directly in the Proxmox storage configuration.
- **SMB** — Used for general file access from personal devices on the Home network (media, documents, etc.).

This means either node can boot a VM from a shared ISO or restore a backup without needing to copy files between hosts.

---

## Backup Strategy

Backups follow a two-layer approach, keeping data close for fast restores and off-site for disaster recovery:

```
[ PVE-01 & PVE-02 ]  →  [ UGREEN NAS ]  →  [ Hetzner Storage Box ]
   VMs & LXCs            Layer 1 (local)       Layer 2 (off-site)
```

| Layer | Target | Frequency | Purpose |
|-------|--------|-----------|----------|
| **1 — Local** | UGREEN NAS | Daily | All VMs and LXC containers are backed up to the NAS for fast restores |
| **2 — Off-site** | Hetzner Storage Box | Weekly | The NAS pushes a copy to Hetzner for disaster recovery |

The flow is straightforward: every VM and container on both Proxmox nodes gets backed up to the NAS first. From there, a weekly job syncs everything to a Hetzner Storage Box — ensuring there's always an off-site copy in case local hardware fails entirely.

---

## Tips & Gotchas

- **UGREEN NAS backup limitations** — Out of the box, the UGREEN NAS does not support file-level backups to external targets. The default tools back up in proprietary formats, which means you can't just point Hetzner (or any other backend) at individual files. A custom cronjob is needed to handle proper file-by-file syncs (e.g., via `rsync` or `rclone`). This is a form of **vendor lock-in** and something to be aware of before committing to the platform. Implementing this is still on the to-do list.

- **NFS authentication is IP-based only** — The UGREEN NAS does not support user/password authentication for NFS shares. Access control is limited to IP or host-based rules (e.g., allow `*` or a specific subnet). The GUI officially only supports NFSv3, which is inherently IP-based. NFSv4 can be enabled by manually editing config files over SSH, but there's no Kerberos support. This means anyone on an allowed IP can access the share without credentials — fine in a VLAN-segmented network, but something to keep in mind when evaluating NAS options.

---

[Back to Home](../README.md)
