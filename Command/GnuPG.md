# GnuPG

## Install

If not installed, run:
```shell
sudo apt-get install gnupg
```

## Help list

```shell
gpg -h
```

## Generate private key

```shell
gpg --full-generate-key
```
Choose:
- RSA and RSA (default)
- 4096bit size
- Valid 2y

## Manage Private Keys

### List private keys

```shell
gpg --list-secret-keys --keyid-format LONG
```

### Export private key

```shell
gpg --output mygpgkey_sec.asc --armor --export-secret-key <gpg_key_id>
```
or
```shell
gpg --output mygpgkey_sec.asc --armor --export-secret-keys --export-options export-backup <gpg_key_id>
```
Read more:  [How to export a GPG private key and public key to a file](https://unix.stackexchange.com/questions/481939/how-to-export-a-gpg-private-key-and-public-key-to-a-file)

### Import private key

```shell
gpg --allow-secret-key-import --import ~/mygpgkey_sec.gpg
```

## Manage Public Keys

### List public keys

```shell
gpg --list-keys --keyid-format LONG
```

### Export public key

```shell
gpg --output mygpgkey_pub.asc --armor --export <gpg_pubkey_id>
```

### Import public key

```shell
gpg --import ~/mygpgkey_pub.gpg
```

## Encrypt a file

```shell
gpg --armor -r <recipients_email> -e <file_to_encrypt>
```

## Decrypt a file

```shell
gpg -o <output_filename> -d <file_to_decrypt>
```

## Change pubkey expiration date

- Read #1: [How to change the expiration date of a GPG key](https://www.g-loaded.eu/2010/11/01/change-expiration-date-gpg-key/)
- Read #2: [Github - krisleech/renew-gpgkey.md](https://gist.github.com/krisleech/760213ed287ea9da85521c7c9aac1df0)

## Submit the public key online

```shell
gpg --keyserver keys.openpgp.org --send-keys <gpg_pubkey_id>
gpg --keyserver keyserver.ubuntu.com --send-keys <gpg_pubkey_id>
gpg --keyserver pgp.mit.edu --send-keys <gpg_pubkey_id>
```
or
```shell
gpg --export <email> | curl -T - https://keys.openpgp.org
```

## Search for a public key online

```shell
gpg --keyserver keys.openpgp.org --recv-keys <gpg_pubkey_id>
gpg --keyserver keyserver.ubuntu.com --recv-keys <gpg_pubkey_id>
gpg --keyserver pgp.mit.edu --recv-keys <gpg_pubkey_id>
```
and/or
```shell
gpg --auto-key-locate keyserver --locate-keys <email>
```

## Quick Guide ;-)

Source: https://serverfault.com/questions/86048/how-to-backup-gpg

Step 1. Export

```shell
gpg --export --armor your@id.here > your@id.here.pub.asc
gpg --export-secret-keys --armor your@id.here > your@id.here.priv.asc
gpg --export-secret-subkeys --armor your@id.here > your@id.here.sub_priv.asc
gpg --export-ownertrust > ownertrust.txt
```

Step 2. Import

```shell
gpg --import your@id.here.pub.asc
gpg --import your@id.here.priv.asc
gpg --import your@id.here.sub_priv.asc
gpg --import-ownertrust ownertrust.txt
```

Step 3. Trust imported key

```
gpg --edit-key your@id.here
gpg> trust
Your decision? 5
```

## Appendix A - Sources

- [keys.openpgp.org - Usage](https://keys.openpgp.org/about/usage)
- [How to Backup and Restore Your GPG Key](https://risanb.com/code/backup-restore-gpg-key/)
- [serverfault - How to backup GPG?](https://serverfault.com/questions/86048/how-to-backup-gpg)
