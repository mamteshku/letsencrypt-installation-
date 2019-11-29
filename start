#Install Certbot and obtain a certificate


wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
sudo cp certbot-auto /usr/bin/



sudo /usr/bin/certbot-auto --nginx -d example.com -d www.example.com certonly --debug




c/letsencrypt. The final nginx.conf file should look like the following:


#nginx configuration
# Settings for normal server listening on port 80
server {
  listen       80 default_server;
  listen       [::]:80 default_server;
  server_name  example.com www.example.com;
  root         /usr/share/nginx/html;  # location / {
  # }  # Redirect non-https traffic to https
  if ($scheme != "https") {
    return 301 https://$host$request_uri;
  }
}
# Settings for a TLS enabled server.
server {
  listen       443 ssl http2 default_server;
  listen       [::]:443 ssl http2 default_server;
  server_name  example.com www.example.com;
  root         /usr/share/nginx/html;  ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
  include /etc/letsencrypt/options-ssl-nginx.conf;  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}


#Update security parameters on the website

sudo mkdir /etc/pki/nginx
sudo openssl dhparam -out /etc/pki/nginx/dhparams.pem 2048
sudo nano /etc/nginx/nginx.conf

# Settings for a TLS enabled server.
server {
   .
   .
   .
   .
   ssl_dhparam "/etc/pki/nginx/dhparams.pem";
}

# Settings for a TLS enabled server
server {
   .
   .
   .
   .
   add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
}


#Automatically renew the certificate

sudo /usr/bin/certbot-auto renew --dry-run
sudo cat /var/log/letsencrypt/letsencrypt.log

#Set up certificate auto-renewal
crontab -e
45 00 * * * /usr/bin/certbot-auto renew --quiet --no-self-upgrade --renew-hook "service nginx restart"

#This will run the cron at 45 minutes past midnight every day, every month, every week. Here’s the syntax in more detail:

minute hour day month weekday command
where
minute -> 00 to 59
hour -> 00 to 23
day -> 01 to 31
month -> 01 to 12
weekday -> 0 to 7 (Here 0 or 7 is Sunday)
command -> cron job to run* denotes all possible values, any number can be replaced with a *
