# Rsync

## Basic Usage
Copy the contents of the source folder to the destination folder:
```shell script
rsync -r <source_folder>/ <destination_folder>
```
Note:  
- The option `-r` means recursive  
- The trailing `/` in the source folder means the contents and not the folder itself

### Archive syncing
Syncs recursively and preserves symbolic links, special and device files, modification times, group, owner, and permissions.
```shell script
rsync -a <source_folder>/ <destination_folder>
```
Note: 
- The option `-a`, `--archive` is equal to `-rlptgoD`
- Does not preserve hardlinks

### Dry run
The option `-n`, `--dry-run` performs a trial run that doesn't make any changes. Furthermore, produces mostly the same output as a real run.

### Delete
By default, rsync does not delete anything from the destination folder. To change that behavior use `--delete`.

### Show progress
 The option `--progress` shows a useful progress bar. We can combine `--progress` with `--partial` using the `-P`.

### Checksum
The option `-c`, `--checksum` checks if the files have been changed and are in need of a transfer. 
Without this option, rsync uses a "quick check" that (by default) checks if each file's size and time of last modification match between the sender and receiver. 
This option changes this to compare a 128-bit checksum for each file that has a matching size. 
Generating the checksums means that both sides will expend a lot of disk I/O reading all the data in the files in the transfer (and this is prior to any reading that will be done to transfer changed files), so this can slow things down significantly.

### Exclude
The option `---exclude=<pattern>` can be used to exclude files or folder matching the provided pattern. For more information read the manual.

### Verbose
The option `-v` , `--verbose`

## Remote copy (through SSH) 

```shell script
rsync -vaP <username>s@<domain_name_or_IP>:<source_folder> <destination_folder>
```

## Appendix A - Sources
- [Digital Ocean - How To Use Rsync to Sync Local and Remote Directories on a VPS](https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps)
- [Computer Hope - Linux rsync command](https://www.computerhope.com/unix/rsync.htm)
