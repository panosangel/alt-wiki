# System - MergerFS & SnapRAID

## Disk Devices

List available disk devices.
```bash
lsblk -o +uuid,name
```

In our example they are the following:
- 1\*System disk (/dev/sda)
- 2\*Data disks (/dev/sdb,sdc)
- 1\*Parity disk (/dev/sdd)

## Disk Management

Make partitions for the disks.
_See the appropriate guide for more!_

## Modify Reserved Space (Optional)

```bash
sudo tune2fs -m 2 /dev/sdb1
sudo tune2fs -m 2 /dev/sdc1
# parity disk must be bigger than data disk
sudo tune2fs -m 1 /dev/sdd1
```

##  MergerFS installation

Find the latest stable release from the [Official Source](https://github.com/trapexit/mergerfs/releases). 
Currently, this version is `2.33.5` and we target `Ubuntu 22.04` so download and install at once:
```bash
wget https://github.com/trapexit/mergerfs/releases/download/2.33.5/mergerfs_2.33.5.ubuntu-jammy_amd64.deb && sudo dpkg -i mergerfs*.deb
```
If not present, `fuse` needs to be installed:
```bash
sudo apt-get install fuse
```

## Mount point creation

```bash
sudo mkdir /mnt/disk{1,2}
sudo mkdir /mnt/parity1
sudo mkdir /mnt/storage
```

## Create `fstab` entries

Assuming that the disks are formatted and ready to use, we need to auto-mount the filesystems  on startup.

Get disks' UUIDs:
```bash
lsblk -o +uuid,name
```
or
```bash
sudo blkid
```

Output:
```
sda2   25e6a918-f219-11e8-9682-08002771d528
sdb1   4bf67ef2-8196-45b7-9f32-11ad2c656ba4
sdc1   71c291ce-c124-466c-a09d-c7c413c61081
sdd1   1acb96a3-33f3-4ec8-808e-9566c6794e36
```

Edit fstab:
```
sudo nano /etc/fstab
```

Add the following at the end of the file
```
# Data Disks
UUID=4bf67ef2-8196-45b7-9f32-11ad2c656ba4    /mnt/disk1     ext4  defaults 0 2
UUID=71c291ce-c124-466c-a09d-c7c413c61081    /mnt/disk2     ext4  defaults 0 2

# MergerFS union
/mnt/disk*                                   /mnt/storage  fuse.mergerfs defaults,allow_other,use_ino,minfreespace=200G,dropcacheonclose=true,moveonenospc=true,fsname=>

# SnapRAID disks
UUID=1acb96a3-33f3-4ec8-808e-9566c6794e36    /mnt/parity1   ext4  defaults 0 0
```

Save and run the following commands:
```bash
sudo mount -a
```
```bash
df -Th
```

## Installing SnapRAID

In Ubuntu, we have two options for installing SnapRAID.
From PPA:
```bash
sudo add-apt-repository ppa:tikhonov/snapraid && sudo apt-get update
sudo apt-get install snapraid
```
or from sources (preferably) and the following script:
```bash
wget https://github.com/amadvance/snapraid/releases/download/v12.1/snapraid-12.1.tar.gz
tar xf snapraid-*.tar.gz
cd snapraid-*
./configure
make
make check
make install
```

Verify that SnapRAID is installed:
```bash
snapraid -V
```

In case you build the program in another system you need to run the following in the _target_ system:
```bash
/usr/bin/mkdir -p '/usr/local/bin'
/usr/bin/install -c snapraid '/usr/local/bin'
/usr/bin/mkdir -p '/usr/local/share/man/man1'
/usr/bin/install -c -m 644 snapraid.1 '/usr/local/share/man/man1'
```

## Configuring SnapRAID

Edit the configuration file:
```bash
sudo nano /etc/snapraid.conf
```

Add/edit the following lines. Be aware these changes are based on the current example:
```
parity /mnt/parity1/snapraid.parity

content /var/snapraid/snapraid.content
content /mnt/disk1/snapraid.content
content /mnt/disk2/snapraid.content

data d1 /mnt/disk1/
data d2 /mnt/disk2/

exclude *.unrecoverable
exclude /tmp/
exclude /lost+found/
exclude .DS_Store
exclude ._.DS_Store
exclude .Thumbs.db
exclude .Trashes

autosave 100
```

Save and sync for the first time. It may take a while...
```bash
sudo snapraid sync
```

## Automating SnapRAID

Using [snapraid-runner](https://github.com/Chronial/snapraid-runner) and `cron` we can keep the SnapRAID parity synced.

Download `snapraid-runner` script and config.
```bash
sudo mkdir /opt/snapraid-runner && cd /opt/snapraid-runner
sudo wget https://raw.githubusercontent.com/Chronial/snapraid-runner/master/snapraid-runner.py
sudo wget -O snapraid-runner.conf https://raw.githubusercontent.com/Chronial/snapraid-runner/master/snapraid-runner.conf.example
```

Edit config file:
```bash
sudo nano /opt/snapraid-runner/snapraid-runner.conf
```
```
[snapraid]
executable = /usr/bin/snapraid
config = /etc/snapraid.conf
deletethreshold = 250

[logging]
file = /var/log/snapraid-runner.log

[scrub]
enabled = true
percentage = 22
older-than = 10
```

Add an entry using `crontab`:
```bash
sudo crontab -e
```
```
0 3 * * * python2 /opt/snapraid-runner/snapraid-runner.py -c /opt/snapraid-runner/snapraid-runner.conf
```

## Appendix A - Sources

- [Perfect Media Server](https://perfectmediaserver.com/)
- [linuxserver.io - The Perfect Media Server 2017](https://blog.linuxserver.io/2017/06/24/the-perfect-media-server-2017/)
- [linuxserver.io - The Perfect Media Server 2016](https://blog.linuxserver.io/2016/02/02/the-perfect-media-server-2016/)
- [Disk Pooling in Linux with mergerFS](https://www.teknophiles.com/2018/02/19/disk-pooling-in-linux-with-mergerfs/)
- [Setting up SnapRAID on Ubuntu to Create a Flexible Home Media Fileserver](https://zackreed.me/setting-up-snapraid-on-ubuntu/)
- [MergerFS neat tricks](https://zackreed.me/mergerfs-neat-tricks/)
- ["check" and the parity drive](https://sourceforge.net/p/snapraid/discussion/1677233/thread/6e990e7a/)
