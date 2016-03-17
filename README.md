# letsencrypt.sh-email-notify-hook

Allows letsencrypt.sh to notify you by email when a new DNS record needs to be created. This is useful when your DNS provider has no API and does not support DDNS.

When using this hook, letsencrypt.sh will generate a new certificate wait for you to manually update the challenge record on your provider of choice. If desired, you can schedule letsencrypt.sh using cron, and still be notified when your certificate is about to expire.

For a fully automated solution, consider any of the other hooks [here](https://github.com/lukas2511/letsencrypt.sh/wiki/Examples-for-DNS-01-hooks).

## Setup

```
$ git clone https://github.com/lukas2511/letsencrypt.sh
$ cd letsencrypt.sh
$ git clone https://github.com/kappataumu/letsencrypt-cloudflare-hook hooks/email-notify
```

## Usage

```
$ ./letsencrypt.sh --cron --domain example.com --challenge dns-01 --hook 'hooks/email-notify/hook.sh'
#
# !! WARNING !! No main config file found, using default config!
#
+ Generating account key...
+ Registering account key with letsencrypt...
Processing example.com
 + Signing domains...
 + Creating new directory /home/letsencrypt/src/letsencrypt.sh.staging/certs/example.com ...
 + Generating private key...
 + Generating signing request...
 + Requesting challenge for foobar.alias.bennettp123.com...
 + Settling down for 10s...
```

Check your email and create the TXT record specified. Use the smallest TTL possible.

letsencrypt.sh will wait indefinitely for the TXT record to be created and to propagate (this may take a while):

```
 + DNS not propagated. Waiting 30s for record creation and replication...
 + DNS not propagated. Waiting 30s for record creation and replication...
 + DNS not propagated. Waiting 30s for record creation and replication...
 + Challenge is valid!
 + Requesting certificate...
 + Checking certificate...
 + Done!
 + Creating fullchain.pem...
 + Done!
 
```

## OCSP stapling file

If you're behind a firewall and can only access the internet via proxy, you need to provide a stapling file. email-notify-hook can optionally generate this stapling file after generating the certificate.

This is only needed if direct access to the internet is not available.

To use this, set OCSP_RESPONSE_FILE to the file to store the OCSP response. Optionally, you can also set http_proxy, and the response will be obtained via the proxy specified.

```
$ OCSP_RESPONSE_FILE=/path/to/ocsp.resp http_proxy=http://127.0.0.1:3128 ./letsencrypt.sh --cron --domain example.com --challenge dns-01 --hook 'hooks/email-notify/hook.sh'
```

To enable this in nginx, add the following line to nginx config:
```
ssl_stapling_file /home/letsencrypt/ocsp/ocsp.resp;
```

You should also update the OCSP file regularly (eg daily using cron) to ensure it is valid and up to date.
