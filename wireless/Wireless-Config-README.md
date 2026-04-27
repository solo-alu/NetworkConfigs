# Lab - Wireless Configuration (Day 58)

## 🎯 Objective

Configure WLANs on a Cisco Wireless LAN Controller (WLC) using the GUI accessed via HTTPS from PC1's web browser. Create dynamic interfaces, configure WLANs with WPA2-PSK security, and connect a smartphone to the wireless network to verify connectivity.

---

## 📋 Lab Tasks

- Access the WLC GUI by navigating to the WLC management IP address via HTTPS in PC1's web browser
- Log into the WLC GUI using admin credentials
- Review the WLC Monitor summary page — note connected APs, clients, and rogue APs
- Create a new dynamic interface on the WLC and map it to the correct VLAN
- Navigate to WLANs → Create New to create a new WLAN
- Set the WLAN Profile Name, SSID, and WLAN ID
- Under the General tab — enable the WLAN and assign it to the correct dynamic interface
- Under the Security tab → Layer 2 — set security to WPA2 Policy with AES encryption and PSK authentication
- Enter the PSK passphrase for the WLAN
- Apply and verify the WLAN appears as enabled in the WLAN list
- Connect the smartphone to the WLAN by configuring its wireless settings with the correct SSID and PSK
- Verify the smartphone receives an IP address and can ping devices on the network

---

## 📸 Screenshots

Since this lab is configured entirely through the WLC GUI and smartphone wireless settings there are no CLI config files. The following screenshots document the configuration:

- [wlc-login.png](./wlc-login.png) — WLC HTTPS login page from PC1's browser
- [wlc-monitor.png](./wlc-monitor.png) — WLC Monitor summary page
- [wlc-dynamic-interface.png](./wlc-dynamic-interface.png) — Dynamic interface creation
- [wlc-wlan-general.png](./wlc-wlan-general.png) — WLAN General tab (SSID, status, interface)
- [wlc-wlan-security.png](./wlc-wlan-security.png) — WLAN Security tab (WPA2, AES, PSK)
- [wlc-wlan-list.png](./wlc-wlan-list.png) — Completed WLAN showing as enabled
- [smartphone-wireless.png](./smartphone-wireless.png) — Smartphone wireless settings connected to the WLAN

---

## 🖥️ WLC GUI Navigation Reference

```
Accessing the WLC GUI:
  Open PC1 web browser → https://[WLC management IP]
  Username: admin
  Password: Cisco123

Creating a Dynamic Interface:
  Controller → Interfaces → New
  Interface Name: [name]
  VLAN ID: [VLAN number]
  → Apply
  IP Address, Netmask, Gateway, DHCP Server → Apply

Creating a WLAN:
  WLANs → dropdown: Create New → Go
  Profile Name: [name]
  SSID: [SSID name]
  ID: [WLAN ID]
  → Apply

WLAN General Tab:
  Status: Enabled ✓
  Interface: [select dynamic interface]
  → Apply

WLAN Security Tab → Layer 2:
  Layer 2 Security: WPA + WPA2
  WPA2 Policy: Enabled ✓
  WPA2 Encryption: AES ✓
  Auth Key Mgmt: PSK
  PSK Format: ASCII
  Pre-Shared Key: [passphrase]
  → Apply
```

---

## 🔍 Issues / Notes

- The WLC GUI must be accessed using HTTPS not HTTP — the browser may show a security warning about the certificate which you can bypass in Packet Tracer
- After creating the WLAN you must go back and manually enable it under the General tab — it is disabled by default and this is one of the most common things to forget
- The dynamic interface must be created before the WLAN — it maps the WLAN to a VLAN and the WLAN cannot be assigned an interface that doesn't exist yet
- The smartphone in Packet Tracer connects to the WLAN through its Config tab → Wireless settings — enter the SSID and PSK there
- WPA2 with AES encryption is the standard used in modern enterprise wireless networks — TKIP is older and less secure and should not be selected
- The WLAN ID, VLAN number, and SSID number should match where possible to keep the configuration organized and easy to troubleshoot

