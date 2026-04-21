# Lab - Dynamic ARP Inspection (Day 51)

## 🎯 Objective

Configure Dynamic ARP Inspection (DAI) on SW1 and SW2 to protect the network against ARP poisoning attacks. DAI relies on the DHCP snooping binding table to validate ARP messages, so DHCP snooping must be configured first. Enable all three additional DAI validation checks and configure trusted ports on both switches appropriately.

---

## 📋 Lab Tasks

- Configure R1 as a DHCP server with a pool for 192.168.1.0/24 excluding the first 9 addresses
- Configure DHCP snooping on both SW1 and SW2 as a prerequisite for DAI
- Enable DAI on VLAN 1 on both SW1 and SW2
- Enable all three additional DAI validation checks (dst-mac, ip, src-mac) in a single command on both switches
- Configure trusted ports on each switch — SW2's g0/1 (connected to SW1) and SW1's g0/1 and g0/2 (connected to SW2 and R1)
- Verify DAI configuration using show commands on both switches

---

## ⚙️ Device Configurations

- [SW1-config.txt](./SW1-config.txt)
- [SW2-config.txt](./SW2-config.txt)

---

## ✅ Commands Used

```
! --- DHCP Snooping (prerequisite for DAI) ---

! SW1
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 1
SW1(config)# no ip dhcp snooping information option
SW1(config)# interface g0/2
SW1(config-if)# ip dhcp snooping trust
! SW1's g0/1 (connected to SW2) is intentionally left untrusted

! SW2
SW2(config)# ip dhcp snooping
SW2(config)# ip dhcp snooping vlan 1
SW2(config)# no ip dhcp snooping information option
SW2(config)# interface g0/1
SW2(config-if)# ip dhcp snooping trust

! --- DAI Configuration ---

! Enable DAI on VLAN 1 on SW2
SW2(config)# ip arp inspection vlan 1

! Enable all three additional validation checks in a single command
! (dst-mac, ip, and src-mac — order does not matter)
SW2(config)# ip arp inspection validate dst-mac ip src-mac

! Configure SW2's g0/1 (connected to SW1) as a DAI trusted port
SW2(config)# interface g0/1
SW2(config-if)# ip arp inspection trust

! Enable DAI on VLAN 1 on SW1
SW1(config)# ip arp inspection vlan 1

! Enable all three additional validation checks on SW1
SW1(config)# ip arp inspection validate dst-mac ip src-mac

! Configure SW1's g0/1 and g0/2 as DAI trusted ports
! (connected to SW2 and R1 respectively)
SW1(config)# interface range g0/1 - 2
SW1(config-if-range)# ip arp inspection trust

! --- Verification ---
SW1# show ip arp inspection interfaces
SW1# show ip arp inspection vlan 1
SW2# show ip arp inspection interfaces
SW2# show ip arp inspection vlan 1
SW1# show run
SW2# show run
```

---

## 🔍 Issues / Notes

- DAI depends entirely on the DHCP snooping binding table to validate ARP messages — DHCP snooping must be configured and working before DAI will function correctly
- All three validation checks (dst-mac, ip, src-mac) must be entered in a single command — entering them separately overwrites the previous entry rather than adding to it
- SW1 has two trusted DAI ports (g0/1 toward SW2 and g0/2 toward R1) because legitimate ARP traffic arrives from both directions — SW2 only needs one trusted port (g0/1 toward SW1)
- SW1's g0/1 was intentionally left untrusted for DHCP snooping but trusted for DAI — this is a deliberate design choice in the lab to show that DHCP snooping and DAI trusted ports can be configured independently
- Untrusted DAI ports validate every ARP message against the DHCP snooping binding table — if the IP-to-MAC mapping doesn't match, the ARP is dropped
- Trusted DAI ports forward all ARP messages without inspection — only configure these on ports connected to other switches or routers you fully trust
- DAI protects against ARP poisoning attacks where a rogue device sends fake ARP replies to map its MAC to another device's IP address

