# System Advanced - Network Bridge for QEMU/KVM #

## Basic Info ##

**Description:** Setting up a network bridge interface to use with KVM guests.  
**System OS:** Linux Mint 18.2 (Server) & Ubuntu Server 16.04 LTS x86_64 (Guest)  
**Date:** 2017.06.09

## Allow Qemu to use the bridge ##

```sh
sudo nano /etc/qemu/bridge.conf
```

Insert the line below:

```
allow br1
```

## Create the bridge (at server) ##

```sh
sudo nano /etc/network/interfaces
```

Edit the file to look as the following:

```
auto l0
iface lo inet loopback

auto br1
iface br1 inet static
    address 10.0.1.1
    network 10.0.1.0
    netmask 255.255.255.0
    broadcast 10.0.1.255
    dns-nameservers 8.8.8.8 8.8.4.4
    bridge_ports <NAME_OF_ETHERNET_INTERFACE>
    bridge_stp off
    bridge_fd 0
    bridge_maxwait 0
```

[darkod](https://ubuntuforums.org/member.php?u=946366) @ [UbuntuForums](https://ubuntuforums.org/showthread.php?t=2368932) suggests to:
>Delete the broadcast and network parameters.
>They are not needed, they are calculated from the address and netmask.
>And if you make an error in them it can mess things up. I have seen people putting wrong values for broadcast and network not understanding what they are. And less lines make the interfaces file cleaner

**Hint:** We are using a new network space 10.0.1.X to avoid any collisions with any other networks we may log in. Especially when our eth and wlan adapters are controlled by DHCP.  
You are free to choose any network space, even  the one set up by your local router.

**Read more:** [Debian Wiki - Bridging Network Connections](https://wiki.debian.org/BridgeNetworkConnections)  
**Read more:** [YouTube - Ubuntu 14.04 KVM Setup 1: Bridge Network](https://www.youtube.com/watch?v=cgLQl-FhSws)  


## Restart services ##

```sh
sudo service network-manager restart
sudo service networking restart
```

## Combining wireless and ethernet adapter (at server) ##

If a wireless connection to the Internet is used, the following code must be run during host's boot in order to provide the VMs internet through the bridge.

```sh
sudo iptables -t nat -A POSTROUTING -o <WIRELESS_ADAPTER> -j MASQUERADE
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

**Read more:** [Bridging Wifi to Ethernet on Ubuntu not working](http://superuser.com/questions/597834/bridging-wifi-to-ethernet-on-ubuntu-not-working)  
**Read more:** [Debian Wiki - Bridging with a wireless NIC](https://wiki.debian.org/BridgeNetworkConnections#Bridging_with_a_wireless_NIC)  

## Setup the interface (at client) ##

```sh
sudo nano /etc/network/interfaces
```

Add/Edit the file as follows:

```
auto eth0
iface eth0 inet static
    address 10.0.1.XXX
    netmask 255.255.255.0
    broadcast 10.0.1.255
    gateway 10.0.1.1
```

## Add name resolver (at client) ##

```sh
sudo nano /etc/resolv.conf
```

Add/Edit the following code. Instead of router's IP, an external DNS resolver IP can be provided (i.e. Google 8.8.8.8 and 8.8.4.4). Beware to insert one IP per line.

```
nameserver <ROUTER_IP>
```

**Troubleshooting:**
[nameserver 127.0.1.1 in resolv.conf won't go away!](
https://askubuntu.com/questions/627899/nameserver-127-0-1-1-in-resolv-conf-wont-go-away)
