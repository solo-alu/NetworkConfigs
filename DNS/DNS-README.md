# Lab - DNS (Domain Name System)

## 🎯 Objective

Configure DNS on a Cisco router and verify that hostnames can be resolved to IP addresses. Understand how DNS queries work and how to point devices to a DNS server.

---

## 🖧 Topology

![Lab Topology](./topology.png)

---

## 📋 Lab Tasks

- Configure the DNS server in Packet Tracer with hostname to IP mappings
- Point the router to the DNS server using `ip name-server`
- Enable DNS lookup on the router
- Configure static hostname mappings using `ip host`
- Verify DNS resolution by pinging devices by hostname instead of IP

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)

---

## ✅ Verification Commands Used

```
! Point router to DNS server
R1(config)# ip name-server [DNS server IP]

! Enable hostname to address translation (on by default)
R1(config)# ip domain-lookup

! Configure a static hostname mapping on the router
R1(config)# ip host [hostname] [IP address]

! Verify DNS entries
R1# show hosts

! Test DNS resolution
R1# ping [hostname]
```

---

## 🔍 Issues / Notes

- 

