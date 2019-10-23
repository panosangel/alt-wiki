# System Configuration - UFW

## Basic operations 
Check status
```bash
sudo ufw status verbose
```
Enable firewall
```bash
sudo ufw enable
```
Disable firewall
```bash
sudo ufw disable
```
Disable UFW and delete all active rules
```bash
sudo ufw reset
```

## Set default policies
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default allow forward
```

## Predefined services
List service profiles:
```bash
sudo ufw app list
```
View profile details:
```bash
sudo ufw app info 'Nginx Full'
```
Enable a service
```bash
sudo ufw allow ssh
```

## Manage rules
List all rules (numbered)
```bash
sudo ufw status numbered
```
Delete a rule
```bash
sudo ufw delete <number_of_rule>
```

## Examples
Basic format:
```bash
sudo ufw <allow|deny> from <source_ip> to <destination_ip> port <port_number> proto <tcp|udp>
```
Rules:
```bash
sudo ufw allow from any to any port 20,21 proto tcp # comma separated ports
sudo ufw allow from 10.1.1.0/8 to any port 53 proto udp
sudo ufw allow from 192.168.1.108 # specific ip
sudo ufw allow from 192.168.1.0/24 # defined subnet
sudo ufw allow from 192.168.1.108 to any port 22
sudo ufw allow 22/tcp # short format
sudo ufw allow 6000:6007/tcp # range of ports
sudo ufw allow in on eth1 to any port 3306 # defined network interface
```

## Configuration
Edit configuration file:
```bash
sudo nano /etc/default/ufw
```

### Disable  IPv6
Change `IPV6=yes` to `#IPV6=yes`

## Appendix A - Sources
- [How To Set Up a Firewall with UFW on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)
- [How To Set Up a Firewall with UFW on Ubuntu 18.04](https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/)
