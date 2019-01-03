# System Configuration - Networking

## 1. Configure network adapter
In Ubuntu 18.04, the network is configured with Netplan and the configuration file is `/etc/netplan/*.yaml`. The traditional network configuration file `/etc/network/interfaces` is not used anymore.

Open the highest priority YAML file in the path`/etc/netplan/` and add/edit one of the following options.

**Option 1.:** DHCP
```
 network:
   version: 2
   renderer: networkd
   ethernets:
     ens33:
       dhcp4: yes
       dhcp6: no
       dhcp4-overrides:
         use-dns: no
       nameservers:
         search: [anydomain.local]
         addresses: [1.1.1.1, 1.0.0.1]
```
**Option 2.:** Static
```
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.1.101/24]
      gateway4: 192.168.1.1
      nameservers:
        search: [anydomain.local]
        addresses: [1.1.1.1, 1.0.0.1]
```
Then restart the network to apply the changes:
```bash
sudo netplan generate
sudo netplan apply --debug
```
Verify the changes:
```bash
ip add show
```
Ping a domain to check internet connection
```
ping -c4 google.com
```

## 2. Set hostname
**Modern method:**
```bash
hostnamectl set-hostname <random-hostname>
```
**Classic method:**
Update system hostname. Read at boot time.
```bash
echo <random-hostname> > /etc/hostname
```
Set for current session
```bash
hostname <random-hostname>
```

## 3.  Update host file
If does not exist, add the following line in `/etc/hosts`.
```
192.168.1.101 randomhostname.anydomain.local randomhostname
```

## 4. Verify new settings
Run the following in the command line.
```bash
hostname
hostname -f
```
The first command returns the short hostname while the second command shows the fully qualified domain name (fqdn).

## 5. Configure systemd-resolve
Check enabled nameservers:
```bash
systemd-resolve --status
```
Edit configuration file:
```bash
sudo nano /etc/systemd/resolved.conf
```
Change 
`#DNSSEC=no` to `DNSSEC=yes`

Change `/etc/resolv.conf` pointer:
```bash
sudo rm /etc/resolv.conf
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
When needed, disable systemd-resolved and manage `/etc/resolv.conf` manually
```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
```

## 5. Benchmarking LAN

[How do you test the network speed between two boxes?](https://askubuntu.com/questions/7976/how-do-you-test-the-network-speed-between-two-boxes)

## Appendix A - Interesting reads
[Ubuntu 18.04 Server Guide - Networking](https://help.ubuntu.com/lts/serverguide/networking.html.en) 
[Ubuntu Bionic: Netplan](https://blog.ubuntu.com/2017/12/01/ubuntu-bionic-netplan)
[Network Name Resolution manager](https://www.freedesktop.org/software/systemd/man/systemd-resolved.service.html#/etc/resolv.conf)
[How to take back control of /etc/resolv.conf on Linux](https://www.ctrl.blog/entry/resolvconf-tutorial)
[Ubuntu 18.04: Network configuration](https://www.hiroom2.com/2018/05/29/ubuntu-1804-network-en/)
[A name resolution issue with systemd-resolved we found in the wild](https://moss.sh/name-resolution-issue-systemd-resolved/)
[My War on Systemd-resolved](https://ohthehugemanatee.org/blog/2018/01/25/my-war-on-systemd-resolved/)
[Setup DNS Resolution With “resolv.conf” in Examples](https://www.shellhacks.com/setup-dns-resolution-resolvconf-example/)

## Appendix B - Free DNS servers
- OpenDNS: 208.67.222.222, 208.67.220.220
- Cloudflare: 1.1.1.1, 1.0.0.1
- Google: 8.8.8.8, 8.8.4.4
- Quad9: 9.9.9.9, 149.112.112.112

