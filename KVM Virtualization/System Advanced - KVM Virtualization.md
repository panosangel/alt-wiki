# System Advanced - KVM Virtualization #

## Basic Info ##

**Description:** Install QEMU/KVM and basic settings  
**System OS:** Linux Mint 18.02 Cinnamon (x86_64)  
**Linux kernel version:** 4.8.0-53  
**QEMU/KVM version:** 2.6.2  
**CPU:** Intel i7 7700HQ  
**Date:** 2017.11.06  

**Source:** [Linux Mint Forums - HOW-TO make dual-boot obsolete using kvm VGA passthrough](http://forums.linuxmint.com/viewtopic.php?f=231&t=212692)  
**Read more:** [How to setup a gaming virtual machine with GPU passthrough (QEMU, KVM, libvirt, and VFIO) ](http://www.se7ensins.com/forums/threads/how-to-setup-a-gaming-virtual-machine-with-gpu-passthrough-qemu-kvm-libvirt-and-vfio.1371980/)  
**Read more:** [Ubuntu Forums - Windows Gaming VM - KVM / UEFI Version - HowTo](http://ubuntuforums.org/showthread.php?t=2266916)


## Turn IOMMU ON ##

Edit grub settings:

```sh
sudo nano /etc/default/grub
```

Add/edit as follows:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

```sh
sudo update-grub
```
**Reboot system.**

Then check if everything works ok.

```sh
dmesg | grep "Virtualization Technology for Directed I/O"
```

## Install necessary packages ##

Get the latest version from PPA.

```sh
sudo add-apt-repository ppa:jacob/virtualisation
sudo apt-get update
```

```sh
sudo apt-get install qemu-kvm qemu-utils
sudo apt-get install libvirt-bin libvirt-dev virt-manager bridge-utils
sudo apt-get install virt-viewer spice-client-gtk python-spice-client-gtk
sudo apt-get install seabios
```

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

## Enable networking - Setting up bridge interface ##

See the specific guide `System Advanced - Nework Bridge for QEMU/KVM`

or

[Define a network bridge using Ubuntu’s / Linux Mint’s Network Manager application](https://heiko-sieger.info/define-a-network-bridge-using-ubuntus-linux-mints-network-manager-application/)  

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
