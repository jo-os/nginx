# nginx
nginx easy examples

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
