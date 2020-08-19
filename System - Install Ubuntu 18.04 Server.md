# System - Install Ubuntu 18.04 Server

## Installation settings
- **OS:** Ubuntu 18.04 LTS
- **Hostname:** <your-hostname> i.e. zeus
- **Domain:** <your-domain> i.e. olympus.local
- **Local user:** <local-user> i.e. hercules
- **IP:** Managed via DHCP static assign
-  **Disk Partitions:**
   -   /boot = 1 GB (ext4 file system)
   -   / = 30 GB (ext4 file system)
   -   /home = ?? G (ext4 file system)
 - **Preinstalled services:** OpenSSH

## First commands after installation
Enable extra repositories:
```bash
sudo add-apt-repository universe
```
Update system:
```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
sudo reboot
```
﻿Add system user to sudoers (if not already):
```bash
sudo usermod -aG sudo <local-user>
```
Check date and time:
```bash
date
```
If it's not correct, set the appropriate timezone. In my case it is `Europe/Athens`.
```bash
sudo timedatectl set-timezone Europe/Athens
```
_See the appropriate guide for more!_

## Install packages:

### Basic:
```bash
sudo apt-get install dstat
```

### Optional:
```bash
sudo apt-get install build-essential
```

## Setup a swap
_See the appropriate guide ;)_

## Move /tmp to ram
Add in `/etc/fstab` the following line:
```
tmpfs    /tmp  tmpfs   nodev,nosuid,size=2G 0 0
```

## Set unattended upgrades
Depending on a version you may have one or other or both of `/etc/apt/apt.conf.d/10periodic` or `/etc/apt/apt.conf.d/20auto-upgrades`.

Not modifying the original will stop getting nags about overwriting the packages' maintainers version on upgrade of the unattended-upgrade package !!

Create `/etc/apt/apt.conf.d/21periodic-auto-upgrades_on` which will supersede either `/etc/apt/apt.conf.d/10periodic or /etc/apt/apt.conf.d/20auto-upgrades`
```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
APT::Periodic::RandomSleep "1";
APT::Periodic::Verbose "2";
```
Source: 
- [Ubuntu 18.04  Server Guide - Package Management - Automatic Updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html.en)
- [Ubuntu Enable Automatic Updates Unattended Upgrades](https://www.richud.com/wiki/Ubuntu_Enable_Automatic_Updates_Unattended_Upgrades#50unattended-upgrades)
- [Cerebrux - Τα πρώτα 10 λεπτά σε έναν νέο Server: Βασικές ρυθμίσεις ασφαλείας](https://cerebrux.net/2016/06/15/10-lepta-server-vasiki-asfaleia/)
## Nice additions
- [Ubuntu Logo in color ASCII art](https://ubuntuforums.org/showthread.php?t=2385550)
    
## Appendix A - Sources
- [Ubuntu 18.04 LTS Server (Bionic Beaver) Installation Guide with Screenshots](https://www.linuxtechi.com/ubuntu-18-04-server-installation-guide/)
