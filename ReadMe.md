# Debian Homelab

This repository documents my homelab which is built on a headless Debian server. \
**Included**: system configuration, virtualization setup, networking, troubleshooting.

## Contents
- `docs` - Server documentation
- `rhcsa-prep` - Notes for the RHCSA

## Server Overview
- CPU: Intel i5 3210m
- RAM: 8gb DDR3
- Storage: 120GB ssd
- OS: Debian 13 (headless)
- Hypervisor: KVM/QEMU + libvirt + Cockpit
- Networking: Network bridge (br0), en0 (enslaved)
- Management: Cockpit + SSH
