# Cisco SD-WAN Implementation

## 1. Project Overview

This project focuses on the implementation of a Cisco SD-WAN solution using the Cisco Viptela architecture in an EVE-NG environment.

The objective was to build a centralized SD-WAN infrastructure connecting the Ministry of Education headquarters with three school branches located in Gabès, Sfax, and Sousse.

The solution uses centralized management and control components to establish secure communication between geographically distributed sites through MPLS and Business Internet transport networks.

## 2. Project Objectives

- Implement a Cisco SD-WAN architecture using EVE-NG.
- Deploy and configure vManage, vSmart, vBond, and vEdge devices.
- Establish MPLS and Business Internet transport networks.
- Configure underlay and overlay connectivity.
- Establish secure IPsec tunnels between sites.
- Verify routing and connectivity using OMP and ping tests.
- Monitor the network through the vManage dashboard.

## 3. Technologies and Tools

- Cisco SD-WAN (Viptela 20.6.2)
- EVE-NG
- Cisco IOL L2 switches
- Cisco IOL L3 routers
- vManage
- vSmart
- vBond
- vEdge
- MPLS
- IPsec
- OMP (Overlay Management Protocol)

## 4. Network Architecture

The topology consists of one headquarters and three remote branches.

| Site ID | Location |
|---|---|
| 10 | Ministry of Education Headquarters |
| 20 | Sfax |
| 30 | Gabès |
| 40 | Sousse |
| 100 | SD-WAN Controllers |

The architecture includes three main SD-WAN controllers:

- **vManage:** Centralized management and monitoring.
- **vSmart:** Centralized control and routing information distribution.
- **vBond:** Orchestrates the initial authentication and connection of SD-WAN devices.

The vEdge routers provide connectivity between the different sites.

## 5. Network Addressing

### 5.1 Control Network

| Device | IP Address |
|---|---|
| vManage | 192.168.137.10 |
| vSmart | 192.168.137.20 |
| vBond | 192.168.137.30 |
| vEdge HQ | 192.168.137.50 |
| vEdge Sfax | 192.168.137.60 |
| vEdge Gabès | 192.168.137.80 |
| vEdge Sousse | 192.168.137.90 |

Control network: `192.168.137.0/24`

### 5.2 Transport Networks

**MPLS Network:** `10.0.0.0/24`

| Device | IP Address |
|---|---|
| R7 | 10.0.0.1 |
| HQ | 10.0.0.2 |
| Sfax | 10.0.0.3 |
| Gabès | 10.0.0.4 |
| Sousse | 10.0.0.5 |

**Business Internet Network:** `20.0.0.0/24`

### 5.3 Internal LAN Networks

| Site | Network |
|---|---|
| HQ | 30.0.0.0/24 |
| Sfax | 40.0.0.0/24 |
| Gabès | 50.0.0.0/24 |
| Sousse | 60.0.0.0/24 |

## 6. VPN Configuration

The project uses three VPNs:

- **VPN 0:** Transport network (underlay).
- **VPN 512:** Management network.
- **VPN 10:** Service network for communication between sites.

## 7. EVE-NG Installation and Configuration

The Cisco SD-WAN virtual images were installed in EVE-NG.

Example of extracting a vSmart image:

```bash
cd /opt/unetlab/addons/qemu/vsmart-20.6.2/
tar -xzf vsmart-20.6.2-001.tgz
