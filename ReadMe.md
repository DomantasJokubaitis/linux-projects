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
Afterwards I installed and enabled SSH.
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

Since writing out ssh domantas@192.168.1.153 every time is cumbersome, I edited the ssh config inside ~/.ssh/config:
```bash
Host eve
    Hostname 192.168.1.153
    User domantas
    IdentityFile ~/.ssh/id_ed25519
```

Now it's possible to SSH into the server by only writing `ssh eve`, great!

## A few tweaks
Since a server shouldn't ever go to sleep on it's own, I turned off all the ways for the server to do so:
```bash
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
I also learnt that masking creates a symlink to /dev/null, which in turn makes a service impossible to start, contrary to disabling a service.

The next action I did was enabling Wake on Lan in my server's BIOS and editing my debiandell profile:
```bash
sudo nmcli con mod debiandell 802-3-ethernet.wake-on-lan magic
```
Ether-wake had to be installed into my main laptop to user WoL:
```bash
sudo dnf install ether-wake
```
Then the command which turns on the server was put inside /usr/local/bin/wakeserver.sh
```bash
#!/bin/bash
sudo ether-wake -D d4:be:d9:86:08:e9
```

## GUI server overview
Up until this point I've only heard of Cockpit. So after some reading i decided to install it, since a simple server overview will definetely be useful in the future.
```bash
sudo apt install cockpit

systemctl enable cockpit
systemctl start cockpit
```
Using Cockpit is easy, just type `host:9090' into an internet browsers search bar (replace host with your actual host) and type in your login details when prompted.
Inside the Cockpit there are a couple of sections which let me quickly see how's my server doing. Of course, the server is not doing anything meaningful for now, so I'll get back to Cockpit when it's needed.