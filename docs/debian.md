# A homelab project
---
A Dell Latitude E6530 laptop has been converted to a homelab.
The goal of this project is simple: make an old laptop into a usable server/homelab. It should be able to:
1. Run at least one VM where I would practise labs for the RHCSA.
2. Act as storage for automated backups (mobile phone, laptop, personal computer)
3. Host media (photos for now)

## Hardware problems
The homelab contains a 120GB SSD, which is very small for todays standarts, especially for a server.
The 750GB HDD is more than a decade old and isn't reliable enough to hold data.
8GB of DDR3 RAM is only enough for about 3 virtual machines.

## Distro history
Debian13 was the distribution of choice. The server previously experienced Windows 7, 10, 11 and Fedora Linux 40-42.
Debian was chosen for it's well known stability, and to widen my personal knowledge after experience in only fedora based distros and a short experience with Arch.

## First steps
After installing Debian and setting up an admin account, I immediately disabled the Gnome GUI using the command below.
```bash
sudo systemctl set-default multi-user.target
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
>Masking a target creates a symlink to `/dev/null`, which in turn makes a service impossible to start, contrary to disabling a service, where it's still possible to start under the right conditions.
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
## Virtualization
The RHCSA preperation book I'm reading (RHCSA9 Certification Guide by Sander Van Vugt) requires me to have a RHEL based distribution installed, so a decision on virtualization software was made.
KVM/QEMU was chosen for it's near to native performance and light weight.
> To get started I used a guide which could be found [here](https://www.freecodecamp.org/news/turn-ubuntu-2404-into-a-kvm-hypervisor)
>It's meant for Ubuntu but can still be used for systems running Debian.
First and foremost, I installed mandatory libraries:
```bash
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils -y
```
Add user to the libvirt group:
```bash
sudo chmod -aG libvirt user
```
A network bridge is needed for virtual machines would be accessible from the workstation.
The table below shows different networking options that could be used for virtualization purposes.
![Table with networking modes and their traits](pictures/networking-modes.png)
The ethernet interface should be enslaved to the bridge, and the latter needs a static ip address:
```bash
sudo nmcli con add type bridge ifname br0 con-name bridge-br0
sudo nmcli con add type ethernet ifname eno1 master br0 con-name bridge-slave-en0
sudo nmcli con mod bridge-br0 gw4 192.168.1.x ipv4.dns "8.8.8.8 8.8.4.4"
```
Delete the old connection and activate the new connection:
```bash
sudo nmcli con down eth-name && sudo nmcli con delete eth-name
sudo nmcli con up bridge-br0
```
Now a new network bridge must be defined in libvirt via `vim ~/br0.xml`:
```xml
<network>
  <name>br0</name>
  <forward mode='bridge'/>
  <bridge name='br0'/>
</network>
```
```bash
sudo virsh net-define ~/br0.xml
sudo virsh net-start br0
sudo virsh net-autostart br0

sudo virsh net-list all
```
I ran into a problem where the virtual machines refused to start because of non-descriptive network errors.
The solution was to edit the xml mentioned before, and delete any other network sections that it has. Redefine after this step. 

Another problem that I encountered - all of the created vm's would either
loop in the GRUB screen or would freeze after "succesfully" booting.
The fix I found was to redefine the default storage pool. The default pool has too little space for one virtual machine.
To set a new default storage pool, read [this](https://serverfault.com/questions/840519/how-to-change-the-default-storage-pool-from-libvirt).

To summarize, you just need to define the non-default storage pool as the default via
`virsh pool-define-as`.

Unfortunately no RHEL-based distros, including CentOS, AlmaLinux, RockyLinux work as virtual machines.
I tried changing BIOS mode to UEFI for the virtual machines, but that didn't help.

I don't have the solution for now, so I'm using a fedora server installation
for the **RHCSA** preparation.
