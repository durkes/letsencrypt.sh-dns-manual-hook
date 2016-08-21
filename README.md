yum install git -y
mkdir /var/letsencrypt
cd /var/letsencrypt
git clone https://github.com/lukas2511/letsencrypt.sh
cd letsencrypt.sh
git clone https://github.com/durkes/letsencrypt.sh-dns-manual-hook hooks/dns-manual
./letsencrypt.sh --cron \
--challenge dns-01 \
--hook 'hooks/dns-manual/hook.sh' \
--domain "\
tld.com \
www.tld.com \
sub.tld.com \
"
