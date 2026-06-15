
# Netplan Configuration on Ubuntu

## Overview

Netplan is the default network configuration tool used in modern Ubuntu releases. It allows administrators to configure network interfaces using YAML files.

Common tasks include:

* Configuring a static IP address
* Enabling DHCP
* Setting DNS servers
* Configuring gateways
* Managing multiple interfaces

---

## Check Network Interfaces

Display available network interfaces:

```bash
ip addr
```

Or:

```bash
ip link show
```

Example output:

```text
ens33
ens34
lo
```

---

## Netplan Configuration Files

Configuration files are usually stored in:

```bash
/etc/netplan/
```

List available configuration files:

```bash
ls -l /etc/netplan/
```

Example:

```text
00-installer-config.yaml
```

---

## Backup Existing Configuration

Before making changes:

```bash
sudo cp /etc/netplan/00-installer-config.yaml \
/etc/netplan/00-installer-config.yaml.bak
```

---

## Configure DHCP

Example DHCP configuration:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
```

Apply configuration:

```bash
sudo netplan apply
```

---

## Configure Static IP Address

Example:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

Apply configuration:

```bash
sudo netplan apply
```

---

## Test Configuration Before Applying

To avoid losing connectivity on a remote server:

```bash
sudo netplan try
```

The configuration will automatically roll back if not confirmed.

---

## Verify Configuration

Check assigned IP address:

```bash
ip addr show ens33
```

Check routing table:

```bash
ip route
```

Check DNS servers:

```bash
cat /etc/resolv.conf
```

---

## Troubleshooting

### Validate Configuration

```bash
sudo netplan generate
```

### View Detailed Information

```bash
sudo netplan --debug apply
```

### Check Connectivity

Ping gateway:

```bash
ping -c 4 192.168.1.1
```

Ping internet:

```bash
ping -c 4 8.8.8.8
```

Ping domain name:

```bash
ping -c 4 google.com
```

---

## Useful Commands

```bash
ip addr
ip route
sudo netplan try
sudo netplan apply
sudo netplan generate
sudo netplan --debug apply
```

---

## Summary

Netplan simplifies network management on Ubuntu by using YAML-based configuration files. Understanding DHCP, static IP addressing, routing, and DNS configuration is essential for Linux system administration and network engineering tasks.
