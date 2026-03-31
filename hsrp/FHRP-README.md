# First Hop Redundancy Protocols (FHRP)

First Hop Redundancy Protocols solve a critical problem in network design — if a host's default gateway goes down, it loses access to the rest of the network. FHRPs solve this by using a **virtual IP and MAC address** shared between two or more routers, so if the active router fails, a standby router takes over automatically with no impact on end hosts.

The three main FHRPs covered in the CCNA are:
- **HSRP** (Hot Standby Router Protocol) — Cisco proprietary
- **VRRP** (Virtual Router Redundancy Protocol) — open standard
- **GLBP** (Gateway Load Balancing Protocol) — Cisco proprietary, also load balances

---

## 📖 Key Concepts Covered

- Virtual IP and virtual MAC address operation
- HSRP Active and Standby router election (priority and preemption)
- HSRP versions 1 vs 2
- VRRP Master and Backup router roles
- GLBP and how it differs from HSRP/VRRP
- Preemption and why it matters for failover

---

## 🗂️ Labs

| Lab | Topic | Status |
|-----|-------|--------|
| [Lab 01](./lab01-hsrp/) | HSRP Configuration | ✅ Done |

---

## 💡 Commands Reference

```
! Configure HSRP on an interface
Router(config-if)# standby version 2
Router(config-if)# standby 1 ip 192.168.1.254
Router(config-if)# standby 1 priority 110
Router(config-if)# standby 1 preempt

! Verify HSRP
Router# show standby
Router# show standby brief

! Configure VRRP on an interface
Router(config-if)# vrrp 1 ip 192.168.1.254
Router(config-if)# vrrp 1 priority 110
Router(config-if)# vrrp 1 preempt

! Verify VRRP
Router# show vrrp
Router# show vrrp brief
```

---

## 🔍 Things I Learned / Struggled With

- The virtual IP must be in the same subnet as the router interfaces but cannot be assigned to any real interface
- Higher priority wins the Active/Master election — default priority is 100
- Without preemption configured, a recovered router won't automatically take back the Active role even if it has a higher priority
- HSRP and VRRP both use one active gateway while GLBP can actively load balance across multiple routers simultaneously

---

## 📎 Resources

- [Jeremy's IT Lab - Day 29 FHRP](https://www.youtube.com/@JeremysITLab)
- [Cisco HSRP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipapp_fhrp/configuration/xe-16/fhp-xe-16-book.html)
