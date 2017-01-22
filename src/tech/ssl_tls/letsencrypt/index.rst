Let's Encrypt
====================================

=============================================
Issue New Certificate
=============================================

old method
----------------------------------

::

[wnoguchi@mx1 ~]$ /usr/local/letsencrypt/letsencrypt-auto certonly --webroot -w /var/www/nw4.io -d docs.pg1x.io
Requesting root privileges to run certbot...
  /home/wnoguchi/.local/share/letsencrypt/bin/letsencrypt certonly --webroot -w /var/www/nw4.io -d docs.pg1x.io
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for docs.pg1x.io
Using the webroot path /var/www/nw4.io for all unmatched domains.
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0018_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0018_csr-certbot.pem

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/docs.pg1x.io/fullchain.pem. Your cert will
   expire on 2017-04-22. To obtain a new or tweaked version of this
   certificate in the future, simply run letsencrypt-auto again. To
   non-interactively renew *all* of your certificates, run
   "letsencrypt-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

