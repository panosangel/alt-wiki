# System - Datetime Sync

Check date and time:
```bash
date
```
List all time zones
```bash
timedatectl list-timezones
```
Set the appropriate one. 
```bash
sudo timedatectl set-timezone <timezone>
```
Time synchronization is enabled on Ubuntu by default using the `timesyncd` service.
```bash
timedatectl
```
Ensure that `System clock synchronized` and `systemd-timesyncd.service active` are set to `yes`. If not, run:
```bash
sudo timedatectl set-ntp on
```

## Appendix A - Sources
- Copy from [Ubuntu: Change Timezones and Set Up Time Synchronization](﻿﻿﻿https://devanswers.co/ubuntu-change-timezone-synchronize-ntp/)
