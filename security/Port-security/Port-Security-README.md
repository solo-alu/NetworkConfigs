# Lab - Port Security (Day 49)

## 🎯 Objective

Configure port security on SW1 and SW2 using different violation modes, MAC address learning methods, and maximum MAC address limits. Trigger security violations on both switches and observe how each violation mode responds. Practice recovering a port from the err-disabled state.

---

## 📋 Lab Tasks

- Configure port security on SW1 access ports using the shutdown violation mode with a static MAC address
- Configure port security on SW2's uplink port using the restrict violation mode with sticky MAC address learning and a maximum of 4 secure MAC addresses
- Ping R1 from each PC to populate the sticky MAC table on SW2
- Verify sticky MAC addresses were learned and saved to running-config
- Trigger a violation on SW2 by configuring a VLAN 1 SVI on SW1 and pinging R1 — the new source MAC causes a violation
- Trigger a violation on SW1 by changing PC1's MAC address in Packet Tracer
- Observe that SW2 uses restrict mode (drops traffic, increments counter, port stays up) and SW1 uses shutdown mode (port goes err-disabled)
- Recover the err-disabled port on SW1 using shutdown and no shutdown
- Verify port security status on both switches throughout the lab

---

## ⚙️ Device Configurations

- [SW1-config.txt](./SW1-config.txt)
- [SW2-config.txt](./SW2-config.txt)

---

## ✅ Commands Used

```
! Port must be an access port before enabling port security
SW1(config-if)# switchport mode access

! Enable port security on the interface
SW1(config-if)# switchport port-security

! Set the maximum number of allowed MAC addresses (default is 1)
SW1(config-if)# switchport port-security maximum 1

! Statically define an allowed MAC address
SW1(config-if)# switchport port-security mac-address [mac-address]

! Set violation mode to shutdown (default - port goes err-disabled on violation)
SW1(config-if)# switchport port-security violation shutdown

! Enable sticky MAC learning (dynamically learns and saves MACs to running-config)
SW2(config-if)# switchport port-security mac-address sticky

! Set maximum secure MAC addresses (SW2 uplink allows 4)
SW2(config-if)# switchport port-security maximum 4

! Set violation mode to restrict (drops traffic, increments counter, port stays up)
SW2(config-if)# switchport port-security violation restrict

! Set violation mode to protect (drops traffic silently, no counter, port stays up)
SW(config-if)# switchport port-security violation protect

! Recover an err-disabled port manually
SW1(config-if)# shutdown
SW1(config-if)# no shutdown

! Configure automatic errdisable recovery for port security violations
SW1(config)# errdisable recovery cause psecure-violation
SW1(config)# errdisable recovery interval 30

! Verify port security on all interfaces
SW1# show port-security

! Verify port security on a specific interface
SW1# show port-security interface [interface]

! View all secure MAC addresses on the switch
SW1# show port-security address

! Check sticky MACs saved to running config
SW2# show running-config interface [interface]

! Verify MAC address table
SW2# show mac address-table
```

---

## 🔍 Issues / Notes

- Port security only works on access ports or manually configured trunk ports — it will not work on a port in dynamic auto or dynamic desirable mode
- Sticky MAC addresses show as type "Static" in the MAC address table even though they were dynamically learned — this is expected behavior
- SW2's restrict mode keeps the port up and just drops traffic from unknown MACs while incrementing the violation counter — this is useful when you don't want to disrupt the whole port
- SW1's shutdown mode immediately puts the port into err-disabled state — the port LED goes off and all traffic stops
- To recover from err-disabled you must run shutdown then no shutdown on the interface — just no shutdown alone will not work
- Sticky MAC addresses are only saved permanently if you run copy running-config startup-config — otherwise they are lost on reboot
- Changing a PC's MAC address in Packet Tracer is a quick way to simulate a rogue device and test violation behavior

