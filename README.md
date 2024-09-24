# nginx

### Nginx server configuration for multiple server on same domain

```
server {

    listen 80 default_server;

    location / {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    location /blogs/ {
        proxy_pass http://65.0.101.184/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # This prevents internal redirects by WordPress
        rewrite ^/blogs/(.*)$ /$1 break;

        sub_filter '/wp-content/' '/blogs/wp-content/';
        sub_filter '/wp-includes/' '/blogs/wp-includes/';
        sub_filter_once off;

    }
    
}
```

### Nginx basic server configuration for multiple server on same domain
```
server {
    listen 80 default_server;

    location / {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /blogs/ {
        proxy_pass http://localhost:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
