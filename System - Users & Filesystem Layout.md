# System - Users & Filesystem Layout

## Strategy
- One user per person
- Common group (i.e. `common`)
- One private folder per user (`/mnt/storage/<username>`)
- One common folder (`/mnt/storage/<common-group>`)
- One ssh key-pair per user (_See the SSH guide for more!_)

## Prepare users and groups
Add a common group for all user/persons:
```bash
sudo addgroup <common-group>
```
Add a system user per person:
```bash
sudo adduser <username>
```
Add new user to common group:
```bash
sudo usermod -aG <common-group> <username>
```

## Create group's private folder
```bash
sudo mkdir /mnt/storage/<common-group>
sudo chown -R root:<common-group> /mnt/storage/<common-group>
sudo chmod -R 770 /mnt/storage/<common-group>
```
To retain `group` permissions in newly created files the gid must be set:
```bash
sudo chmod g+s /mnt/storage/<common-group>
```
To prevent other users deleting user owner's files set the sticky bit:
```bash
sudo chmod +t /mnt/storage/<common-group>
```
Set default ACL permissions
```bash
sudo setfacl -d -m g::rwx /mnt/storage/<common-group>
sudo setfacl -d -m o::--- /mnt/storage/<common-group>
```
Check the ACL settings:
```bash
getfacl /mnt/storage/<common-group>
```
\* **Note**: To enable ACL support, filesystems must be mounted with `acl` option.

## Create users' private
```bash
sudo mkdir /mnt/storage/<username>
sudo chown -R <username>:<username> /mnt/storage/<username>
sudo chmod -R 770 /mnt/storage/<username>
```

## Link to storage folder
Create a link called `storage` inside the `home` folder of every user pointing at the `/mnt/storage/<username>`.

Example:
```bash
ln -s SOURCE LINK
```
Logged in as $USERNAME create a link:
```bash
ln -s /mnt/storage/<username> /home/<username>/storage
```

## Appendix A - Resources
- [How to set default file permissions for all folders/files in a directory?](https://unix.stackexchange.com/questions/1314/how-to-set-default-file-permissions-for-all-folders-files-in-a-directory)
- [Shared Folders in Ubuntu with setgid and ACL](http://brunogirin.blogspot.com/2010/03/shared-folders-in-ubuntu-with-setgid.html)
