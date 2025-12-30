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

## References
[KVM User's Guide](https://docs.oracle.com/en/operating-systems/oracle-linux/9/kvm-user/kvm-network-topicgroup.html#kvm-virtual-networks)
[Libvirt FAQ](https://wiki.libvirt.org/FAQ.html#libvirt-faq)
