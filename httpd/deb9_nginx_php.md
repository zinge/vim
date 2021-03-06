### Ставим nginx
```
sudo apt-get install nginx
```
проверяем работу, т.е. необходимо в броузере открыть http://localhost

### Ставим postgresql
```
sudo apt-get install postgresql
```

проверяем, т.е.
```
ss -an | grep "5432"
```

### Ставим php
```
sudo apt-get install php-fpm php-pgsql php-xml php-mbstring php-zip
```

дополнительно правим файл /etc/php/7.0/fpm/php.ini
```
cgi.fix_pathinfo=0
```

ну и перезапускаем демона, чтобы наверняка
```
sudo systemctl restart php7.0-fpm.service
```
### Настройка сервера

правим файл /etc/nginx/sites-available/default, и приводим его к виду
```
server {
    listen 80 default_server;
    #listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

проверяем правильность конфига,
```
sudo nginx -t
```

перезагружаем конфиги
```
sudo systemctl reload nginx.service
```

для проверки php, правим файлик /var/www/html/info.php и приводим его к виду
```
<?php
    phpinfo();
```
открываем в броузере, http://localhost/info.php

### все!
настройка закончена
