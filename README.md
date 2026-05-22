# 🏠 Smart Home Automation System
### Computer Networks Project — Cisco Packet Tracer

![Network](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![VLANs](https://img.shields.io/badge/VLANs-4-green)
![IoT](https://img.shields.io/badge/IoT%20Devices-14-orange)
![Automation](https://img.shields.io/badge/Automation%20Rules-13-red)
![Features](https://img.shields.io/badge/Features-15%2B-purple)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## 📋 Project Overview

A fully functional Smart Home Network simulated in Cisco Packet Tracer,
implementing enterprise-grade networking features including VLAN segmentation,
IoT automation, wireless networks, security hardening, remote access via NAT,
QoS traffic management, and centralized monitoring.

> This project bridges the gap between basic home networking and professional
> enterprise-level network design using real Cisco IOS configurations.

---

## 🗂️ Repository Contents

| File | Description |
|------|-------------|
| `.pkt` | Cisco Packet Tracer — complete network topology |
| `CN_PROJECT_DOCUMENTATION.docx` | Full project documentation (65+ pages) |

---

## 🌐 Network Topology — Physical Devices

| Device | Model | Role |
|--------|-------|------|
| HOMEROUTER | Cisco 2811 | Edge routing, OSPF, NAT, internet gateway |
| Multilayer Switch0 | Cisco Catalyst 3560-24P | Core routing, VLANs, QoS, ACL |
| Switch2 | Cisco Catalyst 2950 | Access layer — home admin PCs |
| Switch-PT | Generic | Connects AP2 + Cloud0 to HOMEROUTER |
| Server0 | — | DHCP, DNS, HTTP, Syslog, IoT services (192.168.1.10) |
| Web Server | — | Hosts smart home web dashboard (192.168.1.11) |
| Access Point0 | — | SmartHomeWiFi — VLAN 20 |
| Access Point GUEST | — | GuestWiFi — VLAN 40 |
| Access Point2 | — | RemoteWiFi — external access via Cloud0 |
| Cloud0 | — | Simulates internet / ISP connection |

---

## 📡 VLAN Segmentation

| VLAN | Name | Subnet | Gateway | Purpose |
|------|------|--------|---------|---------|
| 10 | OFFICE_PCs | 192.168.2.0/24 | 192.168.2.1 | Home owner/resident PCs |
| 20 | IOT_DEVICES | 192.168.3.0/24 | 192.168.3.1 | 14 smart home IoT devices |
| 30 | MANAGEMENT | 192.168.1.0/24 | 192.168.1.1 | Servers and management |
| 40 | GUEST | 192.168.4.0/24 | 192.168.4.1 | Isolated visitor access |

- Inter-VLAN routing via SVIs on Multilayer Switch0
- `ip routing` enabled on Switch0 for Layer 3 functionality
- Trunk port Fa0/4 carries all 4 VLANs between Switch0 and Switch2

---

## 🔀 OSPF Dynamic Routing

- OSPF Process 1 on both Multilayer Switch0 and HOMEROUTER
- HOMEROUTER Router ID: `200.0.0.1` — Area 0 (Backbone)
- All 4 VLAN subnets advertised via OSPF
- Neighbor relationship verified on Vlan30 interface
- Full network reachability confirmed across all segments

---

## 🌐 DHCP & DNS — Server0

**Three DHCP Pools configured on Server0 (192.168.1.10):**

| Pool Name | Network | Gateway | DNS |
|-----------|---------|---------|-----|
| VLAN10_POOL | 192.168.2.0/24 | 192.168.2.1 | 192.168.1.10 |
| VLAN20_POOL | 192.168.3.0/24 | 192.168.3.1 | 192.168.1.10 |
| VLAN40_POOL | 192.168.4.0/24 | 192.168.4.1 | 192.168.1.10 |

- DHCP relay (`ip helper-address 192.168.1.10`) on VLAN 10 and VLAN 20 SVIs
- DNS record: `smarthome.local` → 192.168.1.10
- VLAN 30 (Management) uses static IPs — no DHCP relay needed

---

## 📶 Three Wireless Networks

| SSID | Access Point | VLAN | Security | Users |
|------|-------------|------|----------|-------|
| SmartHomeWiFi | Access Point0 | 20 | WPA2-PSK / AES / Cisco1234 | Smartphone0, IoT devices |
| GuestWiFi | Access Point GUEST | 40 | WPA2-PSK / AES / Cisco1234 | Laptop0, Laptop1 |
| RemoteWiFi | Access Point2 | — | WPA2-PSK / AES / Cisco1234 | Remote Phone |

- GuestWiFi completely isolated from internal network via ACL
- RemoteWiFi reaches Server0 through HOMEROUTER NAT and Cloud0

---

## 🤖 IoT Automation System

**14 IoT Devices registered on Server0 — all confirmed online (green status)**

**13 Automation Rules across 5 categories:**

### 🔒 Security Rules
| Rule | Condition | Actions |
|------|-----------|---------|
| motion-security | Motion detector ON | Lights ON + Door unlock + Fan ON |
| garage-alert | Garage door opens | Siren ON |

### 🌡️ Climate Control Rules
| Rule | Condition | Actions |
|------|-----------|---------|
| high-temp-cooling | Temp > 28°C | Fan HIGH + AC set to 22°C |
| low-temp-heating | Temp < 18°C | Heat ON |
| extreme-heat | Temp > 35°C | Window OPEN |

### 🚨 Smoke Emergency Rules
| Rule | Condition | Actions |
|------|-----------|---------|
| smoke-emergency | Smoke alarm = TRUE | Siren ON + Window OPEN + Door UNLOCK |
| smoke_clear | Smoke alarm = FALSE | Siren OFF + Window CLOSE |

### 💧 Humidity Rules
| Rule | Condition | Actions |
|------|-----------|---------|
| high-humidity | Humidity > 70% | Humidifier ON |
| low-humidity | Humidity < 40% | Humidifier OFF |

### 🪟 Ventilation Rules
| Rule | Condition | Actions |
|------|-----------|---------|
| ventilation-open | Temp > 35°C | Window OPEN |

---

## 🌍 Remote Access via NAT

- Static NAT on HOMEROUTER: `200.0.0.1` → `192.168.1.10` (ports 80 & 8080)
- Remote Phone → RemoteWiFi → Access Point2 → Switch-PT → HOMEROUTER → Cloud0
- NAT operates at router level, independent of VLAN structure
- Verified via `show ip nat translations`

---

## 🔒 Security Features

### SSH v2
- Configured on Switch0, Switch2, and HOMEROUTER
- RSA key generation + domain name set on all three devices
- SSH v2 enforced — Telnet disabled
- Username: `admin`

### Port Security
- Switch2 Fa0/2 (PC0) and Fa0/5 (PC1) — max 1 MAC, violation: shutdown
- Switch0 Fa0/5 (Server0) — max 1 MAC, violation: shutdown

### ACL — Guest Network Isolation
- ACL name: `BLOCK_GUEST`
- Applied inbound on VLAN 40 interface
- Blocks access to VLAN 10 (192.168.2.0/24), VLAN 20 (192.168.3.0/24), VLAN 30 (192.168.1.0/24)
- Permits all other traffic (internet access allowed)

### Banner MOTD
```
SMART HOME NETWORK - AUTHORIZED ONLY
Unauthorized access is strictly prohibited.
All activity is monitored and logged.
```
Applied on Switch0, Switch2, and HOMEROUTER.

### SNMP Monitoring
- Community string `public` — Read-Only
- Community string `private` — Read-Write
- Configured on all three network devices

### Syslog — Centralized Logging
- All three devices send logs to Server0 (192.168.1.10)
- Server0 Syslog service confirmed receiving entries
- Verified via `show run | include logging`

---

## ⚡ QoS — Traffic Priority Policy

**Policy name: SMART_HOME_QOS** — applied on Multilayer Switch0 using Cisco MQC

| Traffic Class | Match Criteria | Bandwidth |
|--------------|----------------|-----------|
| IOT_TRAFFIC | VLAN 20 (IoT devices) | 50% — Strict Priority |
| class-default | Home admin devices | 30% |
| GUEST_TRAFFIC | VLAN 40 (Guest) | 20% — Limited |

> IoT alerts and security camera feeds always get bandwidth first —
> guests can never interfere with critical smart home traffic.

---

## ⚠️ Known Limitations (Packet Tracer)

| Limitation | Details |
|------------|---------|
| SNMP traps | `snmp-server enable traps` not supported — basic community strings used |
| QoS on VLAN interfaces | `service-policy` on SVI not supported in Packet Tracer |
| Syslog timestamps | `service timestamps log` returns incomplete command error |
| Cloud0 | Simulates internet — real deployment needs actual ISP |
| Banner timing | Appears post-password in Packet Tracer — correctly configured |

---

## ✅ Feature Completion Checklist

- [x] VLAN segmentation (4 VLANs)
- [x] Inter-VLAN routing via SVIs
- [x] OSPF dynamic routing
- [x] DHCP (3 pools on Server0)
- [x] DNS (smarthome.local)
- [x] Three wireless networks (WPA2-PSK)
- [x] 14 IoT devices registered
- [x] 13 automation rules configured
- [x] Remote access via NAT
- [x] SSH v2 on all devices
- [x] Port security on all access ports
- [x] Guest network isolation via ACL
- [x] Banner MOTD on all devices
- [x] SNMP community strings
- [x] Centralized Syslog on Server0
- [x] QoS policy (SMART_HOME_QOS)
- [x] Web server hosting smart home dashboard

---

## 🛠️ Technologies & Protocols

`Cisco IOS` `OSPF Area 0` `VLANs` `802.1Q Trunking` `SVI` `DHCP` `DNS`
`Static NAT` `SSH v2` `RSA Encryption` `ACL` `Port Security` `WPA2-PSK`
`AES Encryption` `SNMP v2c` `Syslog` `MQC QoS` `IoT Automation` `HTTP`

---

## 📁 Project Structure

```
Smart-Home-Network-CN-Project/
├── Smart_Home_Automation.pkt       ← Cisco Packet Tracer topology
```

---

*Submitted as part of Computer Networks course project.*
*Simulated in Cisco Packet Tracer — configurations follow real Cisco IOS standards.*
