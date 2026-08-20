Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/operational_systems
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/to_learning 
Related: [[Linux Roadmap]]

---
# Oracle Linux 9: Basic Security and SSH Configuration

## Overview

This tutorial covers the basic configuration we performed on an Oracle Linux 9 server:

1. Check and disable SELinux.
2. Check and disable the firewall.
3. Create a dedicated group for remote access.
4. Add a user to the remote access group.
5. Configure SSH to allow only users from that group.
6. Validate the final configuration.

The commands below are applicable to Oracle Linux 9 and are generally similar on RHEL 9 and compatible distributions.

---

## 1. Check the SELinux Status

First, verify whether SELinux is enabled and which mode it is using.

```bash
getenforce
```

Possible results:

* `Enforcing`: SELinux is enabled and actively enforcing policies.
* `Permissive`: SELinux is enabled but only logs policy violations.
* `Disabled`: SELinux is completely disabled.

For more detailed information:

```bash
sestatus
```

You can also check the persistent configuration:

```bash
grep '^SELINUX=' /etc/selinux/config
```

---

## 2. Temporarily Disable SELinux

To switch SELinux to permissive mode without rebooting:

```bash
sudo setenforce 0
```

Then validate:

```bash
getenforce
```

The expected result is:

```text
Permissive
```

This is temporary. The setting may return to its configured mode after a reboot.

---

## 3. Permanently Disable SELinux

Edit the SELinux configuration:

```bash
sudo vi /etc/selinux/config
```

Change:

```text
SELINUX=enforcing
```

to:

```text
SELINUX=disabled
```

After changing the configuration, reboot the server:

```bash
sudo reboot
```

After the reboot, verify:

```bash
getenforce
```

The expected result is:

```text
Disabled
```

> Important: On modern Oracle Linux versions, disabling SELinux permanently is a significant security decision. If the objective is only to troubleshoot an application, `Permissive` mode is often preferable to completely disabling SELinux.

---

# 4. Check the Firewall

Oracle Linux 9 uses **firewalld** as its standard firewall management service.

Check whether it is installed:

```bash
rpm -q firewalld
```

Check its current status:

```bash
sudo systemctl status firewalld
```

---

## 5. Stop the Firewall Temporarily

To stop firewalld immediately:

```bash
sudo systemctl stop firewalld
```

This does not prevent the service from starting again after a reboot.

---

## 6. Disable the Firewall Permanently

To prevent firewalld from starting automatically:

```bash
sudo systemctl disable firewalld
```

To stop it immediately and disable it for future boots:

```bash
sudo systemctl disable --now firewalld
```

Verify:

```bash
sudo systemctl status firewalld
```

---

## 7. Check nftables and iptables

Stopping firewalld does not disable the Linux networking firewall subsystem itself.

Oracle Linux 9 uses **nftables** as the underlying packet filtering framework.

Check the current nftables rules:

```bash
sudo nft list ruleset
```

You can also inspect iptables compatibility rules:

```bash
sudo iptables -L -n -v
```

If the iptables output shows:

```text
Chain INPUT (policy ACCEPT)

Chain FORWARD (policy ACCEPT)

Chain OUTPUT (policy ACCEPT)
```

with no additional rules, there are no restrictive iptables rules currently listed.

> Keep in mind that `firewalld`, `nftables`, and `iptables` are related but are not exactly the same thing. Disabling firewalld does not inherently prevent another service or manually configured ruleset from applying packet filtering.

---

# 8. Create the Remote Access Group

Create a dedicated Linux group called `remoto`:

```bash
sudo groupadd remoto
```

Verify the group:

```bash
getent group remoto
```

---

# 9. Add the User to the Group

Add the `admin` user to the `remoto` group:

```bash
sudo usermod -aG remoto admin
```

The `-aG` options are important:

* `-a`: append the group without removing existing group memberships.
* `-G`: modify the user's supplementary groups.

Verify:

```bash
id admin
```

You should see `remoto` listed among the user's groups.

The user may need to log out and log back in before the new group membership is reflected in an existing session.

---

# 10. Configure SSH to Allow Only the Remote Group

The goal is to allow SSH access only to users who belong to the `remoto` group.

Edit the SSH server configuration:

```bash
sudo vi /etc/ssh/sshd_config
```

Add:

```text
AllowGroups remoto
```

This tells `sshd` to allow SSH authentication only for users belonging to the specified group.

For example:

```text
AllowGroups remoto
```

If multiple groups need SSH access, they can be specified on the same line:

```text
AllowGroups remoto suporte devops
```

---

# 11. Validate the SSH Configuration

Before restarting SSH, always validate the configuration:

```bash
sudo sshd -t
```

If the command returns nothing, the configuration syntax is valid.

If there is an error, fix it before restarting the SSH service.

---

# 12. Restart SSH

After validating the configuration:

```bash
sudo systemctl restart sshd
```

Check the service:

```bash
sudo systemctl status sshd
```

---

# 13. Important SSH Safety Procedure

Before restarting `sshd`, keep your current SSH session open.

This is especially important when configuring:

```text
AllowGroups remoto
```

If the current user is not actually a member of `remoto`, you could prevent yourself from opening a new SSH session.

A safe procedure is:

1. Keep the current SSH session open.
2. Add the user to `remoto`.
3. Verify with `id admin`.
4. Add `AllowGroups remoto` to `sshd_config`.
5. Run `sshd -t`.
6. Restart `sshd`.
7. Open a second SSH session.
8. Confirm that `admin` can connect.
9. Only then close the original session.

---

# 14. Final Validation

After completing the configuration, perform the following checks.

### SELinux

```bash
getenforce
```

Expected result if completely disabled:

```text
Disabled
```

### Firewall

```bash
sudo systemctl is-active firewalld
```

Expected result:

```text
inactive
```

### Remote group

```bash
getent group remoto
```

The `admin` user should appear as a member.

### User groups

```bash
id admin
```

The output should contain:

```text
remoto
```

### SSH configuration

```bash
sudo sshd -t
```

No output means the configuration is syntactically valid.

### SSH service

```bash
sudo systemctl is-active sshd
```

Expected result:

```text
active
```

---

# 15. Final Configuration

At the end of the configuration, the server should have the following setup:

```text
Oracle Linux 9
│
├── SELinux
│   └── Disabled
│
├── firewalld
│   └── Disabled
│
├── Linux group
│   └── remoto
│
├── User
│   └── admin → member of remoto
│
└── SSH
    └── AllowGroups remoto
```

The result is that SSH access is restricted to users belonging to the `remoto` group.

For example:

```text
admin
 └── remoto
      └── SSH access: ALLOWED
```

A user that is not a member of `remoto` will not be allowed to authenticate through SSH.

## Security Consideration

This configuration intentionally reduces several layers of host security by disabling SELinux and firewalld. It may be appropriate for a controlled lab, development environment, troubleshooting scenario, or a server where these controls are managed elsewhere, such as through cloud network security controls.

For a production server, it is generally preferable to keep SELinux and host-level firewall protection enabled unless there is a documented reason to disable them.
