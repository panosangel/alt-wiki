# System - Root Account Hardening

## Protect `root` user

###  Change file accessibility
Make sure any file created by the root account is set to only be accessible to root by default. Run as **root**  the following command:
```bash
echo 'umask 0077' >> ~/.bashrc
```

### Disable command history
Prevent hackers from seeing the commands you have typed in the past which might expose passwords. Run as **root**  the following command:
```bash
echo 'set +o history' >> ~/.bashrc
```
