# How to Rename a Network Interface from `enp0s3` to `eth0` on Oracle Linux 9

## Overview

On Oracle Linux 9, network interfaces normally use predictable names such as `enp0s3`, `ens160`, or `enp1s0`.

If you need to use the traditional `eth0` name, simply running:

```bash
ip link set dev enp0s3 name eth0
```

is not enough.

This command changes the interface name only at runtime. After a reboot, the operating system will create the interface again using its default predictable name, such as `enp0s3`.

Oracle Linux 9 uses **NetworkManager** instead of the old `network-scripts` system, so you should not expect to find files such as:

```text
/etc/sysconfig/network-scripts/ifcfg-enp0s3
```

The recommended approach is to configure the interface name at boot level and make sure the NetworkManager connection profile matches it.

---

## 1. Check the Current Interface

First, identify the network interface:

```bash
ip a
```

You may see something similar to:

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP>
    link/ether 08:00:27:xx:xx:xx
```

In this example, the physical interface is `enp0s3`.

You can also check NetworkManager:

```bash
nmcli device status
```

Example:

```text
DEVICE   TYPE      STATE      CONNECTION
enp0s3   ethernet  connected  Wired connection 1
lo       loopback  unmanaged   --
```

---

## 2. Why `ip link set` Is Not Persistent

The following command:

```bash
ip link set dev enp0s3 name eth0
```

changes the interface name only in the running kernel.

It does **not** change the operating system's configuration for how the interface should be named during the next boot.

Therefore, after reboot:

```bash
ip a
```

will normally show:

```text
2: enp0s3:
```

again.

This is also why changing only the NetworkManager connection profile may not be enough. The profile can be configured for `eth0`, while the actual physical device still appears as `enp0s3`.

---

# 3. Oracle Linux 9 Uses NetworkManager

On Oracle Linux 9, NetworkManager stores connection profiles under:

```text
/etc/NetworkManager/system-connections/
```

You can inspect them with:

```bash
ls -l /etc/NetworkManager/system-connections/
```

You may find a file such as:

```text
Wired connection 1.nmconnection
```

These files replace the traditional `ifcfg-*` configuration files used by older Oracle Linux and RHEL versions.

---

# 4. Configure the Interface to Use `eth0`

The cleanest approach is to disable the predictable interface naming mechanism at boot.

Edit:

```text
/etc/default/grub
```

Find:

```text
GRUB_CMDLINE_LINUX=
```

For example:

```text
GRUB_CMDLINE_LINUX="rhgb quiet"
```

Add:

```text
net.ifnames=0 biosdevname=0
```

The result should look similar to:

```text
GRUB_CMDLINE_LINUX="rhgb quiet net.ifnames=0 biosdevname=0"
```

### What do these parameters do?

`net.ifnames=0` disables the predictable network interface naming scheme.

`biosdevname=0` prevents BIOS-based naming from being used.

Together, they allow the traditional naming convention such as:

```text
eth0
eth1
```

to be used.

---

# 5. Regenerate the GRUB Configuration

After changing `/etc/default/grub`, regenerate the GRUB configuration.

For a BIOS-based system:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

For a UEFI installation, the correct configuration path can depend on the Oracle Linux installation and EFI layout. Check the existing EFI configuration before overwriting it.

You can check whether the system is booted using UEFI with:

```bash
test -d /sys/firmware/efi && echo UEFI || echo BIOS
```

---

# 6. Configure the NetworkManager Connection

Now check the available NetworkManager connections:

```bash
nmcli connection show
```

You may see:

```text
NAME                UUID                                  TYPE      DEVICE
Wired connection 1  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  ethernet  enp0s3
```

Change the connection profile so that it is associated with `eth0`:

```bash
nmcli connection modify "Wired connection 1" connection.id eth0 connection.interface-name eth0
```

The important part here is:

```text
connection.interface-name eth0
```

This tells NetworkManager that the connection profile belongs to the device named `eth0`.

You can verify the configuration with:

```bash
nmcli connection show eth0
```

Look for:

```text
connection.id:              eth0
connection.interface-name: eth0
```

---

# 7. Reboot the Server

At this point, reboot the machine:

```bash
reboot
```

After the system comes back, check:

```bash
ip a
```

The expected result should be similar to:

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

Then check NetworkManager:

```bash
nmcli device status
```

Expected:

```text
DEVICE  TYPE      STATE      CONNECTION
eth0    ethernet  connected  eth0
lo      loopback  connected  --
```

---

# 8. If `eth0` Exists but Has No IP Address

This is an important troubleshooting case.

You may find that after reboot:

```bash
ip a
```

shows:

```text
2: enp0s3:
```

or:

```text
2: eth0:
```

but there is no IP address.

First check the NetworkManager profiles:

```bash
nmcli connection show
```

Then check the device:

```bash
nmcli device status
```

If the profile is not active, try:

```bash
nmcli connection up eth0
```

If this fails, inspect the profile:

```bash
nmcli connection show eth0
```

Pay particular attention to:

```text
connection.interface-name
ipv4.method
ipv4.addresses
ipv4.gateway
ipv4.dns
connection.autoconnect
```

For example, a static IPv4 configuration might contain:

```text
ipv4.method: manual
ipv4.addresses: 192.168.1.100/24
ipv4.gateway: 192.168.1.1
ipv4.dns: 192.168.1.1
connection.autoconnect: yes
```

If DHCP is being used instead:

```text
ipv4.method: auto
```

---

# 9. Check the NetworkManager Logs

If the interface exists but NetworkManager does not activate the connection, check the NetworkManager logs:

```bash
journalctl -u NetworkManager -b
```

You can also filter for the interface:

```bash
journalctl -u NetworkManager -b | grep -E 'eth0|enp0s3'
```

This can reveal problems such as:

* The connection profile is associated with the wrong interface.
* NetworkManager cannot find the expected device.
* Another connection profile is being activated.
* The interface is unmanaged.
* DHCP is failing.
* The interface name does not match the connection profile.

---

# 10. Verify the MAC Address

Before making changes, it is also useful to record the MAC address:

```bash
ip link show enp0s3
```

Example:

```text
2: enp0s3:
    link/ether 08:00:27:12:34:56
