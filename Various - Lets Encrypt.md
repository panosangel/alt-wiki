# Various - Let’s Encrypt

## Install Certbot
```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

## Issue certificate - DNS method (manual)
We assume that port 80 and/or port 443 is not available in our system. As a result we must use `dns` method. Manual mode is selected in case our DNS domain registrar is not supported. The method is pretty straight forward and requires only one DNS (TXT) record entry.
```bash
sudo certbot certonly \
  --manual \
  --preferred-challenges dns \
  --email panagiotis@angelidakis.net \
  --manual-public-ip-logging-ok \
  --agree-tos \
  --no-eff-email
```
Then we list all the domains we want to secure with the certificate. Manual DNS mode allows for **wildcard** domains as well.
```
*.mydomain.com
```
Add the TXT records to the DNS registrar as instructed in the screen.
Then verify the certificate:
```bash
certbot certificates
```

### Dockerize
1. Create a permanent (non `--rm`) container:
    ```bash
    docker container run -it --name letsencrypt -v "/etc/letsencrypt:/etc/letsencrypt" ubuntu:latest
    ```
2. Install and issue the certifications using the instructions above!
3. If you want to start the container again i.e. to renew the certificates:
    ```bash
    docker start -i letsencrypt
    ```

## Appendix A - Sources
- [serverfault - How to use Let's Encrypt DNS challenge validation?](https://serverfault.com/a/812038)
- [Certbot, Installation for Ubuntu 18.04 LTS](https://certbot.eff.org/lets-encrypt/ubuntubionic-other)
- [Certbot, Getting manual certificates](https://certbot.eff.org/docs/using.html#manual)
- [Let’s Encrypt wildcard certificate with Certbot](https://www.nikio.io/infrastructure/lets-encrypt-wildcard-certificate-with-certbot/)
- [Generate Wildcard SSL certificate using Let’s Encrypt/Certbot](https://medium.com/@saurabh6790/generate-wildcard-ssl-certificate-using-lets-encrypt-certbot-273e432794d7)
