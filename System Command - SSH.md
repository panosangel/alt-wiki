# System Commands - SSH

## Create keypair 
RSA 4096bit key with comment:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f outputfile
```

## Changing the passphrase of a private key
Change the passphrase of a private key file without altering it.
```bash
ssh-keygen -p
```

## Transfer public key to remote host

### The easy way:
```bash
ssh-copy-id -i filename <remote_username>@<remote_host>
```

### The manual way:
In the remote system, login as <remote_user> and run the following commands:
```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
In the **local** system run the command to display the key:
```bash
cat ~/.ssh/id_rsa.pub
```
Then add manually public key to authorized_keys file in the **remote** server:
```bash
nano ~/.ssh/authorized_keys
```

### Forwarding your SSH Credentials to Use on a Server
If you wish to be able to connect without a password to one server from within another server, you will need to forward your SSH key information. This will allow you to authenticate to another server through the server you are connected to, using the credentials on your local computer.

To start, you must have your SSH agent started and your SSH key added to the agent (see above). After this is done, you need to connect to your first server using the `-A` option. This forwards your credentials to the server for this session:
```bash
ssh -A <remote_username>@<first_remote_host>
```
From here, you can SSH into any other host that your SSH key is authorized to access. You will connect as if your private SSH key were located on this server.

## Easier SSH

### Basic settings
Edit client's ssh config
```bash
nano ~/.ssh/config
```
Add the following:
```
Host *
    ServerAliveInterval 120
```

 ### SSH Multiplexing
Save resources making SSH re-connections faster.
```bash
mkdir ~/.ssh/tmp
chmod 700 ~/.ssh/tmp
```
Edit client's ssh config:
```bash
nano ~/.ssh/config
```
Add the following:
```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/tmp/%h-%r-%p
    ControlPersist 900
```

### Create aliases
```bash
nano ~/.ssh/config
```
Add the following:
```
Host <ALIAS-NAME>
    HostName hostname.domain.tld
    Port <PORT-NUMBER>
    User <USERNAME>
```

## Popup notifications (not tested)
```bash
sudo apt-get install libnotify-bin
```
Create script
```bash
nano ~/bin/notify.sh
```
```bash
#!/bin/bash
 
hostname=$1;
username=$2;
title="SSH connection";
body="A new connection to $hostname as $username has been established";
level="low";
icon="/usr/share/icons/gnome/32x32/emblems/emblem-system.png";
 
[ ! $(tty -s) ] && notify-send -u "$level" -i "$icon" "$title" "$body";
```
```bash
nano ~/.ssh/config
```
```
Host *
    PermitLocalCommand yes
    LocalCommand ~/bin/notify.sh %h %r
```

## Tips & Hints

### Global client configuration
In the local machine, global client configuration can be set using the `/etc/ssh/ssh_config`.

### Manage SSH session
1. Send to background:
Press `ENTER` and type `~`+`CTRL`+`Z`
2. Bring to foreground:
`fg`
3. Display the backgrounded tasks:
`jobs`
4. Bring any of the tasks to the foreground by using the index in the first column with a percentage sign:
`fg %2`

### Disconnect from the Client-Side
Press `ENTER` and type  `~`+`.`

## Appendix A - Sources
- [Η φιλική πλευρά του SSH - μέρος 1ο](https://deltahacker.gr/friendly-ssh-p1/)
- [Η φιλική πλευρά του SSH - μέρος 2ο](https://deltahacker.gr/friendly-ssh-p2/)
- [DigitalOcean - SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- [Archived - About SSH and long running processes](http://krenel.org/about-ssh-and-long-long-running-processes.html)
