# dg2tv.com

> sudo mkdir -p /var/www/dg2tv.com/public_html

> sudo chown -R $USER:$USER /var/www/dg2tv.com/public_html

> sudo chmod -R 755 /var/www/dg2tv.com

> nano /var/www/dg2tv.com/public_html/index.html

``` html
<html>
    <head>
        <title>Welcome to dgtv!</title>
    </head>
    <body>
        <h1>Welcome to dg2tv.com</h1>
    </body>
</html>
```

> sudo nano /etc/nginx/sites-available/dg2tv.com.conf
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/dg2tv.com/public_html;
        index index.html;

        server_name dg2tv.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```


> sudo ln -s /etc/nginx/sites-available/dg2tv.com.conf /etc/nginx/sites-enabled/

> sudo systemctl restart nginx

> nano /lib/systemd/system/dg2tv.service

```
[Unit]
Description=dg2tv.com website
After=network.target

[Service]
Type=simple
Restart=always
RestartSec=5s
WorkingDirectory=/var/www/dg2tv.com/public_html
ExecStart=/var/www/dg2tv.com/public_html/dg2tv

[Install]
WantedBy=multi-user.target
```

> systemctl status dg2tv

> sudo ufw allow 8082/tcp

> sudo nano /etc/nginx/sites-available/dg2tv.com.conf

```
server {

        root /var/www/dg2tv.com/public_html;
        index index.html;

        server_name dg2tv.com;
        client_max_body_size 100G;

        location / {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
                proxy_read_timeout 600;
                proxy_send_timeout 2m;
                proxy_pass  http://127.0.0.1:8082;
        }

        add_header X-Powered-By MATEORS;
}
```

> journalctl -u dg2tv --no-pager -f
