# Encrypt Data Disk

## Install software
```shell script
sudo apt-get install cryptsetup
```
Run modprobe in the end to load the needed modules _without a reboot_.
```shell script
sudo modprobe dm-crypt sha256 aes
```

## Gather information
Default options:
```shell script
cryptsetup --help | tail -n 10
```
Benchmark the hardware:
```shell script
cryptsetup benchmark
```

## Prepare the disk
Find the disk
```shell script
sudo fdisk -l
```
**(OPTIONAL)**  If you have data stored on the hard drive to begin with, and if **the drive is a traditional spindle drive**, shred it first.  
To just quickly wipe file systems (old data may remain):
```shell script
wipefs -a <target device>
```
OR  

**(OPTIONAL)** For a 2TB disk, expect even the simplest form of shredding to last 8-12 hours depending on your hardware.
```shell script
sudo shred --verbose --random-source=/dev/urandom --iterations=1 /dev/<device_name>
```
With a fresh drive, wipe existing partitions and start over with a single new primary partition:
```shell script
sudo fdisk /dev/<device_name>
```
Then create EXT4 filesystem:
```shell script
sudo mkfs.ext4 /dev/<partition_name>
```
Take a note of your UUID, as you'll use it for mounting the encrypted volume later on. You can use blkid to find it:
```shell script
sudo blkid
```
Note: You can create the LUKS container directly into a disk (i.e. /dev/sdb) instead of an existing partition (i.e. /dev/sdb1).

## Configure encrypted volume
```shell script
sudo cryptsetup --verbose --verify-passphrase luksFormat /dev/<partition_name>
```
Or the more advanced
```shell script
sudo cryptsetup --verbose \
                --type luks2 \
                --hash sha512 --iter-time 5000 --use-random \
                luksFormat /dev/<partition_name>
```
This will create a mapper that identifies your encrypted volume (really a dm-crypt target); it will be named "NameOfYourChoice".
```shell script
sudo cryptsetup luksOpen /dev/<partition_name> <NameOfYourChoice>
```
You can see a mapping name /dev/mapper/<NameOfYourChoice> after successful verification of the supplied key material which was created with luksFormat command extension:
```shell script
ls -l /dev/mapper/<NameOfYourChoice>
```
You can use the following command to see the status for the mapping:
```shell script
cryptsetup -v status <NameOfYourChoice>
```

## Format encrypted volume
**(OPTIONAL)** First, you need to write zeros to /dev/mapper/<NameOfYourChoice> encrypted device. This will allocate block data with zeros.  
This ensures that outside world will see this as random data i.e. it protects against disclosure of usage patterns: 
```shell script
sudo dd if=/dev/zero of=/dev/mapper/<NameOfYourChoice> bs=8M status=progress
```
Now, select a file system for your encrypted volume. I've opted for ext4. This will take a minute or so.
```shell script
sudo mkfs -t ext4 -m 1 /dev/mapper/<NameOfYourChoice>
```

## Test the encrypted partition
Create a mount point:
```shell script
sudo mkdir /media/<AnotherNameOfYourChoice>
```
Mount the partition
```shell script
sudo mount /dev/mapper/<NameOfYourChoice> /media/<AnotherNameOfYourChoice>
```
Now, you can start placing your files into it. However, you will be able to do that only with `sudo` because regular users do not have access to this folder.  
Let’s change the ownership of this encrypted folder, to give access to regular users:
```shell script
sudo chown -R `whoami`:<user_group> /media/<AnotherNameOfYourChoice>
```
Now test it by create a file as a regular user without sudo.
```shell script
touch /media/<AnotherNameOfYourChoice>/test.txt
```
Try  unmounting to avoid silent mount errors:
_See instructions below_

## Unmount and secure data
```shell script
sudo umount /media/<AnotherNameOfYourChoice>
sudo cryptsetup luksClose <NameOfYourChoice>
```

