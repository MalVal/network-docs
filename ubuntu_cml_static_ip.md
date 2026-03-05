# Ubuntu Static IP Configuration Guide for CML

This guide explains how to set a static IP address, gateway, and DNS on Ubuntu (18.04, 20.04, 22.04, 24.04) in **Cisco Modeling Labs (CML)**.

---

## 1️⃣ Check the network interface name

Open a terminal and run:

```bash
ip addr
```

Look for your network interface name, e.g., `enp0s3`, `ens33`, or `eth0`. Note it for later.

---

## 2️⃣ Identify the Netplan file

Netplan is used for network configuration. List the files:

```bash
ls /etc/netplan/
```

Example file: `00-installer-config.yaml`.

---

## 3️⃣ Edit the Netplan file

Open the file with `nano`:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

### Example static IP configuration

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:               # Replace with your interface name
      dhcp4: no
      addresses:
        - 192.168.1.50/24  # Static IP + subnet mask (/24 = 255.255.255.0)
      gateway4: 192.168.1.1 # Gateway
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] # DNS servers
```

> ⚠️ YAML is sensitive to spaces. Make sure the indentation is correct.

---

## 4️⃣ Apply the configuration

```bash
sudo netplan apply
```

---

## 5️⃣ Verify the configuration

```bash
ip addr       # Verify IP
ip route      # Verify gateway
ping 8.8.8.8  # Test connectivity
```

---

## 💡 CML Tips

- Ensure your VM is connected to a **cloud/external** network to reach the gateway.
- Use IP addresses within the same subnet as the CML gateway.
- For multiple VMs, create separate Netplan configs or modify the existing file accordingly.

