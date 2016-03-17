# letsencrypt.sh-email-notify-hook

Allows letsencrypt.sh to notify you by email when a new DNS record needs to be created. This is useful when your DNS provider has no API and does not support DDNS.

When using this hook, letsencrypt.sh will generate a new certificate wait for you to manually update the challenge record on your provider of choice. If desired, you can schedule letsencrypt.sh using cron, and still be notified when your certificate is about to expire.

For a fully automated solution, consider any of the other hooks [https://github.com/lukas2511/letsencrypt.sh/wiki/Examples-for-DNS-01-hooks](here).
