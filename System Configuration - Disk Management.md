# System Configuration - Disk Management

## Disk Devices
List available disk devices with names and uuids:
```bash
lsblk -o +uuid,name
```

## Create partitions in new disks
Lets assume that we are working on `/dev/sdb` disk.
```bash
sudo parted -a optimal /dev/sdb
```
Create a new GPT disklabel (aka partition table):
```
(parted) mklabel gpt
```
Set the default unit to TB:
```
(parted) unit TB
```
Create one partition occupying all the space on the drive:
```
(parted) mkpart primary ext4 0% 100%
```
Check that the results are correct:
```
(parted) print
```
There should be one partition occupying the entire drive. Save and quit "parted":
```
(parted) quit
```
To format the new partition as ext4 file system (best for use under Ubuntu):
```bash
sudo mkfs -t ext4 /dev/sdb1
```
or
```bash
sudo mkfs.ext4 /dev/sdb1
```

## Modify Reserved Space (Optional)
When formatting the drive as ext23/ext34, 5% of the drive's total space is reserved for the super-user (root) so that the operating system can still write to the disk even if it is full. However, for disks that only contain data, this is not necessary.

**Note:** You may run this command on a fat32 file system, but it will do nothing; therefore, I highly recommend not running it.

You can adjust the percentage of reserved space with the "tune2fs" command, like this:
```bash
sudo tune2fs -m 3 /dev/sdb1
```
This example reserves 3% of space - change this number if you wish.

## Appendix A - Sources
- [Ubuntu Official - Installing A New Hard Drive](https://help.ubuntu.com/community/InstallingANewHardDrive)


