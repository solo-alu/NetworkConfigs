# Lab 02 - Extended ACLs

## 🎯 Objective

Configure extended ACLs to filter traffic based on source IP, destination IP, protocol, and port number and apply them close to the source.

---

## 🖧 Topology

![Lab 02 Topology](./topology.png)

---

## 📋 Lab Tasks

- Configure extended numbered ACLs
- Configure extended named ACLs
- Filter traffic by protocol (TCP, UDP, ICMP)
- Filter traffic by port number (HTTP, HTTPS, FTP etc.)
- Apply ACLs close to the traffic source
- Verify ACL operation and hit counts

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)
- [R2-config.txt](./R2-config.txt)

---

## ✅ Verification Commands Used

```
show access-lists
show ip interface [interface]
show running-config | include access
```

---

## 🔍 Issues / Notes

- 

