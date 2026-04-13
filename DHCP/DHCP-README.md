# Lab - DHCP (Dynamic Host Configuration Protocol)

## 🎯 Objective

Configure a router as a DHCP server to dynamically assign IP addresses to hosts, configure a router interface as a DHCP client, and set up a DHCP relay agent to forward DHCP requests across different subnets.

---

## 📋 Lab Tasks

- Configure R2 as a DHCP server with separate pools for each network
- Set excluded address ranges to reserve addresses for network devices
- Configure R1's G0/0 interface as a DHCP client
- Configure R1 as a DHCP relay agent to forward requests to R2
- Verify PCs receive IP addresses automatically

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)
- [R2-config.txt](./R2-config.txt)

---

## ✅ Verification Commands Used

```
! Configure DHCP server pools
R2(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R2(config)# ip dhcp pool POOL1
R2(dhcp-config)# network 192.168.1.0 255.255.255.0
R2(dhcp-config)# default-router 192.168.1.1
R2(dhcp-config)# dns-server 8.8.8.8
R2(dhcp-config)# domain-name jeremysitlab.com

! Configure a router interface as a DHCP client
R1(config-if)# ip address dhcp

! Configure DHCP relay agent
R1(config-if)# ip helper-address [DHCP server IP]

! Verify DHCP
R2# show ip dhcp binding
R2# show ip dhcp pool
R2# show ip dhcp server statistics
```

---

## 🔍 Issues / Notes

- 

