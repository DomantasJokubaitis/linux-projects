# Debian Homelab

This repository documents my homelab which is built on a headless Debian server.
Included: system configuration, virtualization setup, networking, troubleshooting.

## Contents
- `docs` - Server documentation
- `rhcsa-prep` - Notes for the RHCSA
- `images` - Screenshots and visualised information

## Server Overview
- Hostname: eve (Hypervisor), adam (workstation)
- OS: Debian 13 (headless)
- Hypervisor: KVM/QEMU + libvirt + Cockpit
- Networking: Network bridge (br0), en0 (enslaved)
- Storage: /srv for VM's and .iso files
- Management: Cockpit + SSH
