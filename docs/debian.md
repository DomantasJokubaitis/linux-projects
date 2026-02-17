# A homelab project
---
A Dell Latitude E6530 laptop has been converted to a homelab.
The goal of this project is simple: make an old laptop into a usable server/homelab. It should be able to:
1. Run at least one VM where I would practise labs for the RHCSA.
2. Act as storage for automated backups (mobile phone, laptop, personal computer)
3. Host media (photos for now)

## Distro history
Debian13 was the distribution of choice. The server previously experienced Windows 7, 10, 11 and Fedora Linux 40-42.
Debian was chosen for it's well known stability, and to widen my personal knowledge after experience in only fedora based distros and a short experience with Arch.

## First steps
Set up an admin account, disable the Gnome desktop environment:
```bash
sudo systemctl set-default multi-user.target
sudo reboot now
```
A hostname and a static IP is needed to reliably SSH into the server without worrying about a changed IP:
```bash
hostnamectl hostname host

sudo nmcli con add type ethernet con-name eth-name ifname eno1 ip4 192.168.1.x/24 gw4 192.168.1.x \ ipv4.dns "8.8.8.8 8.8.4.4"

nmcli con down "Wired connection 1"
sudo nmcli con delete "Wired connection 1"

sudo nmcli con up eth-name 
```
Afterwards I installed and enabled SSH.
```bash
sudo apt install ssh

sudo systemctl enable sshd.service
sudo systemctl start sshd.service
```

From the server:
```bash
cd ~/.ssh
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519.pub host@192.168.1.x
```

Since writing out `ssh host@192.168.1.x` every time is cumbersome, I edited the ssh config inside `~/.ssh/config`:
```bash
Host host
    Hostname ip
    User person
    IdentityFile ~/.ssh/id_ed25519
```

Now it's possible to SSH into the server by writing `ssh host`

## Power tweaks
Since a server shouldn't ever sleep, any form of sleep and hibernation must be disabled:
```bash
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
>
The next action was enabling Wake on Lan in the server's BIOS and editing the eth-name profile:
```bash
sudo nmcli con mod eth-name 802-3-ethernet.wake-on-lan magic
```
Ether-wake had to be installed into my workstation for WoL:
```bash
sudo dnf install ether-wake
```
`/usr/local/bin/wakeserver.sh` script:
```bash
#!/bin/bash
sudo ether-wake -D a1:b2:c3:d4:e5:f6
```
Disabling the laptop screen:
```sudo vim /etc/default/grub```
```GRUB_CMDLINE_LINUX_DEFAULT="quiet consoleblank=60"```

## Cockpit
Cockpit is a server monitoring tool which shows general system information, performance metrics, virtual machines, etc.
To use Cockpit it has to be installed and enabled on the server:
```bash
sudo apt install cockpit cockpit-machines -y

systemctl enable cockpit
systemctl start cockpit
```
To use Cockpit, enter `host:9090` into an internet browsers search bar (replace host with your actual host) and enter the server login details.
> Make sure to add host to /etc/hosts

## References
[Disable laptop screen](https://gist.github.com/tankibaj/71547848decdd7cc3bf0c6df68935c8f)

