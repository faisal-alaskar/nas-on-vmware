# NAS on VMware

Build a DIY NAS using **TrueNAS SCALE** on **VMware**, without cloud subscriptions.
A practical homelab setup for local storage, backups, and iPhone photo syncing.

📘 **Arabic version:** [اقرأ الشرح بالعربي](README.ar.md)

---

## Overview

This repository provides a **real-world, step-by-step guide** to building a
self-hosted NAS using **TrueNAS SCALE** running inside a **VMware virtual machine**.

The goal is to achieve:

* Large local storage capacity
* Full control over data
* No monthly cloud subscriptions
* Simple and reliable file sharing across devices

This setup was built and tested in a home environment and is suitable for
personal use, backups, and homelab projects.

---

## Why This Setup?

I was choosing between:

* Buying a ready-made NAS with **4TB for ~$300**
* Or buying a **14TB hard drive for ~$160**

I chose the second option and built my own NAS because:

* Much larger storage for less money
* My PC already runs **24/7**
* No dependency on cloud providers
* Full ownership of personal data (photos, files, backups)

---

## Architecture Overview

* **Host OS:** Windows (PC running 24/7)
* **Hypervisor:** VMware Workstation
* **NAS OS:** TrueNAS SCALE (Open Source & Free)
* **OS Disk:** 40GB virtual disk (system only)
* **Storage Disk:** Physical hard drive passed directly to the VM
* **Network Mode:** Bridged
* **File Sharing:** SMB

Clients can access the NAS from:

* Windows
* macOS
* iPhone (via PhotoSync)
* Any device on the local network

---

## Requirements

### Hardware

* PC or laptop (always-on preferred)
* One dedicated hard drive for storage (HDD or SSD)
* LAN connection recommended

### Software

* Windows (Host)
* VMware Workstation Player or Pro
* TrueNAS SCALE ISO

---

## Download TrueNAS SCALE

This guide uses:

```
TrueNAS SCALE 22.02.2.1 (Angelfish)
```

