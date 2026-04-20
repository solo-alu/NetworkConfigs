# Lab - DHCP Snooping (Day 50)

## 🎯 Objective

Configure DHCP snooping on SW1 and SW2 to protect the network against rogue DHCP servers. Configure trusted and untrusted ports correctly, troubleshoot why PC1 fails to get an IP address after the initial configuration, and identify the role of DHCP Option 82 (the information option) in causing the issue.

---

## 📋 Lab Tasks

- Configure R1 as a DHCP server with a pool for the 192.168.1.0/24 network, excluding the first 9 addresses
- Enable DHCP snooping globally on both SW1 and SW2
- Enable DHCP snooping for VLAN 1 on both switches
- Configure the uplink interfaces facing R1 as trusted ports on both switches
- Attempt to get an IP address on PC1 using ipconfig /renew — observe that it fails
- Use simulation mode to trace why the DHCP message is being dropped at SW1
- Identify that SW2 is adding DHCP Option 82 (information option) to the message and SW1 is dropping it on an untrusted port
- Disable the information option on both switches to fix the issue
- Verify PC1 successfully receives an IP address after the fix

---

## ⚙️ Device Configurations

- [SW1-config.txt](./SW1-config.txt)
- [SW2-config.txt](./SW2-config.txt)

---

## ✅ Commands Used

```
! --- R1 DHCP Server Configuration ---
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.9
R1(config)# ip dhcp pool POOL1
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1

! --- SW1 DHCP Snooping Configuration ---

! Enable DHCP snooping globally
SW1(config)# ip dhcp snooping

! Enable DHCP snooping for VLAN 1 (only VLAN in use)
SW1(config)# ip dhcp snooping vlan 1

! Configure the uplink interface toward R1 as trusted
SW1(config)# interface g0/2
SW1(config-if)# ip dhcp snooping trust

! Disable Option 82 (information option) — required on switches
! that are NOT acting as DHCP relay agents
SW1(config)# no ip dhcp snooping information option

! --- SW2 DHCP Snooping Configuration ---

SW2(config)# ip dhcp snooping
SW2(config)# ip dhcp snooping vlan 1

! Configure the uplink interface toward R1 as trusted
SW2(config)# interface g0/1
SW2(config-if)# ip dhcp snooping trust

! Disable Option 82 on SW2 as well
SW2(config)# no ip dhcp snooping information option

! --- Optional: Rate limit DHCP messages on untrusted ports ---
! (Protects against DHCP starvation attacks)
SW1(config-if)# ip dhcp snooping limit rate 15

! --- Verification ---
SW1# show ip dhcp snooping
SW1# show ip dhcp snooping binding
SW1# show ip dhcp snooping statistics
```

---

## 🔍 Issues / Notes

- After the initial configuration PC1 could not get an IP address even though the trusted ports were correctly configured — this is a very common real-world gotcha
- The root cause was that SW2 automatically added DHCP Option 82 (the information option) to PC1's DHCP Discover message — when SW1 received this message on an untrusted port it dropped it as a security measure
- The fix is to run no ip dhcp snooping information option on both switches — this option should only be enabled on a switch that is also acting as a DHCP relay agent
- By default all ports are untrusted — only uplink ports facing the legitimate DHCP server should be set as trusted
- Untrusted ports will drop any DHCP Offer or DHCP Ack messages since those should only come from a trusted DHCP server — this is what blocks rogue DHCP servers
- The DHCP snooping binding table is built automatically as clients receive IP addresses — it records MAC address, IP address, VLAN, and interface, and is used by Dynamic ARP Inspection (DAI) in the next lab