## Mount or remount encrypted partition
```shell script
sudo cryptsetup luksOpen /dev/<partition_name> <NameOfYourChoice>
sudo mount /dev/mapper/<NameOfYourChoice> /media/<AnotherNameOfYourChoice>
sudo df -H
sudo mount
```

## Automount on startup (**@TODO Verify**)  
If you run a headless server, you would like your encrypted volume to automount on boot. In this case you need to create a key file that can unlock your volume automatically.
```shell script
sudo dd if=/dev/urandom of=/root/my-keyfile bs=1024 count=128
sudo chmod 400 /root/my-keyfile
```
Add the key file to encrypted partition:
```shell script
sudo cryptsetup luksAddKey /dev/<partition_name> /root/my-keyfile
```
Now, tell the system that you have an encrypted volume, how to find it and how to open the secure volume with a key file:
```shell script
sudo nano /etc/crypttab
<NameOfYourChoice>    UUID=<UUID_OF_PARTITION>   /root/my-keyfile    luks
```
- Note 1. Use the previously identified UUID in the crypttab. Do NOT use the mapper UUID. Use the partition (i.e. sdb1) UUID. 
- Note 2. If you don't want to automount the encrypted volume, replace /root/my-keyfile with the keyword `none`. You'll be prompted for the passphrase upon boot. This is NOT a good idea for headless servers.
When the crypttab is to your liking, tell the system to mount the encrypted volume to a mount point on boot with fstab:
```shell script
sudo nano /etc/fstab
/dev/mapper/<NameOfYourChoice>    /media/<AnotherNameOfYourChoice> ext4    defaults,rw 0 0
```
PRO TIP! If mounting fails between attempts where you change the contents of fstab, try:
```shell script
sudo systemctl daemon-reload
```

## Backup and restore
Backup:
```shell script
sudo cryptsetup luksHeaderBackup --header-backup-file <file> <device>
```
Restore:
```shell script
sudo cryptsetup luksHeaderRestore --header-backup-file <file> <device>
```
If you are unsure about a header to be restored, make a backup of the current one first! 
You can also test the header-file without restoring it by using the --header option for a detached header like this:
```shell script
sudo cryptsetup --header <file> luksOpen <device> /dev/mapper/<NameOfYourChoice
```
More: [6. Backup and Data Recovery](https://gitlab.com/cryptsetup/cryptsetup/-/wikis/FrequentlyAskedQuestions#6-backup-and-data-recovery)

## Manage keys
@TODO

## Appendix A - Sources
- [wikibooks - Cryptsetup](https://en.wikibooks.org/wiki/Cryptsetup)
- [The awesome garage - Encrypt external hard drives with Linux](https://theawesomegarage.com/blog/encrypt-external-hard-drives-with-linux)
- [Average Linux User - Encrypt your Hard Drive in Linux](https://averagelinuxuser.com/encrypt-hard-drive-in-linux/)
- [makeuseof - How to Encrypt Your Personal Data on Linux](https://www.makeuseof.com/tag/encrypt-personal-data-linux/)
- [Debian Buster Manpages](https://manpages.debian.org/buster/cryptsetup-bin/cryptsetup.8.en.html)
- [gentoo linux - Dm-crypt full disk encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption)

## Appendix B - More
- [Archlinux - dm-crypt/Device encryption](https://wiki.archlinux.org/index.php/Dm-crypt/Device_encryption)
- [GitLab - Frequently Asked Questions Cryptsetup/LUKS](https://gitlab.com/cryptsetup/cryptsetup/-/wikis/FrequentlyAskedQuestions)
- [LUKS On-Disk Format Specification](https://gitlab.com/cryptsetup/cryptsetup/wikis/LUKS-standard/on-disk-format.pdf)
- [Cloudflare - Speeding up Linux disk encryption](https://blog.cloudflare.com/speeding-up-linux-disk-encryption/)
