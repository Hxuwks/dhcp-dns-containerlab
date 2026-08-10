# Enterprise Network Emulation Lab

A fully functional, multi-segment corporate network lab emulated locally using **Containerlab**. This project provides a hands-on environment to test routing, high availability, and network security without heavy virtualization.

## Tech Stack & Key Components

* **Orchestration:** Containerlab & Docker
* **Routing & Gateways:** FRRouting (FRR)
* **Switching:** Nokia SR Linux (VLAN trunking and bridging)
* **High Availability:** Keepalived (VRRP protocol for gateway failover)
* **Services:** `dnsmasq` (DNS resolution and DHCP server)
* **Security:** Stateful `iptables` firewall implementing strict network segmentation and *Zero Trust* policies and also `Suricata` for network monitoring.

## Architecture Overview

The topology divides the network into three distinct operational segments:

1. **Development Segment (VLAN 10):** Static addressing with full access to the server infrastructure.
2. **Server Segment (VLAN 20):** Hosts web, application, and database services (`nginx` and multitool containers).
3. **User Segment (VLAN 30):** Client computers with dynamic IP assignment via DHCP and restricted access policies.

## Project Structure & Directory Architecture

To keep configurations clean, modular, and easy to maintain, the project follows a structured directory layout. All configuration files for routers, switches, and services are separated from the main topology definition.

```text
dhcp-dns-lab/
├── topology.clab.yml       # Main Containerlab declarative topology file
└── configs/                # Directory containing device-specific configurations
    ├── frr-backup/         # Configuration files for the backup router
    │   ├── dnsmasq.conf    # DNS/DHCP configuration (if applicable)
    │   ├── frr.conf        # FRRouting routing daemon configuration
    │   └── keepalived-bk.conf # Keepalived (VRRP) backup configuration
    ├── srv-mon/            # Configuration files for suricata
    ├── frr-gateway/        # Configuration files for the primary gateway router
    │   ├── dnsmasq.conf    # dnsmasq configuration for DNS resolution and DHCP
    │   ├── frr.conf        # FRRouting routing configuration
    │   └── keepalived-gw.conf # Keepalived (VRRP) master configuration
    ├── sw-dev/             # Switch configuration for the development segment
    │   └── nokia_sw_dev.cli # Nokia SR Linux CLI commands for VLANs and bridging
    ├── sw-srv/             # Switch configuration for the server segment
    │   └── nokia_sw_srv.cli # Nokia SR Linux CLI setup
    └── sw-usr/             # Switch configuration for the user/client segment
        └── nokia_sw_usr.cli # Nokia SR Linux CLI setup
```

## Quick Start

1. Ensure you have Docker and Containerlab installed.
2. Deploy the topology:
```bash
sudo containerlab deploy -t topology.clab.yml

```
3. Inspect running nodes:
```bash
sudo containerlab inspect -t topology.clab.yml

```
