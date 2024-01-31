# Encrypt Data Disk

## Install software

```shell
sudo apt-get install cryptsetup
```

Run modprobe in the end to load the needed modules _without a reboot_.

```shell
sudo modprobe dm-crypt sha256 aes
```

## Gather information

Default options:
```shell
cryptsetup --help | tail -n 10
```

Benchmark the hardware:
```shell
cryptsetup benchmark
```

## Prepare the disk

### Find the disk

```shell
sudo fdisk -l
```

### Clear disk data

To just quickly wipe file systems (old data may remain):
```shell
wipefs -a <target_device>
```

If you have data stored on the hard drive to begin with, and if **the drive is a traditional spindle drive**, shred it first.
```shell
sudo shred --verbose --random-source=/dev/urandom --iterations=1 /dev/<target_device>
```
_Note_ For a 2TB disk, expect even the simplest form of shredding to last 8-12 hours depending on your hardware.

### Create partitions (optional)

You can create the LUKS container directly into a disk (i.e. /dev/sdb) instead of an existing partition (i.e. /dev/sdb1).  In case we don't want to encrypt the whole disk but a partition instead we need to create the partition(s) now.

With a fresh drive, wipe existing partitions and start over with a single new primary partition:
```shell
sudo fdisk /dev/<target_device>
```

Then create EXT4 filesystem:
```shell
sudo mkfs.ext4 /dev/<target_device>
```

Take a note of your UUID, as you'll use it for mounting the encrypted volume later on. You can use blkid to find it:
```shell
sudo blkid
```

## Create encrypted volume

```shell
sudo cryptsetup --verbose --verify-passphrase luksFormat /dev/<target_device>
```
Or the more advanced
```shell
sudo cryptsetup --verbose \ 
--type luks2 \
--hash sha512 --iter-time 5000 --use-random \ 
luksFormat /dev/<target_device>
```

## Map volume

Then we will create a mapper that identifies the encrypted volume (really a dm-crypt target); it will be named "NameOfYourChoice".
```shell
sudo cryptsetup luksOpen /dev/<target_device> <NameOfYourChoice>  
```

You can see a mapping name `/dev/mapper/<NameOfYourChoice>` after successful verification of the supplied key material which was created with luksFormat command extension:
```shell
ls -l /dev/mapper/<NameOfYourChoice>
```

You can use the following command to see the status for the mapping:
```shell
sudo cryptsetup -v status <NameOfYourChoice>
```

## Format encrypted volume

### Wiping newly created mount (optional)

First, you need to write zeros to `/dev/mapper/<NameOfYourChoice>` encrypted device. This will allocate block data with zeros.    
This ensures that outside world will see this as random data i.e. it protects against disclosure of usage patterns:
```shell
sudo dd if=/dev/zero of=/dev/mapper/<NameOfYourChoice> bs=8M status=progress  
```

### Partitioning

Now, select a file system for your encrypted volume. I've opted for EXT4. This will take a minute or so.
```shell
sudo mkfs -t ext4 -m 1 /dev/mapper/<NameOfYourChoice>
```

## Test the encrypted partition

Create a mount point:
```shell
sudo mkdir /media/<AnotherNameOfYourChoice>
```

Mount the partition
```shell
sudo mount /dev/mapper/<NameOfYourChoice> /media/<AnotherNameOfYourChoice>
```

Now, you can start placing your files into it. However, you will be able to do that only with `sudo` because regular users do not have access to this folder.

Let’s change the ownership of this encrypted folder, to give access to regular users:
```shell
sudo chown -R `whoami`:<user_group> /media/<AnotherNameOfYourChoice>  
```

Now test it by create a file as a regular user without sudo.
```shell
touch /media/<AnotherNameOfYourChoice>/test.txt
```
Try  unmounting to avoid silent mount errors:  
_See instructions below_

## Unmount and secure data

```shell
sudo umount /media/<AnotherNameOfYourChoice>sudo cryptsetup luksClose <NameOfYourChoice>
```

## Mount or remount encrypted partition

```shell
sudo cryptsetup luksOpen /dev/<target_device> <NameOfYourChoice>  
sudo mount /dev/mapper/<NameOfYourChoice> /media/<AnotherNameOfYourChoice>
sudo df -H
sudo mount
```

## Automount on startup (**@TODO Verify**)

