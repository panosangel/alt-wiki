# System Information

## Software

Find kernel version:
```shell script
uname -a
```

Find distro version:
```shell script
cat /etc/os-release
```

## Hardware

General information:
```shell script
inxi -Fxzr
```

Find RAM speed:
```shell script
sudo dmidecode --type 17
```