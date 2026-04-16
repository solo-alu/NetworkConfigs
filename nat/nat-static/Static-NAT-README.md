# Lab - Static NAT (Network Address Translation) Part 1

## 🎯 Objective

Configure static NAT on R1 to map the private IP addresses of internal PCs to public IP addresses, allowing them to communicate with external hosts on the internet. Verify NAT translations and understand the difference between static and dynamic NAT entries.

---

## 📋 Lab Tasks

- Attempt to ping 8.8.8.8 from PC1 before NAT is configured to confirm it fails
- Configure the inside and outside NAT interfaces on R1
- Create a static NAT mapping for each PC — mapping their private IP to a public IP in the 100.0.0.x/24 subnet
- Ping 8.8.8.8 from PC1 again to confirm NAT is working
- Ping google.com from each PC and verify NAT translations on R1
- Clear the NAT translation table and observe which entries remain

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)

---

## ✅ Commands Used

```
! Define the inside NAT interface (facing internal PCs)
R1(config)# interface g0/1
R1(config-if)# ip nat inside

! Define the outside NAT interface (facing the internet/ISP)
R1(config)# interface g0/0
R1(config-if)# ip nat outside

! Configure static NAT mappings (one per PC)
! Syntax: ip nat inside source static [inside-local] [inside-global]
R1(config)# ip nat inside source static 192.168.0.1 100.0.0.1
R1(config)# ip nat inside source static 192.168.0.2 100.0.0.2
R1(config)# ip nat inside source static 192.168.0.3 100.0.0.3

! Verify NAT translations
R1# show ip nat translations

! View NAT statistics
R1# show ip nat statistics

! Clear dynamic NAT translations (static entries will remain)
R1# clear ip nat translation *
```

---

## 🔍 Issues / Notes

- Before NAT is configured the pings to 8.8.8.8 fail because the ISP drops traffic with private source IP addresses
- The inside interface must face the internal network and the outside interface must face the internet — getting these backwards is a common mistake
- Static NAT creates a permanent one-to-one mapping — each PC needs its own dedicated public IP address
- After running clear ip nat translation * only the dynamic entries are removed — static entries always remain in the translation table
- Each PC had 8.8.8.8 configured as its DNS server which is why pinging google.com by name also worked

