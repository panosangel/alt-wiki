# System - KVM Virtualization

## Basic Info

- **Description:** Install QEMU/KVM and basic settings
- **System OS:** Ubuntu 22.04 LTS (x86_64)
- **Linux kernel version:** 5.15
- **QEMU/KVM version:** 6.2
- **CPU:** Intel i7 4770
- **Date:** 2022.10.15

## Installation

Install on host computer
```shell
sudo apt install -y qemu-system libvirt-daemon-system bridge-utils
```

Install on host to manage guests
```shell
sudo apt install -y virtinst libguestfs-tools virt-top 
```

Install GUI to manage the guests
```shell
sudo apt install -y virt-manager
```

Install UEFI Firmware for Virtual Machines
```shell
sudo apt install -y ovmf 
```

## Add managing user to new groups

```shell
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER 
```

## Create network bridge (br0)
[TODO]

## Enable and Check daemon

```shell
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
sudo systemctl check libvirtd
```




## Sources

- [How to Install KVM on Ubuntu 22.04 (Jammy Jellyfish)](https://www.linuxtechi.com/how-to-install-kvm-on-ubuntu-22-04/)
- [Server World - KVM](https://www.server-world.info/en/note?os=Ubuntu_22.04&p=kvm&f=1)
- [Virtualization Tuning and Optimization Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_tuning_and_optimization_guide/index)
- [gentoo linux -QEMU](https://wiki.gentoo.org/wiki/QEMU)
- [QEMU -  A generic and open source machine emulator and virtualizer](https://www.qemu.org/)
- [Arch - libvirt](https://wiki.archlinux.org/title/libvirt)

## Troubleshooting

- [virt-sysprep error Domain not found: no domain with matching name](https://askubuntu.com/questions/1415438/virt-sysprep-error-domain-not-found-no-domain-with-matching-name)


-------------------

## Load necessary modules on startup ##

```sh
sudo nano /etc/initramfs-tools/modules
```

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
vhost-net
```

Update the initramfs.

```sh
sudo update-initramfs -u
```

## Various Checks ##

```sh
kvm-ok
lsmod | grep kvm
```

```sh
lsmod | grep vfio
```

```sh
qemu-system-x86_64 --version
```

```sh
dmesg | grep pci-stub
```

```sh
dmesg | grep VFIO
```

## Download OVMF BIOS ##

Enable UEFI boot for the guest OS.

```sh
sudo apt-get install ovmf
```

The OVMF version in the repositories is usually outdated. If it's not compatible with your hardware then download the [latest edk2.git-ovmf-x64](https://www.kraxel.org/repos/jenkins/edk2/) and unpack to `/`.

Backup the current files.

```sh
sudo mv /usr/share/ovmf/OVMF.fd /usr/share/ovmf/OVMF.fd.ORIGINAL
sudo mv /usr/share/OVMF/OVMF_CODE.fd /usr/share/OVMF/OVMF_CODE.fd.ORIGINAL
sudo mv /usr/share/OVMF/OVMF_VARS.fd /usr/share/OVMF/OVMF_VARS.fd.ORIGINAL
```

```sh
sudo cp /usr/share/edk2.git/ovmf-x64/OVMF-pure-efi.fd /usr/share/ovmf/OVMF.fd
sudo cp /usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd /usr/share/OVMF/OVMF_CODE.fd
sudo cp /usr/share/edk2.git/ovmf-x64/OVMF_VARS-pure-efi.fd /usr/share/OVMF/OVMF_VARS.fd
```

```sh
sudo ln -s '/usr/share/ovmf/OVMF.fd' '/usr/share/qemu/OVMF.fd'
```

## [Windows] Download VFIO Drivers ##

Download the VFIO driver ISO to be used with the Windows installation from https://fedoraproject.org/wiki/Windows_Virtio_Drivers. Below are the direct links to the ISO images:

Latest VIRTIO drivers: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso

Stable VIRTIO drivers: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

## [Windows 7] Fix Update Issue ##

https://support.microsoft.com/en-us/kb/3135445  
https://support.microsoft.com/en-us/kb/3172605

## Install SPICE guest tools ##

[SPICE - Download Guest Tools](http://www.spice-space.org/download.html)  

**Linux Guest**

```sh
sudo apt-get install spice-vdagent
```

Video drivers

```sh
sudo apt-get install xserver-xorg-video-qxl
```

**Windows Guest**  
https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-exe.latest

## Additional Packages ##

Fix the error: "Error connecting to graphical console. Error opening Spice console, SpiceClientGtk missing."

```sh
sudo apt-get install gir1.2-spice-client-gtk-3.0
```

**Source:** [Linux Mint Forums - Spice doesn't work from Virtual Machine Manager](https://forums.linuxmint.com/viewtopic.php?t=227025)

## SPICE Remote View ##

```sh
spicy -h localhost -p 5900
```

## Edit XML settings ##

```sh
virsh edit BOXNAME
```


