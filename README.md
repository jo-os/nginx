# nginx
nginx easy examples

https://www.youtube.com/watch?v=kTClpNYtfWA

/etc/nginx/sites-enabled/

my-site.com.conf
```
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }
}
```
```
telnet my-site.com 80
---------------
GET / HTTP/1.1
Host: my-site.com
```
https://habr.com/ru/articles/348206/ - приоритет location
```
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }

  location /admin {
    RETURN 200 "ADMIN\n";
  }
}
```
```
telnet my-site.com 80
---------------
GET /admin HTTP/1.1
Host: my-site.com
```
```
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }

  location /admin {
    auth_basic ""Restricted";
    auth_basic_user_file htpasswd;
    try_files NONE @return200;
  }

  location @return200 {
    RETURN 200 "ADMIN\n";
  }
}
```
Пример не провереный
```
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }

  location /admin {
    auth_basic ""Restricted";
    auth_basic_user_file htpasswd;
    root /opt/www;
    try_files /maintenance.html $uri $uri/index.html @app @return200;
    #try_files NONE @return200;
  }

  location @app {
    proxy_pass http://my-backend;
  }

  location @return200 {
    RETURN 200 "ADMIN\n";
  }
}
```
Пример рабочий
```
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }

  location /admin {
    root /opt/www;
    add_header "Cache-Control" "private, no-cashe, no-store" always;
    try_files /maintenance.html $uri $uri/index.html @app @return200;
  }

  location @return200 {
    RETURN 200 "ADMIN\n";
  }
}
```
```
curl -D - -s -H 'Host: my-site.com' http://my-site.com/admin
```

Балансировка
```
upstream backend {
  server ip-1;
  server ip-2;
}
server {
  listen 80;

  server_name my-site.com;

  location / {
    proxy_pass http://backend;
  }
}

```
