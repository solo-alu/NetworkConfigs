# Lab - SSH (Secure Shell)

## 🎯 Objective

Configure SSH on Switch 2 and use Laptop 1 to remotely access it securely via SSH instead of the less secure Telnet protocol.

---

## 📋 Lab Tasks

- Set a hostname and domain name on SW2 (required for RSA key generation)
- Generate RSA encryption keys on SW2
- Create a local user account for SSH authentication
- Configure VTY lines to accept only SSH connections
- Assign a management IP to SW2
- SSH into SW2 from Laptop 1 and verify access

---

## ⚙️ Device Configurations

- [SW2-config.txt](./SW2-config.txt)

---

## ✅ Verification Commands Used

```
! Set hostname and domain name
SW2(config)# hostname SW2
SW2(config)# ip domain-name jeremysitlab.com

! Generate RSA keys
SW2(config)# crypto key generate rsa
! (recommended key size: 2048 bits)

! Create local user
SW2(config)# username jeremy secret ccnapass

! Configure VTY lines for SSH only
SW2(config)# line vty 0 15
SW2(config-line)# login local
SW2(config-line)# transport input ssh

! Set SSH version 2
SW2(config)# ip ssh version 2

! Verify SSH
SW2# show ip ssh
SW2# show ssh

! SSH from Laptop 1
ssh -l jeremy [SW2 management IP]
```

---

## 🔍 Issues / Notes

- 

