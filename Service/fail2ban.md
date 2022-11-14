# fail2ban

## Basic configuration
Install package:
```bash
sudo apt-get install fail2ban
```
If package _cannot_ be found enable the `universe` repository:
```bash
sudo add-apt-repository universe
```
Make a copy of the default `fail2ban` configuration file and work on that one.
```bash
sudo cp /etc/fail2ban/fail2ban.conf /etc/fail2ban/fail2ban.local
```
Also make a copy of the default `jail` configuration file and work on that one.
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

## Secure services
Let's edit the `jail` configuration file.
```bash
sudo nano /etc/fail2ban/jail.local
```

### Default section
In the `[default]` section we are interested on setting the following:
```
# "bantime" is the number of seconds that a host is banned.
bantime  = 5m
# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime = 10m
maxretry = 5
```
```
ignoreip = 127.0.0.1/8 192.168.1.101/24
```

### Service section
To enable a daemon protection add the `enabled = true` in the appropriate section  i.e.`[sshd]` as shown below:
```
enabled = true
```
If the default service port has been changed:
```
port = 2222
```
Furthermore `maxretry`, `findtime` and `bantime` can be set per service overriding the default settings.

**Choose to monitor ONLY the the active system services otherwise the fail2ban service will exit with error.**

## Appendix A - Sources
[Use Fail2ban to Secure Your Server](https://www.linode.com/docs/security/using-fail2ban-for-security/)
