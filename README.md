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
|---------|----------|
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
|--------|------------|
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
|--------|------------|
| R7 | 10.0.0.1 |
| HQ | 10.0.0.2 |
| Sfax | 10.0.0.3 |
| Gabès | 10.0.0.4 |
| Sousse | 10.0.0.5 |

**Business Internet Network:** `20.0.0.0/24`

### 5.3 Internal LAN Networks

| Site | Network |
|------|---------|
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

### 7.1 vSmart Image Installation

Example of extracting a vSmart image:

```bash
cd /opt/unetlab/addons/qemu/vsmart-20.6.2/
tar -xzf vsmart-20.6.2-001.tgz
```

After extracting the image, EVE-NG permissions were updated:

```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### 7.2 Cisco IOL Configuration

The following commands were used to prepare the Cisco IOL license:

```bash
cd /opt/unetlab/addons/iol/bin
chmod +x CiscoKey.py
python3 CiscoKey.py
touch iourc
nano iourc
```

The generated license key was added to the `iourc` file.

## 8. Controller and vEdge Configuration

A basic system configuration template:

```text
config
system
 host-name <DEVICE_NAME>
 system-ip <SYSTEM_IP>
 site-id <SITE_ID>
 organization-name <ORGANIZATION_NAME>
 vbond <VBOND_IP>
!
commit
```

### 8.1 Transport Interface Configuration

Example configuration template for VPN 0:

```text
config
vpn 0
 interface <TRANSPORT_INTERFACE>
  ip address <TRANSPORT_IP>/<PREFIX>
  no shutdown
 !
 ip route <DESTINATION_NETWORK>/<PREFIX> <NEXT_HOP>
!
commit
```

The transport interfaces were configured to provide underlay connectivity between the SD-WAN devices.

## 9. Certificate Installation

A root certificate authority was generated on vManage.

### 9.1 Access the vManage Shell

```text
vshell
```

### 9.2 Generate the Private Key

```bash
openssl genrsa -out ROOT-CA.key 2048
```

### 9.3 Generate the Root Certificate

```bash
openssl req -x509 -new -nodes \
-key ROOT-CA.key \
-sha256 -days 3652 \
-subj "/C=TN/ST=TN/O=SOTETEL/CN=vManage.SOTETEL" \
-out ROOT-CA.pem
```

### 9.4 Transfer the Certificate to the Controllers

```text
request download scp://admin@<VMANAGE_IP>:/home/admin/ROOT-CA.pem
```

### 9.5 Install the Root Certificate

```text
request root-cert-chain install /home/admin/ROOT-CA.pem
```

### 9.6 Verify the vEdge Certificate Serial Number

```text
show certificate serial
```

**Security note:** Never upload `ROOT-CA.key`, passwords, credentials, or private keys to a public GitHub repository.

## 10. Underlay and Overlay Routing

The project uses two routing approaches:

- **Underlay:** Static routing to provide IP connectivity through the transport networks.
- **Overlay:** OMP to distribute routing information between SD-WAN devices.

IPsec tunnels were established to secure communication between the different sites.

## 11. Verification and Testing

The following commands were used to verify the SD-WAN environment.

### 11.1 Control Connections

```text
show control connections
```

### 11.2 OMP Routing Table

```text
show omp routes
```

### 11.3 Interface Status

```text
show interface
```

### 11.4 IP Routing Table

```text
show ip routes
```

### 11.5 BFD Sessions

```text
show bfd sessions
```

### 11.6 System Status

```text
show system status
```

### 11.7 Connectivity Test

```text
ping <REMOTE_SITE_IP>
```

Connectivity was tested between:

- HQ and Sfax.
- Sfax and Gabès.

The OMP routing table was also checked on vSmart to verify that routes were learned and distributed between the sites.

## 12. Monitoring with vManage

The vManage dashboard was used to monitor the SD-WAN infrastructure, including:

- Device status.
- Control connections.
- Routing information.
- IPsec tunnel status.
- TLOC information.
- Interface statistics.
- CPU and memory utilization.
- Ping and traceroute tests.
- Application-aware routing information.

## 13. Challenges and Limitations

During implementation, several challenges were encountered:

- Selecting compatible Cisco SD-WAN images for EVE-NG.
- Managing virtual machine resource consumption.
- Configuring certificates and onboarding vEdge devices.
- Resolving duplicate chassis-number issues.
- Limiting the topology to four sites due to available hardware resources.

Advanced security configurations, including additional ACL policies, were not fully implemented due to time constraints.

## 14. Conclusion

This project provided practical experience in implementing and managing a Cisco SD-WAN infrastructure.

It covered controller deployment, device onboarding, certificate installation, underlay and overlay routing, IPsec tunnels, connectivity testing, and centralized monitoring through vManage.

The implementation demonstrated how Cisco SD-WAN can provide centralized management and secure connectivity between geographically distributed sites.

## 15. References

- [Cisco SD-WAN Controllers Installation](https://networklessons.com/cisco/cisco-sd-wan/cisco-sd-wan-controllers-installation)
- [Cisco SD-WAN vEdge Onboarding](https://networklessons.com/cisco/cisco-sd-wan/cisco-sd-wan-vedge-onboarding)
- [Cisco SD-WAN Service VPN](https://networklessons.com/cisco/cisco-sd-wan/cisco-sd-wan-service-vpn)

---

## Academic Project

- **Institution:** National Engineering School of Tunis (ENIT)
- **University:** University of Tunis El Manar
- **Company Context:** SOTETEL
- **Academic Year:** 2025–2026
- **Supervisor:** Mme Meriem Kassar
