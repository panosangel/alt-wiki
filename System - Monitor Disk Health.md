# System - Monitor Disk Health

## Install smartmontools:
```bash
sudo apt-get install smartmontools
```

## Get general information about disk
```bash
sudo smartctl -i /dev/<disk-name>
```

## Check disk's health status
If SMART support is available but not enabled, it can be turned on with the following command: 
```bash
sudo smartctl -s on /dev/<disk-name>
```
To check the health status:
```bash
sudo smartctl -H /dev/<disk-name>
```
More detailed report can be found running:
```bash
sudo smartctl -a /dev/<disk-name>
```
Results of the last tests: 
```bash
sudo smartctl -l selftest /dev/<disk-name>
```

## Check disk's SMART capabilities
```bash
sudo smartctl -c /dev/<disk-name>
```

## Run a short test
```bash
sudo smartctl -t short /dev/<disk-name>
```

## Find and repair bad sectors
Save the bad sector list in a file:
```bash
sudo badblocks -v /dev/<partition-name> > badsectors.txt
```
For ext2/ext3/ext4 partitions:
```bash
sudo e2fsck -l badsectors.txt /dev/<partition-name>
```

## Appendix A - Sources
- [Test If Linux Server SCSI / SATA / SSD Hard Disk Going Bad](https://www.cyberciti.biz/tips/linux-find-out-if-harddisk-failing.html)
- [How to Check and Monitor Hard Disk Health on Linux with Smartmontools](https://www.maketecheasier.com/monitor-hard-disk-health-linux/)
- [TexMint - How to Check Bad Sectors or Bad Blocks on Hard Disk in Linux](https://www.tecmint.com/check-linux-hard-disk-bad-sectors-bad-blocks/)
