# Networking

> A well-designed network is the backbone of any homelab. Here's how Cozyplace is wired up — four isolated segments, strict firewall rules, and zero trust between zones.

---

## Architecture

The network is built around a Ubiquiti stack, providing a unified management plane for routing, switching, and firewall policies. Traffic between segments is tightly controlled — every flow is explicitly allowed or denied.

![Network Architecture](../assets/network_architecture.png)

---

## Network Segments

Four `/16` subnets divide the network by trust level and purpose:

| Segment | Subnet | Purpose | Internet |
|---------|--------|---------|----------|
| **Home** | `10.10.0.0/16` | Personal devices, NAS, VPN | Yes |
| **Lab** | `10.20.0.0/16` | Proxmox hosts (PVE-01, PVE-02), Raspberry Pi | Yes |
| **IoT** | `10.30.0.0/16` | TV, smart lights, printer | Yes (restricted) |
| **DMZ** | `10.40.0.0/16` | Public-facing VMs, reverse proxies (Traefik) | Yes |

### Inter-Segment Firewall Rules

| From \ To | Home | Lab | IoT | DMZ |
|-----------|------|-----|-----|-----|
| **Home** | — | Limited | Blocked | Blocked |
| **Lab** | Limited | — | Limited/Blocked | Full |
| **IoT** | Blocked | Limited/Blocked | — | Limited |
| **DMZ** | Blocked | Full | Limited | — |

The key philosophy: **IoT is untrusted and isolated**, the **Lab and DMZ talk freely** to each other, and **Home stays protected** from everything except selective Lab access.

---

## Topology

A physical/logical view of what lives in each segment:

![Network Topology](../assets/network_topology.png)

| Segment | Devices |
|---------|---------|
| Lab (`10.20.0.0/16`) | PVE-01, PVE-02, Raspberry Pi 5 |
| Home (`10.10.0.0/16`) | NAS, personal devices |
| IoT (`10.30.0.0/16`) | TV, smart lights |
| DMZ (`10.40.0.0/16`) | Proxmox VM running Traefik (public-facing reverse proxy) |

---

## Key Design Decisions

- **VLAN Segmentation** — Four distinct VLANs keep traffic isolated by trust level. IoT devices never touch the Home or Lab network directly.
- **Ubiquiti as the Core** — The DreamRouter 7 and USW Pro Max 16 provide a solid, centrally managed backbone with traffic visibility, firewall rules, and VLAN tagging all in one place.
- **FRITZ! for Wi-Fi** — The FRITZ!Box 7530 AX and Repeater 1200 AX handle Wi-Fi coverage in mesh mode. Reliable, easy to manage, and great range for the space.
- **DMZ Isolation** — Internet-facing services live in the DMZ on their own subnet. Traefik acts as the single entry point, keeping internal services off the public internet.
- **DNS via Technitium** — Running a self-hosted DNS server in the Lab for internal resolution and ad blocking.

---

## Tips & Gotchas

<!-- Add your networking lessons here as you go -->

- *Coming soon: firewall rule examples, VLAN configuration walkthrough, and Technitium DNS setup.*

---

[Back to Home](../README.md)