If you run a headless server, you would like your encrypted volume to automount on boot. In this case you need to create a key file that can unlock your volume automatically.
```shell script  
sudo dd if=/dev/urandom of=/root/my-keyfile bs=1024 count=128  
sudo chmod 400 /root/my-keyfile```  
Add the key file to encrypted partition:  
```shell script  
sudo cryptsetup luksAddKey /dev/<target_device> /root/my-keyfile```  
Now, tell the system that you have an encrypted volume, how to find it and how to open the secure volume with a key file:  
```shell script  
sudo nano /etc/crypttab<NameOfYourChoice>    UUID=<UUID_OF_PARTITION>   /root/my-keyfile    luks  
```  
- Note 1. Use the previously identified UUID in the crypttab. Do NOT use the mapper UUID. Use the partition (i.e. sdb1) UUID.
- Note 2. If you don't want to automount the encrypted volume, replace /root/my-keyfile with the keyword `none`. You'll be prompted for the passphrase upon boot. This is NOT a good idea for headless servers.  
  When the crypttab is to your liking, tell the system to mount the encrypted volume to a mount point on boot with fstab:
```shell script  
sudo nano /etc/fstab/dev/mapper/<NameOfYourChoice>    /media/<AnotherNameOfYourChoice> ext4    defaults,rw 0 0  
```  
PRO TIP! If mounting fails between attempts where you change the contents of fstab, try:
```shell script  
sudo systemctl daemon-reload```  
  
## Backup and restore headers
  
Backup:  
```shell
sudo cryptsetup luksHeaderBackup --header-backup-file <file> <target_device>  
```

Restore:
```shell
sudo cryptsetup luksHeaderRestore --header-backup-file <file> <target_device>  
```

**Warning:** If you are unsure about a header to be restored, make a backup of the current one first!

You can also test the header-file without restoring it by using the --header option for a detached header like this:
```shell  
sudo cryptsetup --header <file> luksOpen <target_device> /dev/mapper/<NameOfYourChoice
```
More: [6. Backup and Data Recovery](https://gitlab.com/cryptsetup/cryptsetup/-/wikis/FrequentlyAskedQuestions#6-backup-and-data-recovery)

## Manage keys

LUKS containers can actually have multiple passphrases or key files associated with them, up to eight (LUKS1) or thirty-two (LUKS2).

To start, take a look at your drive and see how many keys it has. Chances are, you’ll only see key slot 0 occupied. That’s the first one.
```shell
sudo cryptsetup luksDump <target_device> | grep -i key
```

Add a key:
```shell
sudo cryptsetup luksAddKey <target_device>
```

Change a key:
```shell
cryptsetup luksChangeKey <target_device>
```
Note: `--key-slot`, `-S` `<target_key_slot_number>` can be used to target a specific slot.

Delete the key of a specific slot:
```shell
sudo cryptsetup luksKillSlot <target_device> <target_key_slot_number>  
```

Delete the key matched the one to be provided:
```shell
sudo cryptsetup luksRemoveKey <target_device>
```

## Appendix

### Sources

- [wikibooks - Cryptsetup](https://en.wikibooks.org/wiki/Cryptsetup)
- [GitLab - Frequently Asked Questions Cryptsetup/LUKS](https://gitlab.com/cryptsetup/cryptsetup/-/wikis/FrequentlyAskedQuestions)
- [gentoo linux - Dm-crypt full disk encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption)
- [Debian Bookworm Manpages](https://manpages.debian.org/bookworm/cryptsetup-bin/cryptsetup.8.en.html)
- [Archlinux - dm-crypt/Device encryption](https://wiki.archlinux.org/index.php/Dm-crypt/Device_encryption)
- [The awesome garage - Encrypt external hard drives with Linux](https://theawesomegarage.com/blog/encrypt-external-hard-drives-with-linux)
- [Average Linux User - Encrypt your Hard Drive in Linux](https://averagelinuxuser.com/encrypt-hard-drive-in-linux/)
- [makeuseof - How to Encrypt Your Personal Data on Linux](https://www.makeuseof.com/tag/encrypt-personal-data-linux/)
- [AskUbuntu - How to change LUKS passphrase?](https://askubuntu.com/questions/95137/how-to-change-luks-passphrase)
- [Make Tech Easier - How to Change Your LUKS Encryption Passphrase](https://www.maketecheasier.com/change-luks-encryption-passphrase/)

### More

- [LUKS On-Disk Format Specification](https://gitlab.com/cryptsetup/cryptsetup/wikis/LUKS-standard/on-disk-format.pdf)
- [Cloudflare - Speeding up Linux disk encryption](https://blog.cloudflare.com/speeding-up-linux-disk-encryption/)
