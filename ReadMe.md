# Adam and Eve
---
The Dell Latitude E6530 laptop has been wiped and had Debian13 installed at the time of writing. The goal of project "Adam and Eve" is simple: make an old laptop, which used to sit in the drawer reminiscent of it's glory days, a usable server/homelab. It should be able to:
1. Run at least one VM where I would practise my labs for the RHCSA.
2. Act as storage for automated backups (mobile phone, laptop, personal computer)
3. Host media (photos for now)

For targets 2 and 3 a new, bigger capacity Solid State Drive might be needed as the current SSD's capacity is 120GB

## Project name
I chose the name "Adam and Eve" not because of biblical context. "Eve" symbolizes a place where I grew up, and "Adam" is the most famous pair for "Eve".

## Distro history
As mentioned in the beginning, the server has Debian13 installed after experiences with Windows 7, 10, 11 and Fedora Linux 40-42. I chose Debian for it's well known stability, and to get to know something else after experience in only fedora-based distros and a short relationship with Arch.

## First steps
After installing Debian and setting up an admin account, I immediately disabled the Gnome GUI using the command below.
```bash
sudo systemctl set-default multi-user.target
# Sets the default target when booting up to CLI
```
Why? I believe learning the CLI is one of the most important skills an IT proffesional could have. It takes some getting used to if you're used to GUI solutions Windows and MacOS have, but the ever so sweet fruits of learning CLI are worth it.
A cool hostname and a static IP is needed to reliably SSH into the server without worrying about a changed IP:
```bash
hostnamectl hostname eve

sudo nmcli con add type ethernet con-name debiandell ifname eno1 ip4 192.168.1.153/24 gw4 192.168.1.254 \ ipv4.dns "8.8.8.8 8.8.4.4"

nmcli con down "Wired connection 1"
sudo nmcli con delete "Wired connection 1"

sudo nmcli con up debiandell 
```
Afterwards I installed and enabled ssh.
```bash
sudo apt install ssh

sudo systemctl enable sshd.service
sudo systemctl start sshd.service
```

From my main laptop:
```bash
cd ~/.ssh
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519.pub domantas@192.168.1.153

```
Since I didn't want to bother writing out ssh domantas@192.168.1.153 every time, I edited the ssh config:
```bash
vim ~/.ssh/config
```
Inside the file:
```bash
Host eve
    Hostname 192.168.1.153
    User domantas
    IdentityFile ~/.ssh/id_ed25519
```