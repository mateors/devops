http://mostain.sheetcoins.com/

http://mostain.sheetcoins.com/

service --status-all

systemctl list-units --type=service

systemctl list-unit-files --state=enabled

apt install net-tools


sudo apt list --installed | grep nginx


sudo apt install nginx

sudo ufw app list

sudo ufw allow 'Nginx Full'


sudo systemctl enable nginx

systemctl start nginx

rm -rf /etc/nginx/conf.d/default.conf


nano /etc/nginx/sites-available/mostain.sheetcoins.com


server {

listen 80; 
listen [::]:80; 

root /var/www/vhosts/mostain.sheetcoins.com/public_html; 
index index.html; 

server_name mostain.sheetcoins.com; 

location / {
 try_files $uri $uri/ =404;
}

}


chmod -R 755 /var/www/vhosts/*

sudo ln -sf /etc/nginx/sites-available/mostain.sheetcoins.com /etc/nginx/sites-enabled/

nginx -t

systemctl restart nginx


sudo apt install certbot python3-certbot-nginx




nano /etc/nginx/sites-available/ariel.sheetcoins.com

server {

listen 80; 
listen [::]:80; 

root /var/www/vhosts/ariel.sheetcoins.com/public_html; 
index index.html; 

server_name ariel.sheetcoins.com; 

location / {
 try_files $uri $uri/ =404;
}

}

mkdir -p /var/www/vhosts/ariel.sheetcoins.com/public_html

nano /var/www/vhosts/ariel.sheetcoins.com/public_html/index.html

sudo ln -sf /etc/nginx/sites-available/ariel.sheetcoins.com /etc/nginx/sites-enabled/

systemctl restart nginx

sudo apt install certbot python3-certbot-nginx

sudo certbot --nginx -d ariel.sheetcoins.com

netstat -tulnp


ssh -i id_ed25519 root@194.163.148.244

scp -i id_ed25519 ariel root@194.163.148.244:/var/www/vhosts/ariel.sheetcoins.com/public_html

ssh -i id_ed25519 root@194.163.148.244

chmod +x ariel


sudo nano /lib/systemd/system/ariel.service


[Unit]
Description=Ariel website
After=network.target

[Service]
Type=simple
Restart=always
RestartSec=5s
WorkingDirectory=/var/www/vhosts/ariel.sheetcoins.com/public_html
ExecStart=/var/www/vhosts/ariel.sheetcoins.com/public_html/ariel

[Install]
WantedBy=multi-user.target


sudo chmod 664 /lib/systemd/system/ariel.service

sudo service ariel status

sudo systemctl enable ariel


nano /etc/nginx/sites-available/ariel.sheetcoins.com

server {
    listen 80;
    server_name ariel.sheetcoins.com;

	location / {
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
          proxy_set_header X-Real-IP $remote_addr;
	  	  proxy_set_header Host $host;
          proxy_pass  http://127.0.0.1:8081;
        }
}

ln -sf /etc/nginx/sites-available/ariel.sheetcoins.com /etc/nginx/sites-enabled/


    #listen [::]:443 ssl ipv6only=on; # managed by Certbot
    #listen 443 ssl; # managed by Certbot
    #ssl_certificate /etc/letsencrypt/live/ariel.sheetcoins.com/fullchain.pem; # managed by Certbot
    #ssl_certificate_key /etc/letsencrypt/live/ariel.sheetcoins.com/privkey.pem; # managed by Certbot
    #include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    #ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


certbot delete --cert-name ariel.sheetcoins.com


certbot --nginx -d ariel.sheetcoins.com


curl -v ariel.sheetcoins.com
