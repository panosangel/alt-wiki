# Disk Management

## Disk Devices

List available disk devices with a selection of useful columns:
```shell
lsblk -o +UUID,NAME,SIZE,MODEL,SERIAL,TYPE,MOUNTPOINT
```

If you’re using SATA/SAS HBAs, also nice:
```shell
ls -l /dev/disk/by-id/
```

## Create partitions in new disks

Lets assume that we are working on `/dev/sdX` disk. **WARNING: Select the correct disk!**
```shell
sudo parted -a optimal /dev/sdX
```

Create a new GPT disklabel (aka partition table):
```
(parted) mklabel gpt
```

Create one partition occupying all the space on the drive:
```
(parted) mkpart "Data Disk" ext4 1MiB 100%
```

Check that the results are correct:
```
(parted) print
```

There should be one partition occupying the entire drive. Save and quit "parted":
```
(parted) quit
```

### Scripting

Alternatively, if you want to avoid the prompts, it possible to run the command with `--script` flag.
**WARNING: There is no undo functionality ;)**

```
parted -a optimal /dev/sdX --script \
mklabel gpt \
mkpart Data-Disk ext4 1MiB 100%
```

## Format filesystem

To format the new partition as ext4 file system:
```shell
sudo mkfs -t ext4 /dev/sdX1
```
or
```shell
sudo mkfs.ext4 /dev/sdX1
```

When formatting the drive as ext3/ext4, 5% of the drive's total space is reserved for the super-user (root) so that the operating system can still write to the disk even if it is full. However, for disks that only contain data, this is not necessary.
Use `-m` flag to set the reservation percentage:
```shell
sudo mkfs.ext4 -m 3 /dev/sdX1
```

### Modify reserved space later (Optional)

You can adjust the percentage of reserved space with the "tune2fs" command, like this:
```shell
sudo tune2fs -m 3 /dev/sdX1
```
This example reserves 3% of space - change this number if you wish.

## Health checks

In order to catch data & metadata corruption early we need to run periodic fsck checks:

```shell
# Set time-based check every 1 month
sudo tune2fs -i 1m /dev/sdX1
```

and/or

```shell
# Also set a mount-count-based check every 30 mounts as a safety net
sudo tune2fs -c 30 /dev/sdX1
```

**Note:** `fsck` on `ext4` runs at boot time when the check is triggered — the system will pause during boot to run it. On a 1TB partition this typically takes a few minutes. This is expected and fine for a homelab.

Furthermore, we could mount the `ext4` filesystem with `errors=remount-ro` mount option in `/etc/fstab` for the root partition.
If corruption is detected at runtime, the filesystem goes read-only rather than silently serving bad data, which would make symptoms more obvious sooner.

## Appendix A - Sources

- [Ubuntu Official - Installing A New Hard Drive](https://help.ubuntu.com/community/InstallingANewHardDrive)
- [Arch Linux Wiki - Parted](https://wiki.archlinux.org/title/Parted) #interesting
- [Arch Linux Wiki - File Systems](https://wiki.archlinux.org/title/File_systems)
- [Zack Reed -SnapRAID + mergerfs on Ubuntu 24.04: a modern, flexible home storage stack](https://zackreed.me/posts/snapraid-mergerfs-on-ubuntu-24.04/#what-were-building-and-why)