Official download link:
[https://download.truenas.com/TrueNAS-SCALE-Angelfish/22.02.2.1/TrueNAS-SCALE-22.02.2.1.iso](https://download.truenas.com/TrueNAS-SCALE-Angelfish/22.02.2.1/TrueNAS-SCALE-22.02.2.1.iso)

TrueNAS is not the only option, but it was chosen because it is:

* Simple to use
* Open source
* Free
* Actively maintained

---

## Important Disk Notice

If you plan to use a **physical disk from the same PC**:

* The disk **must not** be used by Windows
* Do **not** use the system drive (C:)
* Example:

  * C: → Windows OS
  * D: → Dedicated NAS disk (passed to the VM)

Once passed to the VM, Windows will no longer use this disk.

Alternative (recommended):

* Use a separate internal or external hard drive
* Or dedicate an old PC/laptop entirely as a NAS

---

## Step 1: Install VMware Workstation

* Download VMware Workstation Player (Free):
  [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)
* Install and reboot if required

---

## Step 2: Create the Virtual Machine

1. Open VMware
2. Create New Virtual Machine
3. Select:

   * Installer disc image (ISO)
4. Choose the TrueNAS SCALE ISO

### VM Resources

* CPU: 2 cores
* RAM: 8GB (minimum recommended)

### Network

```
Network Adapter: Bridged
```

This allows TrueNAS to:

* Get an IP from the same network
* Act as a standalone NAS device
* Be reachable from all LAN devices

---

## Step 3: Disk Configuration

### OS Disk

* Virtual disk
* Size: 40GB
* Used only for TrueNAS system files

### Storage Disk

Choose one:

* Physical disk passed directly to the VM
* Dedicated HDD/SSD for NAS data

Do not store Windows data on this disk.

---

## Step 4: Install TrueNAS SCALE

1. Start the VM
2. Select **Install TrueNAS**
3. Choose the **40GB virtual disk**
4. Complete installation
5. Reboot

After boot, TrueNAS will display an IP address.

---

## Step 5: Access the Web Interface

From any device on the network:

```
http://IP-ADDRESS
```

* Log in
* Set a strong admin password
* Assign a static IP (recommended)

---

## Step 6: Create Storage Pool

1. Storage → Pools
2. Create Pool
3. Select the physical disk
4. Create the pool
5. Create a dataset (recommended)

---

## Step 7: Create User

1. Accounts → Users
2. Create User
3. Set username and password
4. Assign permissions to the dataset

---

## Step 8: Enable SMB Sharing

1. Sharing → SMB
2. Add SMB Share
3. Select dataset path
4. Enable SMB service
5. Verify permissions

---

## Accessing the NAS

### Windows

```
\\IP-ADDRESS
```

### macOS or iphone

```
smb://IP-ADDRESS
```

### iPhone

* App: PhotoSync
* Protocol: SMB
* Enter NAS credentials

---

## iPhone Photo Backup (Real Use Case)

* Total data transferred: ~400GB
* Transfer time: ~24 hours
* PC connected via LAN
* iPhone connected via 5GHz Wi-Fi

Result:

* No iCloud
* Browsable files
* Full control over photos and videos

---

## Backup Strategy

Because the data is personal and irreplaceable:

* Enable snapshots
* Backup to another drive
* Or sync to an external NAS/server

Never rely on a single copy.

---

## Alternative Setup (Bare Metal NAS)

If you have:

* An old PC
* Or an unused laptop with a large disk

You can:

* Install TrueNAS directly
* Use it as a dedicated NAS
* No virtualization required

---

## Conclusion

* Building your own NAS is cheaper and more flexible
* TrueNAS SCALE is powerful and easy to use
* VMware allows reuse of existing hardware
* No cloud, no subscriptions, full control

---


## 🛠️ Troubleshooting – Common Issues & Fixes

This section covers the most common problems you may face while setting up or running a NAS on VMware using TrueNAS SCALE, along with practical solutions.

---

### ❌ TrueNAS does not get an IP address

**Possible cause:**

* Incorrect network configuration in VMware

**Fix:**

* Make sure:

  ```
  Network Adapter = Bridged
  ```
* Restart the VM
* Verify the bridged adapter is bound to the correct physical LAN interface

---

### ❌ Cannot access the TrueNAS web interface

**Check the following:**

* The VM is running
* You are using:

  ```
  http://IP-ADDRESS
  ```
* Try accessing from another device on the same network
* Ensure the Windows firewall is not blocking the connection

---

### ❌ Physical disk not visible inside TrueNAS

**Common reasons:**

* Disk is still mounted or in use by Windows
* Disk was not passed correctly to the VM

**Fix:**

* Open **Disk Management** in Windows
* Make sure the disk is:

  * Offline
  * Has no drive letter
* Restart VMware
* Reboot the TrueNAS VM

⚠️ **Never use the same disk simultaneously in Windows and TrueNAS**

---

### ❌ Wrong disk selected during TrueNAS installation

**Result:**

* Data on that disk is erased

**Fix:**

* Unfortunately, there is no recovery
* Always:

  * Use a small virtual disk (40GB) for the OS
  * Use a separate physical disk for storage

---

### ❌ Permission denied when writing or uploading files

**Cause:**

* Incorrect dataset permissions

**Fix:**

1. Go to the dataset
2. Edit Permissions
3. Ensure the user has:

   * Read / Write access
4. Enable:

   ```
   Apply permissions recursively
   ```

---

### ❌ SMB share is visible but not accessible

**Verify:**

* SMB service is running
* Correct username and password are used

**Fix:**

* Services → SMB → Start
* Reconnect from the client device

---

### ❌ Slow transfer speeds

**Possible causes:**

* Weak Wi-Fi connection
* Insufficient VM resources

**Fixes:**

* Use wired LAN instead of Wi-Fi
* Ensure the iPhone is connected to 5GHz Wi-Fi
* Increase VM RAM to 8GB or more
* Avoid USB 2.0 drives for storage

---

### ❌ TrueNAS freezes or shuts down unexpectedly

**Possible causes:**

* Insufficient RAM
* Host system under heavy load

**Fix:**

* Increase RAM allocation
* Disable sleep/hibernation on the host PC
* Turn off aggressive power-saving settings in Windows

---

### ❌ iPhone photos do not fully transfer

**Possible cause:**

* iPhone or network going to sleep

**Fix:**

* Disable Auto-Lock during transfer
* Keep the phone plugged into a charger
* Transfer large libraries in batches

---

### ❌ SMART warnings or disk health alerts

**Important:**

* Do not ignore these alerts

**Fix:**

* Run a SMART test
* Back up data immediately
* Consider replacing the disk

---

## ✅ General Best Practices

* Never use the same disk for OS and data
* Always have at least one backup
* Do not expose the NAS directly to the internet
* Use a static IP address
* Monitor disk health regularly

---

## 📌 Final Note

If you encounter an issue not listed here:

* Open an Issue in the repository
* Or contribute a fix via Pull Request

Sharing real-world issues helps improve this project for everyone 🤝

