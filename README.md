# SD-WAN-PFA2
# SD-WAN Implementation using EVE-NG and Cisco SD-WAN

## Overview

This project presents the design, deployment, and validation of a **Software-Defined Wide Area Network (SD-WAN)** architecture in a virtual lab environment using **EVE-NG** and Cisco SD-WAN components.

The goal was to build a scalable and centralized network connecting multiple sites while demonstrating the advantages of SD-WAN over traditional WAN architecture in terms of flexibility, management, and performance.

---

## Objectives

- Design a multi-site SD-WAN topology
- Deploy Cisco SD-WAN controllers and edge devices
- Configure secure communication between devices
- Implement underlay and overlay networking
- Configure routing and traffic exchange
- Validate end-to-end connectivity between sites
- Simulate an enterprise SD-WAN environment for testing and learning

---

## Technologies Used

- **EVE-NG** – network emulation platform
- **Cisco SD-WAN**
  - vManage
  - vBond
  - vSmart
  - vEdge
- **Cisco IOL images**
- **VMware Workstation**
- **PuTTY** – remote CLI access
- **WinSCP** – secure file transfer

---

## Architecture

The deployed architecture includes:

- **vManage** → centralized management plane
- **vBond** → orchestration and device authentication
- **vSmart** → control plane and policy distribution
- **vEdge routers** → data plane and branch connectivity

### Network Design
- Headquarter site
- Multiple branch sites
- MPLS transport
- Internet transport
- Overlay tunnels between WAN edges

---

## Features Implemented

### Underlay Network
- Static IP addressing
- Interface configuration
- Basic WAN transport connectivity

### Overlay Network
- OMP routing configuration
- TLOC establishment
- Secure tunnel creation

### Security
- Certificate installation
- Authentication between SD-WAN devices
- Secure control connections

### Validation
- Site-to-site ping tests
- Routing verification
- Tunnel status verification
- Monitoring via vManage dashboard

---


## Lab Validation

Validation tests performed include:

- Successful controller communication
- WAN Edge registration to vManage
- OMP route exchange verification
- IPsec tunnel establishment
- Inter-site connectivity tests between branches
- Dashboard monitoring through vManage

---

## Results

The implementation demonstrated that SD-WAN provides:

- Centralized network management
- Improved scalability
- Simplified deployment
- Better flexibility than traditional WAN
- Secure communication between distributed sites
- Efficient traffic management across multiple transports

---

## Learning Outcomes

Through this project I gained practical experience with:

- SD-WAN architecture and deployment
- Cisco SD-WAN ecosystem
- EVE-NG lab building
- Routing and overlay technologies
- Network troubleshooting
- Enterprise WAN design

---

## Author

**Ons Boutouta**  
2nd Year Telecommunications Engineering Student  
National Engineering School of Tunis (ENIT)

---

## Acknowledgments

Special thanks to:

- **Mrs. Meriem Kassar** – Academic Supervisor
- **Mr. Imed Jlailia** – Company Supervisor

---

## License

This project was developed for academic and educational purposes.
