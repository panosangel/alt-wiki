# User & Group Management

## Check user info

If [USERNAME] is omitted, current user is implied.

```sh
id USERNAME
```

## Change password

```sh
passwd USERNAME
```

## Change user info (GECOS)

```sh
chfn USERNAME
```

## Change user shell

```sh
chsh -s SHELL USERNAME
```

OR

```sh
usermod -s SHELL USERNAME
```

OR

```sh
useradd -s SHELL USERNAME
```

## Check creation and expiration date of an account

```sh
chage -l USERNAME
```

## Add user to sudoers

visudo edits the `/etc/sudoers `file in a safe fashion.

```sh
visudo
```

or

```sh
usermod -aG sudo USERNAME
```

or

```sh
gpasswd -a USERNAME sudo
```

## Remove a user from sudoers

```sh
 gpasswd -d USERNAME sudo
```

or

```sh
delgroup USERNAME sudo
```
