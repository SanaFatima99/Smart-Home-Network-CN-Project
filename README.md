
# Smart Home Automation System
### Computer Networks Project — Cisco Packet Tracer

![Network](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![VLANs](https://img.shields.io/badge/VLANs-4-green)
![IoT Devices](https://img.shields.io/badge/IoT%20Devices-14-orange)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## Project Overview
A fully functional Smart Home Network simulated in Cisco Packet Tracer, 
implementing enterprise-grade networking features including VLAN segmentation, 
IoT automation, wireless networks, security hardening, and remote access via NAT.

---

## Repository Contents
| File | Description |
|------|-------------|
| `.pkt` | Cisco Packet Tracer topology file |
| `CN_PROJECT_DOCUMENTATION.docx` | Complete project documentation |

---

## Network Architecture
- **Core Device:** Cisco Catalyst 3560-24P Multilayer Switch
- **Edge Router:** Cisco 2811 (HOMEROUTER)
- **Servers:** Server0 (DHCP/DNS/IoT/Syslog) + Web Server
- **Wireless:** 3 Access Points — SmartHomeWiFi, GuestWiFi, RemoteWiFi

---

## VLAN Design
| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | OFFICE_PCs | 192.168.2.0/24 | Home admin computers |
| 20 | IOT_DEVICES | 192.168.3.0/24 | 14 Smart home devices |
| 30 | MANAGEMENT | 192.168.1.0/24 | Servers & management |
| 40 | GUEST | 192.168.4.0/24 | Isolated guest access |

---

## IoT Automation — 13 Rules
- **Security:** Motion detection → lights, door unlock, siren
- **Climate:** Auto fan/AC based on temperature
- **Emergency:** Smoke detection → siren, window open, door unlock
- **Humidity:** Auto humidifier control
- **Ventilation:** Auto window based on temperature

---

## Security Features
- SSH v2 on all network devices
- Port Security (shutdown on violation)
- ACL-based Guest Network Isolation
- Banner MOTD on all devices
- SNMP Community Strings
- Centralized Syslog on Server0

---

## QoS Policy — SMART_HOME_QOS
| Traffic Class | Priority |
|--------------|----------|
| IOT_TRAFFIC | 50% Strict Priority |
| Home Admin | 30% |
| GUEST_TRAFFIC | 20% Limited |

---

## 🛠️ Technologies Used
`Cisco IOS` `OSPF` `VLANs` `DHCP` `DNS` `NAT` `SSH` `ACL` `QoS` `SNMP` `Syslog` `WPA2` `IoT`
