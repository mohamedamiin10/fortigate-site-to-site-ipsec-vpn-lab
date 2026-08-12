# Site-to-Site VPN | HQ ↔ Branch Office

A hands-on lab implementing a **Site-to-Site IPsec VPN** between two FortiGate firewalls, connecting a Headquarters network and a Branch office network securely over the public internet (simulated with GNS3/EVE-NG, VPCS, and FortiGate VMs).

---

## 📌 Project Overview

This project simulates a real-world enterprise scenario: two geographically separated sites (HQ and Branch) need secure, encrypted connectivity over an untrusted public network. A **FortiGate-to-FortiGate IPsec Site-to-Site VPN tunnel** was configured to encrypt all traffic between the two internal LANs.

**Goals:**
- Establish a stable IPsec VPN tunnel between HQ-FW and BRNCH-1 FortiGates
- Route traffic between two separate VLANs across the tunnel
- Verify end-to-end connectivity between hosts at each site
- Confirm traffic crossing the internet is encrypted (ESP), not sent in plaintext

---

## 🗺️ Network Topology

![Topology](./screenshots/Topology.png)

| Site | Device | Role | Interface | IP |
|------|--------|------|-----------|-----|
| HQ | HQ-FW | FortiGate Firewall | port1 (WAN) | 154.16.73.2/30 |
| HQ | HQ-FW | FortiGate Firewall | port3 (LAN) | VLAN 110 Gateway |
| HQ | HQ-SW | Layer 2 Switch | Gi0/1 | — |
| HQ | User-1 | End host | VLAN 110 | 10.10.10.0/24 |
| Branch | BRNCH-1 | FortiGate Firewall | port1 (WAN) | 153.16.73.2/30 |
| Branch | BRNCH-1 | FortiGate Firewall | port3 (LAN) | VLAN 220 Gateway |
| Branch | BCH1-SW | Layer 2 Switch | Gi0/1 | — |
| Branch | User-2 | End host | VLAN 220 | 20.20.20.0/24 |
| ISP | Edge-R1 / Edge-R2 | Edge Routers | — | 192.168.144.0/24 |

**WAN transit link:** 192.168.144.0/24 (simulated internet, routed via Edge-R1 and Edge-R2)

---

## ⚙️ Configuration Summary

- **Firewalls:** FortiGate (HQ-FW, BRNCH-1)
- **Tunnel type:** Route-based IPsec Site-to-Site VPN
- **Phase 1:** Pre-shared key authentication between 154.16.73.2 (HQ-FW) and 153.16.73.2 (BRNCH-1)
- **Phase 2:** Selectors set to match internal subnets (10.10.10.0/24 ↔ 20.20.20.0/24)
- **Routing:** Static routes on both FortiGates pointing internal subnets across the VPN tunnel interface
- **Edge routers:** Basic routing configured to simulate ISP transit between the two sites

---

## ✅ Verification & Testing

### 1. VPN Tunnel Status — HQ Side
IPsec tunnel `Site to Site - FortiGate` is **up**, showing active data exchange with the Branch peer.

![HQ Tunnel Status](./screenshots/HQ.png)

### 2. VPN Tunnel Status — Branch Side
Matching tunnel status confirms the tunnel is established and passing traffic in both directions.

![Branch Tunnel Status](./screenshots/B1.png)

### 3. End-to-End Connectivity Test
Ping from **User-1** (10.10.10.2, HQ) to **User-2** (20.20.20.2, Branch) succeeds with consistent round-trip times, confirming routing across the VPN is working correctly.

![Ping Test](./screenshots/Ping.png)

### 4. Encryption Verification
A packet capture on HQ-FW's WAN interface (`port1`) shows only **ESP-encrypted** packets between the two public IPs (154.16.73.2 ↔ 153.16.73.2) — confirming that traffic crossing the public network is fully encrypted and no plaintext data is exposed.

![Encryption Test](./screenshots/testing_the_encryption.png)

---

## 🧰 Tools Used

- FortiGate Firewall (VM)
- GNS3 / EVE-NG (network emulation)
- Cisco IOS routers (Edge-R1, Edge-R2)
- VPCS (virtual PC simulator for end-host testing)
- Wireshark / FortiGate CLI sniffer (`diagnose sniffer packet`)

---

## 📂 Repository Structure

```
├── README.md
└── screenshots/
    ├── Topology.png
    ├── HQ.png
    ├── B1.png
    ├── Ping.png
    └── testing_the_encryption.png
```

---

## 🎯 Key Takeaways

- Configured a fully functional IPsec Site-to-Site VPN between two FortiGate firewalls
- Verified secure routing between two separate internal subnets across a public network
- Confirmed encryption at the packet level using ESP traffic analysis
- Strengthened hands-on skills in routing, switching, and FortiGate VPN configuration

---

*Project by Amiin — Networking & Security Student*
[LinkedIn](https://www.linkedin.com/in/mohamed-osman-ali-14a765406/)
