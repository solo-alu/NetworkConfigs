# Lab - FTP & TFTP (File Transfer Protocol / Trivial File Transfer Protocol)

## 🎯 Objective

Use TFTP and FTP to upgrade the Cisco IOS image on routers. Practice navigating the IOS file system, copying files from external servers to flash memory, setting the boot image, and verifying the upgrade after reload.

---

## 📋 Lab Tasks

- Inspect the IOS file system and current flash contents on each router
- Check available flash memory before copying
- Use TFTP to copy a new IOS image from the TFTP server to flash
- Use FTP to copy a new IOS image from the FTP server to flash
- Configure FTP credentials on the router
- Set the new IOS image as the boot file
- Save the configuration and reload the router
- Verify the new IOS version is running after reload
- Delete the old IOS image to free up flash space

---

## ⚙️ Device Configurations

- [R1-config.txt](./R1-config.txt)
- [R2-config.txt](./R2-config.txt)

---

## ✅ Commands Used

```
! View IOS file systems
R1# show file systems

! Check current flash contents and available space
R1# show flash

! Check current IOS version
R1# show version

! Copy IOS image from TFTP server to flash
R1# copy tftp: flash:
Address or name of remote host []? [TFTP server IP]
Source filename []? [IOS filename.bin]
Destination filename []? [IOS filename.bin]

! Configure FTP credentials before copying via FTP
R1(config)# ip ftp username [username]
R1(config)# ip ftp password [password]

! Copy IOS image from FTP server to flash
R1# copy ftp: flash:
Address or name of remote host []? [FTP server IP]
Source filename []? [IOS filename.bin]
Destination filename []? [IOS filename.bin]

! Set the new IOS image as the boot file
R1(config)# boot system flash:[IOS filename.bin]

! Save config and reload to apply new IOS
R1# copy running-config startup-config
R1# reload

! Verify new IOS is running after reload
R1# show version

! Delete old IOS image to free up flash space
R1# delete flash:[old IOS filename.bin]
```

---

## 🔍 Issues / Notes

- Always run show flash before copying to make sure there is enough free space for the new image
- The boot system command must be saved to startup-config before reloading or it will be lost
- FTP requires credentials to be configured with ip ftp username and ip ftp password before the copy command will work
- After reloading, show version confirms which IOS image is currently running in RAM

