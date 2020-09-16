# DNSMASQ

## Backup and edit config file

```sh
sudo cp /etc/dnsmasq.conf /etc/dnsmasq.conf.ORIGINAL
sudo nano /etc/dnsmasq.conf
```
## Basic options

```
# Listen on this specific port instead of the standard DNS port (53).
# Setting this to zero completely disables DNS function, leaving only DHCP and/or TFTP.
#port=5353
```

```
# Never forward plain names (without a dot or domain part)
domain-needed
```

```
# Never forward addresses in the non-routed address spaces.
bogus-priv
```

```
# Query with each server strictly in the order in resolv.conf
strict-order
```

```
# If you don't want dnsmasq to read /etc/resolv.conf or any other file, getting its servers from this file instead (see below), then uncomment this.
no-resolv
```

```
# If you don't want dnsmasq to poll /etc/resolv.conf or other resolv files for changes and re-read them then uncomment this.
no-poll
```

```
# If you don't want dnsmasq to read /etc/hosts, uncomment the following line.
no-hosts
```

```
# If you want it to read another file, as well as /etc/hosts, use
# this. If a directory is given, then read all the files contained in that directory.
addn-hosts=/etc/dnsmasq_hosts
```

```
# Add other name servers here, with domain specs if they are for non-public domains.
# Query the specific domain name to the specific DNS server.
server=/localnet/192.168.1.2
server=/server.education/10.0.0.10
server=8.8.8.8
server=8.8.4.4
```

```
# Add local-only domains here, queries in these domains are answered from /etc/hosts or DHCP only.
local=/mydomain.local/
```

```
# If you want dnsmasq to listen for DHCP and DNS requests only on specified interfaces (and the loopback) give the name of the interface (eg eth0) here.
# Repeat the line for more than one interface.
interface=eth0
interface=br1
```

```
# Set this (and domain: see below) if you want to have a domain automatically added to simple names in a hosts-file.
expand-hosts
```

```
# Set the domain for dnsmasq. this is optional, but if it is set, it does the following things.
# 1) Allows DHCP hosts to have fully qualified domain names, as long as the domain part matches this setting.
# 2) Sets the "domain" DHCP option thereby potentially setting the domain of all systems configured by DHCP
# 3) Provides the domain part for "expand-hosts"
domain=mydomain.local
```

```
# Uncomment this to enable the integrated DHCP server, you need to supply the range of addresses available for lease and optionally a lease time.
# If you have more than one network, you will need to repeat this for each network on which you want to supply DHCP service.
dhcp-range=10.0.1.11,10.0.1.100,12h
```

```
# When a host is requesting an IP address via DHCP also tell it the gateway to use.
dhcp-option=option:router,192.168.0.1
# When a host is requesting an IP address via DHCP also tell it the NTP to use.
dhcp-option=option:ntp-server,192.168.0.5
#
dhcp-option=option:dns-server,10.0.0.10
#
dhcp-option=option:netmask,255.255.255.0
```

```
# Adapted for a typical dnsmasq installation where the host running dnsmasq is also the host running samba.
# You may want to uncomment some or all of them if you use Windows clients and Samba.
dhcp-option=19,0 # option ip-forwarding off
dhcp-option=44,0.0.0.0 # set netbios-over-TCP/IP nameserver(s) aka WINS server(s)
dhcp-option=45,0.0.0.0 # netbios datagram distribution server
dhcp-option=46,8 # netbios node type
```

```
# Any machine saying they are hostname = ‘mylaptop’ gets this IP address
dhcp-host=mylaptop,192.168.0.199,36h
```

```
# Set the limit on DHCP leases, the default is 150
dhcp-lease-max=200
```

```
# Set the DHCP server to authoritative mode. In this mode it will barge in and take over the lease for any client which broadcasts on the network, whether it has a record of the lease or not.
# This avoids long timeouts when a machine wakes up on a new network. DO NOT enable this if there's the slightest chance that you might end up accidentally configuring a DHCP server for your campus/company accidentally.
# The ISC server uses the same option, and this URL provides more information: http://www.isc.org/files/auth.html

dhcp-authoritative
```

```
# Specify  an  IP  address  to  return  for  any host in the given domains.
# Queries in the domains are never forwarded and  always replied  to  with  the specified IP address which may be IPv4 or IPv6.
address=/doubleclick.net/127.0.0.1
```

```
# The following line shows how to make dnsmasq serve an arbitrary PTR record.
# This is useful for DNS-SD. (Note that the domain-name expansion done for SRV records _does_not occur for PTR records.)
#ptr-record=_http._tcp.dns-sd-services,"New Employee Page._http._tcp.dns-sd-services"

ptr-record=2.1.168.192.in-addr.arpa.,"mydomain.local"
address=/mydomain.local/192.168.1.2
```

```
# Include another lot of configuration options.
#conf-file=/etc/dnsmasq.more.conf
#conf-dir=/etc/dnsmasq.d
```

## Entry in /etc/hosts

IP 10.0.0.30 is of machine running dnsmasq
dlp.mydomain.local is [???]

```
10.0.0.30   dlp.mydomain.local dlp
```

## Edit /etc/network/interfaces

Change `dns-nameservers` to the dnsmasq ip

```
dns-nameservers 10.0.0.30
```

## Apply new settings

Restart dnsmasq server

```
systemctl restart dnsmasq
```

Restart networking service

```

```

## Verify settings

Check DNS entry

```sh
dig dlp.mydomain.local.
```

Check PTR entry
```
dig -x 10.0.0.30
```
