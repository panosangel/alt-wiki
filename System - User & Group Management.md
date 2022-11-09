# User & Group Management

## Display users

```shell
cat /etc/passwd
```

or

```shell
getent passwd
```

## Add user (the easy way)

```shell
adduser USERNAME
```

## Add user (the advanced way)

```shell
useradd USERNAME
```

**Optional:**
- `--uid UID` or `-u UID`  
  Option to create a user with a specific UID
- `--gid GID` or `-g GID`  
  Option allows you to create a user with a specific initial login group. You can specify either the group name or the GID number.   
  The group name or GID must already exist.
- `-G GROUP1,GROUP2,GROUP3[...]`  
  Add secondary groups. Groups must already exist.
- `--shell SHELL` or `-s SHELL`  
  Allows you to specify the new user’s login shell.
- `--comment COMMENT` or `-c`  
  Allows you to add a short description for the new user.

Example:
```shell
useradd -m -u 2001 -g MYGROUP -G sudo,OTHERGROUP -s /user/bin/zsh -c "This is a gret user"
```

## Check user info

If `USERNAME` is omitted, current user is implied.

```shell
id USERNAME
```

### Check creation and expiration date of an account

```shell
chage -l USERNAME
```

## Set/Change password

```shell
passwd USERNAME
```

## Change user properties

Generally using `usermod` with some flags from `useradd` we can change the respective properties.

For example `usermod -c "This is my account details" USERNAME` will change account info (GECOS).  
And `usermod -u 1500 USERNAME` will change uid of user to 1500.

## Change user shell

```shell
chsh -s SHELL USERNAME
```

or

```shell
usermod -s SHELL USERNAME
```

## Lock user's account

```shell
 usermod -s /sbin/nologin USERNAME
```

or

```shell
usermod -L USERNAME
```

or

```shell
passwd -l USERNAME
```

## Remove user

```shell
userdel [OPTIONS] USERNAME
```

**Optional:**
- `--remove` or `-r`  
  Remove home and mail spool
- `--force` or `-f`  
  Forcefully remove the user account, even if the user is still logged in or if there are running processes that belong to the user.

## Add user to a group

```shell
usermod -aG GROUP USERNAME
```

## Remove a user from a group

```shell
gpasswd -d USERNAME GROUP
```

## Show all groups

```shell
cat /etc/group
```

or

```shell
getent group
```

## Show user's groups

```shell
groups USERNAME
```

## Create group

```shell
groupadd NEWGROUP
```

**Optional:**
- `--gid GID` or `-g GID`  
  Create a group with a specific GID.

Example:

```shell
groupadd -g 1234 NEWGROUP
```

## Delete group

```shell
delgroup GROUP
```

## Source

- [Linuxize - How to Create Users in Linux (useradd Command)](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/)
- [Linuxize - How to Delete/Remove Users in Linux (userdel Command)](https://linuxize.com/post/how-to-delete-users-in-linux-using-the-userdel-command/)
- [Linuxize - How to Add User to Group in Linux](https://linuxize.com/post/how-to-add-user-to-group-in-linux/)
- [Linuxize - How to Create Groups in Linux (groupadd Command))](https://linuxize.com/post/how-to-create-groups-in-linux/)
- [Linuxize - How to Delete Group in Linux (groupdel Command)](https://linuxize.com/post/how-to-delete-group-in-linux/)
- [Add a User to a Group (or Second Group) on Linux](https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)
- [UNIX / Linux : How to lock or disable an user account](https://www.thegeekdiary.com/unix-linux-how-to-lock-or-disable-an-user-account/)

### Change user and group ID

- [How to (Correctly) Change the UID and GID of a user/group in Linux](https://www.thegeekdiary.com/how-to-correctly-change-the-uid-and-gid-of-a-user-group-in-linux/)
- [How to Change a USER and GROUP ID on Linux For All Owned Files](https://www.cyberciti.biz/faq/linux-change-user-group-uid-gid-for-all-owned-files/)
