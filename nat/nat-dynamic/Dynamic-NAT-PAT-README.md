# Lab - Dynamic NAT & PAT (Network Address Translation) Part 2

## 🎯 Objective

Configure dynamic NAT on R1 using an ACL and address pool, then convert the configuration to PAT (NAT overload) to allow multiple inside hosts to share a single public IP address. Understand the difference between dynamic NAT and PAT and how port numbers are used to track translations.

---

## 📋 Lab Tasks

- Configure inside and outside NAT interfaces on R1
- Create an ACL to define which internal traffic should be translated
- Create a NAT pool of public IP addresses
- Configure dynamic NAT mapping the ACL to the pool
- Ping google.com from each PC and observe NAT translations
- Notice PC3 cannot reach the internet once the pool is exhausted
- Convert the dynamic NAT config to PAT by adding the overload keyword
- Verify all PCs including PC3 can now reach google.com simultaneously
- Also configure PAT using the outside interface IP instead of a pool
- Verify translations and observe port numbers being used to track sessions

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)

---

## ✅ Commands Used

```
! Define inside and outside NAT interfaces
R1(config)# interface g0/1
R1(config-if)# ip nat inside

R1(config)# interface g0/0
R1(config-if)# ip nat outside

! Create ACL to define which traffic should be translated
R1(config)# access-list 1 permit 172.16.0.0 0.0.0.255

! Create a NAT pool of public IP addresses
R1(config)# ip nat pool POOL1 100.0.0.0 100.0.0.3 netmask 255.255.255.0

! Configure dynamic NAT (one-to-one from ACL to pool)
R1(config)# ip nat inside source list 1 pool POOL1

! Convert to PAT using pool overload (many-to-one with port tracking)
R1(config)# ip nat inside source list 1 pool POOL1 overload

! Alternative PAT method - use the outside interface IP instead of a pool
R1(config)# ip nat inside source list 1 interface g0/0 overload

! Verify NAT/PAT translations
R1# show ip nat translations

! View NAT statistics
R1# show ip nat statistics

! Check which NAT commands are in the running config
R1# show running-config | include nat

! Clear all dynamic NAT/PAT translations
R1# clear ip nat translation *
```

---

## 🔍 Issues / Notes

- With dynamic NAT (no overload), each inside host uses one public IP from the pool — if the pool runs out PC3 cannot get translated and pings fail
- Adding the overload keyword converts it to PAT — all hosts now share the pool addresses using unique port numbers to track each session
- When using PAT with interface instead of a pool, R1 uses its own g0/0 public IP and port numbers to track all translations — this is the most common real-world method
- The show ip nat translations output shows port numbers in PAT mode — this is how the router knows which translation belongs to which host
- Traffic denied by the ACL is NOT translated but is also NOT dropped — it simply passes through untranslated
- Dynamic and PAT entries time out automatically when sessions end — unlike static NAT entries which are permanent

