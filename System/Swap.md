# Swap

## 1. Existing swap
First check if the system has any swap configured.
```bash
swapon --show
```
If the output is blank, there is no swap configured so we can continue with this guide.

## 2. Create a Swap File
```bash
sudo fallocate -l 4G /swapfile
```
Now check if the file was created.
```bash
ls -lh /swapfile
```
If it was created correctly, you should see something like:
```none
-rw-r--r-- 1 root root 2.0G Aug 3 18:59 /swapfile
```

## 3. Configure Swap File
Make the swap file only accessible to root.
```bash
sudo chmod 600 /swapfile
```
Mark the file as a swap file.
```bash
sudo mkswap /swapfile
```
If it was successful, you should see something like
```none
Setting up swapspace version 1, size = 2 GiB (2147479552 bytes)
no label, UUID=00aafaee-51c9-46b3-a0fc-8240c134048e
```
Finally we will tell the system to start using our new swap file,
```bash
sudo swapon /swapfile
```
To verify that the swap is now available type:
```bash
sudo swapon --show
```
Result:
```none
NAME      TYPE  SIZE USED PRIO
/swapfile file  4G   0B   -2
```

## 4. Make it Persistent
This swap will only last until next reboot. In order to make it permanent, we will add it to the `/etc/fstab` file.
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

## 5. Some Final Tweaks
For a server, you should change the `swappiness` value to 10.
```bash
sudo sysctl vm.swappiness=10
```
Now change the `vfs_cache_pressure` value to 50.
```bash
sudo sysctl vm.vfs_cache_pressure=50
```
To make these two settings persist after next reboot, edit the following file:
```bash
sudo nano /etc/sysctl.conf
```
Add this to the bottom.
```none
vm.swappiness=10
vm.vfs_cache_pressure=50
```

## Appendix A - Sources
Copy of [Creating Swap Space on Ubuntu 18.04](https://devanswers.co/creating-swap-space-ubuntu-18-04/)
