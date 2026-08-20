Claro. Mantive a estrutura técnica e apenas traduzi para um inglês técnico e natural.

# Tutorial: Samba AD DC Configuration on Ubuntu

This tutorial describes the **correct and functional deployment** of a **Samba AD Domain Controller** on Ubuntu, including:

* Domain provisioning
* Internal DNS
* Kerberos
* Users and groups
* Samba shares
* Authentication and access tests

---

## Prerequisites

* Ubuntu Server 20.04+ recommended
* Static IP address
* Dedicated host, preferably
* Ports **53, 88, 389 and 445** available
* Root or sudo access

---

## 1. Check system information

```bash
lsb_release -a
hostnamectl
ip a | grep inet
timedatectl
```

> **Kerberos requires accurate time synchronization.**

If necessary:

```bash
sudo timedatectl set-ntp true
```

---

## 2. Configure the server hostname

```bash
sudo hostnamectl set-hostname dc1
hostname
hostname -f
```

The expected FQDN is:

```text
dc1.essias.com.br
```

---

## 3. Configure `/etc/hosts`

Edit the file:

```bash
sudo nano /etc/hosts
```

Remove the existing line similar to:

```text
127.0.1.1   old_hostname
```

Add:

```text
127.0.0.1       localhost
192.168.56.10   dc1.essias.com.br dc1
```

Test hostname resolution:

```bash
ping -c 2 dc1
ping -c 2 dc1.essias.com.br
```

---

## 4. Disable `systemd-resolved` and configure the local DNS

Samba AD will provide the DNS service for the domain.

Disable `systemd-resolved`:

```bash
sudo systemctl disable --now systemd-resolved
sudo rm /etc/resolv.conf
```

Create a new `/etc/resolv.conf`:

```bash
sudo nano /etc/resolv.conf
```

Add:

```text
nameserver 127.0.0.1
search essias.com.br
```

Optional: prevent the file from being overwritten:

```bash
sudo chattr +i /etc/resolv.conf
```

---

## 5. Install the required packages

```bash
sudo apt update
sudo apt install -y samba krb5-user winbind smbclient dnsutils
```

During the `krb5-user` installation:

* **Realm:** `ESSIAS.COM.BR`
* Leave the other fields blank.

> The Samba provisioning process will configure Kerberos for the AD domain.

---

## 6. Stop and disable legacy Samba services

```bash
sudo systemctl stop smbd nmbd winbind
sudo systemctl disable smbd nmbd winbind
```

---

## 7. Back up the existing `smb.conf`

```bash
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

---

## 8. Provision the Samba AD domain with RFC2307

Run:

```bash
sudo samba-tool domain provision \
  --realm=ESSIAS.COM.BR \
  --domain=ESSIAS \
  --server-role=dc \
  --dns-backend=SAMBA_INTERNAL \
  --use-rfc2307
```

Verify that the configuration file was created:

```bash
test -f /etc/samba/smb.conf && echo "smb.conf created successfully"
```

---

## 9. Enable and start the Samba AD DC service

```bash
sudo systemctl enable samba-ad-dc
sudo systemctl start samba-ad-dc
systemctl status samba-ad-dc
```

> **Do not use `systemctl restart samba` for an AD DC.**
>
> The correct service is **`samba-ad-dc`**.

---

## 10. Validate the domain and internal DNS

Check the domain information:

```bash
samba-tool domain info 127.0.0.1
```

Test the DNS SRV records:

```bash
host -t SRV _ldap._tcp.essias.com.br
```

A successful result should return the LDAP service associated with your domain controller.

---

## 11. Test Kerberos authentication

Authenticate as the domain Administrator:

```bash
kinit Administrator
klist
```

If `kinit` fails, check:

* DNS resolution
* DNS SRV records
* Server time synchronization
* Hostname and FQDN
* Kerberos realm
* Samba AD DC service status

---

## 12. Create a test user

Create a test user:

```bash
sudo samba-tool user create usuario.teste
```

Authenticate using Kerberos:

```bash
kinit usuario.teste
klist
```

---

## 13. Create a group and add the user

Create the group:

```bash
sudo samba-tool group create grupo.teste
```

Add the user to the group:

```bash
sudo samba-tool group addmembers grupo.teste usuario.teste
```

Verify the group membership:

```bash
sudo samba-tool group listmembers grupo.teste
```

---

## 14. Find the group's GID

Because the domain was provisioned with RFC2307, retrieve the group's GID:

```bash
getent group 'ESSIAS\grupo.teste'
```

Example output:

```text
ESSIAS\grupo.teste:x:20001:
```

> The value `20001` is only an example. Use the actual GID returned by your system.

---

## 15. Create the shared directory

Create the directory:

```bash
sudo mkdir -p /srv/samba/teste
```

Set the group ownership:

```bash
sudo chown -R root:20001 /srv/samba/teste
```

Set the permissions:

```bash
sudo chmod -R 0770 /srv/samba/teste
```

> Replace `20001` with the actual GID obtained in the previous step.

---

## 16. Configure the Samba share

Edit the Samba configuration:

```bash
sudo nano /etc/samba/smb.conf
```

Add the following section at the end of the file:

```ini
[Teste]
    path = /srv/samba/teste
    read only = no
    browsable = yes
    guest ok = no
    valid users = usuario.teste
    force group = grupo.teste
    create mask = 0660
    directory mask = 0770
```

---

## 17. Restart the Samba AD DC

```bash
sudo systemctl restart samba-ad-dc
```

Check the service status:

```bash
systemctl status samba-ad-dc
```

---

## 18. Test access to the Samba share

Connect to the share using the test user:

```bash
smbclient //localhost/Teste -U usuario.teste
```

Inside `smbclient`, test the basic operations:

```text
ls
mkdir teste
```

The `ls` command should list the contents of the share, while `mkdir teste` should create a test directory.

---

## 19. Test password

For the purposes of this tutorial, the test password is:

```text
AdmEssias@123
```

> **Important:** Do not use this password in a production environment. Use a strong, unique password and follow your organization's password policy.

---

# Final validation

At this point, the Samba AD DC should have the following components working:

| Component       | Validation                                     |
| --------------- | ---------------------------------------------- |
| Hostname        | `hostname -f`                                  |
| Local DNS       | `host dc1.essias.com.br`                       |
| AD domain       | `samba-tool domain info 127.0.0.1`             |
| LDAP SRV        | `host -t SRV _ldap._tcp.essias.com.br`         |
| Kerberos        | `kinit Administrator`                          |
| Kerberos ticket | `klist`                                        |
| Users           | `samba-tool user list`                         |
| Groups          | `samba-tool group list`                        |
| Samba service   | `systemctl status samba-ad-dc`                 |
| SMB share       | `smbclient //localhost/Teste -U usuario.teste` |

The most important point is the **order of operations**:

**Hostname → `/etc/hosts` → DNS → packages → Samba provisioning → Samba AD DC → DNS validation → Kerberos → users/groups → shares.**

This order prevents most of the common problems encountered when deploying a Samba AD DC from scratch.
