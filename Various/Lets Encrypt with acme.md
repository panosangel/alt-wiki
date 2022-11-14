# Let’s Encrypt - The ACME.sh version

## Install ACME Shell script

```bash
git clone https://github.com/acmesh-official/acme.sh.git
cd ./acme.sh
./acme.sh --install -m username@domain.tld
```
You `don't have to be root` then, although `it is recommended`.

The script installs a folder to `${HOME}/.acme.sh/` where all the import files will be stored including global ACME configuration, API keys etc.

**Note:** It's important to use a real email, in order to get notifications from the CA regarding the expiration certificates. 
If for any reason you haven't done it during installation, you could later add `ACCOUNT_EMAIL='username@domain.tld'` in `account.conf`

## Issue certificate - DNS API method

The following example is based on [Dynu](https://www.dynu.com/) provider. More info at [ACME.sh - DNS API dynu](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#24-use-dynu-api).

```shell
acme.sh --issue --server letsencrypt \
  --dns dns_dynu \
  -d domain.tld \
  --key-file       /path/to/keys/domain.tld/key.pem \
  --fullchain-file /path/to/keys/domain.tld/fullchain.pem \
  --reloadcmd     "service nginx force-reload"
```

**Parameters:**
- `--server letsencrypt`: Selects Let's Encrypt as the preferred CA
- `--dns dns_dynu`: Selects Dynu API. More APIs at [ACME.sh - DNS API](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
- `-d domain.tld`: The domain we want to get the certificate for. We can add as many as we want `-d ... -d ... -d ...`.
- `--key-file`: Path to copy the private key
- `--fullchain-file`: Path to copy the fullchain key
- `--reloadcmd`: Script/command to run after successful certificate issuance 

## Appendix A - Sources

- [An ACME Shell script: acme.sh](https://github.com/acmesh-official/acme.sh)
- [Let's Encrypt certificate with acme.sh instead of Certbot](https://decovar.dev/blog/2021/04/05/acme-sh-instead-of-certbot/)
