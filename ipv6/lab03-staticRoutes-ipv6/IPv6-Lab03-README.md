# Lab 03 - OSPFv3 for IPv6

## 🎯 Objective

Configure OSPFv3 to dynamically advertise IPv6 routes between routers and verify neighbor relationships and routing table population.

---

## 🖧 Topology

![Lab 03 Topology](./topology.png)

---

## 📋 Lab Tasks

- Configure IPv6 addresses on all router interfaces
- Enable OSPFv3 on all routers
- Assign interfaces to the correct OSPF area
- Verify OSPF neighbor adjacencies
- Verify IPv6 routing table is populated via OSPF

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)
- [R2-config.txt](./R2-config.txt)
- [R3-config.txt](./R3-config.txt)

---

## ✅ Verification Commands Used

```
show ipv6 ospf neighbor
show ipv6 ospf interface
show ipv6 route ospf
show ipv6 protocols
```

---

## 🔍 Issues / Notes

- 

