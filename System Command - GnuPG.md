# System Commands - GnuPG

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
- Valid 5y

## List private keys
```bash
gpg --list-secret-keys --keyid-format LONG
```

## Import/export private key
Export:
```bash
gpg --output mygpgkey_sec.asc --armor --export-secret-key <gpg_key_id>
```
Import:
```bash
gpg --allow-secret-key-import --import ~/mygpgkey_sec.gpg
```

## Import/export public key
Export:
```bash
gpg --output mygpgkey_pub.asc --armor --export <gpg_key_id>
```
Import:
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
