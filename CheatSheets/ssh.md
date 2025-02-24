---
id: ssh
aliases:
  - SSH Setup Guide - Linux to Termius
tags: []
---

# SSH Setup Guide - Linux to Termius

## Installation
```bash
sudo apt update
sudo apt install openssh-server
```

## Service Management
```bash
# Check SSH status
sudo systemctl status ssh

# Start SSH
sudo systemctl start ssh

# Enable SSH on boot
sudo systemctl enable ssh
```

## Finding Your IP Address
```bash
# Show all network interfaces
ip addr show

# Look for the 'inet' address under wlp2s0 (wireless)
# or enp1s0 (ethernet) interface
```

## Termius Configuration
- Host: Your inet address (usually starts with 192.168. or 10.0.)
- Port: 22
- Username: Your Linux username
- Password: Your Linux user password

## Quick Test
```bash
# Test SSH locally
ssh username@localhost
```

## Security Tips
- Use SSH keys instead of passwords for better security
- Keep your system updated
- Change default SSH port (optional)
- Use a firewall
