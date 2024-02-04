# nginx
nginx easy examples

/etc/nginx/sites-enabled/

my-site.com.conf
```conf
server {
  listen 80;

  server_name my-site.com;

  location / {
    RETURN 200 "MY SITE\n";
  }
}
```
