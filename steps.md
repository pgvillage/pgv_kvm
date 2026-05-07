## steps to set up kvm on an Ubuntu machine and prepares VM's ready for Ubuntu to be installed

### 0: download base image
https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.7-x86_64-boot.iso

###  1: install packages 

```bash
sudo apt update
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients virt-manager bridge-utils cloud-image-utils
```

### 2: Add user to kvm group


```bash
sudo usermod -aG libvirt $USER
newgrp libvirt
```

###  3: create template VM

```bash
virt-install   --name rocky-template --ram 2048 --vcpus 2 --disk path=/var/lib/libvirt/images/rocky-template.qcow2,size=20 --os-variant rocky9 --network network=default --graphics spice --cdrom ~/Downloads/Rocky-9.7-x86_64-boot.iso
```

### 4: Enter vm and set it up
elder
Ubuntu4Life!

### 5: Update and install tools

```bash
sudo dnf update -y
sudo dnf install -y vim curl wget git cloud-init qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent
```

### 6: Clean the machine and remove ssh stuff

```bash
sudo truncate -s 0 /etc/machine-id
sudo rm -f /var/lib/dbus/machine-id
sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
sudo rm -f /etc/ssh/ssh_host_*
```

### 7: Shutdown and unlist
Shut down the machine (this will never be started again and can be removed from virt-manager (not the disk))


### 10: convert to reusable template

```bash
chmod -w /var/lib/libvirt/images/rocky-template.qcow2
```

### 11: clone the template to usable images

```bash
qemu-img create -f qcow2 -b /var/lib/libvirt/images/rocky-template.qcow2  /var/lib/libvirt/images/client.qcow2
qemu-img create -f qcow2 -b /var/lib/libvirt/images/rocky-template.qcow2  /var/lib/libvirt/images/db1.qcow2
qemu-img create -f qcow2 -b /var/lib/libvirt/images/rocky-template.qcow2  /var/lib/libvirt/images/db2.qcow2
qemu-img create -f qcow2 -b /var/lib/libvirt/images/rocky-template.qcow2  /var/lib/libvirt/images/db3.qcow2
```

### 12: create the vms

```bash
virt-install --name client --ram 2048 --vcpus 2 --disk path=/var/lib/libvirt/images/client.qcow2 --import --os-variant rocky9 --network network=default --graphics none
virt-install --name client --ram 2048 --vcpus 2 --disk path=/var/lib/libvirt/images/db1.qcow2 --import --os-variant rocky9 --network network=default --graphics none
virt-install --name client --ram 2048 --vcpus 2 --disk path=/var/lib/libvirt/images/db2.qcow2 --import --os-variant rocky9 --network network=default --graphics none
virt-install --name client --ram 2048 --vcpus 2 --disk path=/var/lib/libvirt/images/db3.qcow2 --import --os-variant rocky9 --network network=default --graphics none
```

### 13: first boot

```bash
sudo systemd-machine-id-setup
sudo hostnamectl set-hostname client
```
