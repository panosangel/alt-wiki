# Command - GnuPG

## Install
If not installed, run:
```bash
sudo apt-get install gnupg
```

## Help list
```bash
gpg -h
```

## Generate private key
```bash
gpg --full-generate-key
```
Choose: 
- RSA and RSA (default)
- 4096bit size
- Valid 1y

## Manage Private Keys

**List private keys**
```bash
gpg --list-secret-keys --keyid-format LONG
```

**Export private key**
```bash
gpg --output mygpgkey_sec.asc --armor --export-secret-key <gpg_key_id>
```

**Import private key**
```bash
gpg --allow-secret-key-import --import ~/mygpgkey_sec.gpg
```

## Manage Public Keys

**List public keys**
```bash
gpg --list-keys --keyid-format LONG
```

**Export public key**
```bash
gpg --output mygpgkey_pub.asc --armor --export <gpg_pubkey_id>
```

**Import public key**
```bash
gpg --import ~/mygpgkey_pub.gpg
```

## Encrypt a file
```bash
gpg --armor -r <recipients_email> -e <file_to_encrypt>
```

## Decrypt a file
```bash
gpg -o <output_filename> -d <file_to_decrypt>
```

## Change pubkey expiration date
Read at: [How to change the expiration date of a GPG key](https://www.g-loaded.eu/2010/11/01/change-expiration-date-gpg-key/)

## Submit the public key online
```bash
gpg --keyserver pgp.mit.edu --send-keys <gpg_pubkey_id>
```

## Search for a public key online
```bash
gpg --keyserver pgp.mit.edu --search <email>
```