```

The MAC address is important because it identifies the physical or virtual network adapter independently of its Linux interface name.

You can also check it using:

```bash
nmcli device show enp0s3
```

Look for:

```text
GENERAL.HWADDR
```

---

# 11. Alternative: Rename the Interface Using udev

Another possible approach is to create a udev rule that associates the MAC address with `eth0`.

For example:

```text
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="08:00:27:12:34:56", NAME="eth0"
```

The MAC address must be replaced with the actual MAC address of the interface.

However, on Oracle Linux 9, I would prefer the **GRUB `net.ifnames=0 biosdevname=0` approach** unless there is a specific reason to use a custom udev rule.

---

# 12. Important: Do Not Rely Only on `nmtui`

`nmtui` is useful for managing NetworkManager profiles, but there is an important distinction:

```text
NetworkManager connection name
        ≠
Linux network interface name
```

For example, you can have:

```text
Connection name: eth0
Device name:     enp0s3
```

That does not mean the actual Linux interface has been renamed.

This explains a common situation where `nmtui` appears to be correctly configured but:

```bash
ip a
```

still shows:

```text
enp0s3
```

The kernel and boot process still determine the actual interface name.

---

# 13. Recommended Configuration

For a traditional `eth0` setup on Oracle Linux 9, the important pieces should eventually look like this:

### GRUB

```text
GRUB_CMDLINE_LINUX="... net.ifnames=0 biosdevname=0"
```

### NetworkManager

```text
connection.id=eth0
connection.interface-name=eth0
```

### Linux

```text
ip a
```

should show:

```text
eth0
```

### NetworkManager

```bash
nmcli device status
```

should show something similar to:

```text
DEVICE  TYPE      STATE      CONNECTION
eth0    ethernet  connected  eth0
```

---

# 14. Why This Solves the Problem

The important difference is that we are no longer performing a temporary runtime rename.

Instead, the configuration tells the operating system:

```text
At boot:
    Do not use predictable network names
    ↓
    Create the network interface as eth0
    ↓
    NetworkManager finds eth0
    ↓
    NetworkManager activates the eth0 connection profile
    ↓
    The IP configuration is applied
```

This makes the configuration persistent across reboots.

---

## Final Checklist

Before rebooting a production or remote server, verify:

* [ ] `net.ifnames=0` is present in the GRUB kernel parameters.
* [ ] `biosdevname=0` is present if you want to completely avoid BIOS-based naming.
* [ ] GRUB configuration has been regenerated.
* [ ] A NetworkManager connection profile exists.
* [ ] The connection profile uses `connection.interface-name=eth0`.
* [ ] The IP configuration is correct.
* [ ] `connection.autoconnect` is enabled.
* [ ] The MAC address has been recorded.
* [ ] You have console access or another recovery method before rebooting a remote server.

The key point is: **do not use `ip link set` as the persistent configuration mechanism.** On Oracle Linux 9, configure the boot-time interface naming and make the NetworkManager profile match the resulting interface name.

---
Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/operational_systems
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/to_learning 
Related: [[Linux Roadmap]]
