
# https://wiki.archlinux.org/index.php/Users_and_groups
# http://www.debianadmin.com/users-and-groups-administration-in-linux.html
# 




## Delete user ##

# -r: the user's home directory and mail spool should also be deleted. 
userdel -r username


## System Groups ##

# Find group
# If [username] is omitted, current user is implied.
groups [username]


## Add groups ##
groupadd [group]

## Change Primary/Secondary group ##





# Alternative method to add user to a group
# -a, --add: Add user to group
gpasswd --add username group
gpasswd -a OurUserName sudo

## Rename Group ##
# preserving gid 
groupmod -n [new_group] [old_group]

## Delete Group ##
groupdel [group]

## Remove users from group ##
gpasswd -d [user] [group]
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTE2NDQ2NDJdfQ==
-->