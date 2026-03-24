# OSPF (Open Shortest Path First)

OSPF is a link-state routing protocol that uses Dijkstra's algorithm to find the best path through a network. Unlike distance-vector protocols like RIP, OSPF routers build a complete map of the network topology and make routing decisions based on cost (bandwidth).

---

## 📖 Key Concepts Covered

- OSPF neighbor relationships and the DR/BDR election process
- OSPF cost calculation and how to manipulate it
- Single-area vs multi-area OSPF
- LSAs (Link State Advertisements) and the LSDB (Link State Database)
- Passive interfaces and default route propagation

---

## 🗂️ Labs

| Lab | Topic | Status |
|-----|-------|--------|
| [Lab 01](./lab01-basic-ospf/) | Basic Single-Area OSPF Configuration | ✅ Done |
| [Lab 02](./lab02-ospf-cost/) | Manipulating OSPF Cost | ✅ Done |
| [Lab 03](./lab03-ospf-multiarea/) | Multi-Area OSPF | ⏳ Not Started |

---

## 💡 Commands Reference

```
! Enable OSPF
Router(config)# router ospf 1
Router(config-router)# network 10.0.0.0 0.255.255.255 area 0

! Set router ID manually
Router(config-router)# router-id 1.1.1.1

! Make an interface passive (stops sending hellos)
Router(config-router)# passive-interface g0/1

! Manually set OSPF cost on an interface
Router(config-if)# ip ospf cost 10

! Verify OSPF
Router# show ip ospf neighbor
Router# show ip ospf interface
Router# show ip route ospf
Router# show ip protocols
```

---

## 🔍 Things I Learned / Struggled With

- OSPF neighbors won't form if hello/dead timers don't match between routers
- The router-id is chosen by: manually configured > highest loopback IP > highest active interface IP
- Cost = Reference bandwidth / Interface bandwidth (default reference is 100 Mbps)

---

## 📎 Resources

- [Jeremy's IT Lab - OSPF Playlist](https://www.youtube.com/@JeremysITLab)
- [Cisco OSPF Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/xe-16/iro-xe-16-book.html)
