# nginx

### Installation of Nginx
- Update you system with below command
```
sudo apt update
```
- Now install nginx
  ```
  sudo apt install nginx
  ```

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

### Generate an SSL Certificate with Certbot
- Certbot is a free tool to obtain and install SSL certificates from Let’s Encrypt. Install Certbot:
  ```
  sudo apt install certbot python3-certbot-nginx
  ```
- Run Certbot to obtain the certificate:
  ```
  sudo certbot --nginx -d example.com -d www.example.com
  ``` 
- Certbot sets up a cron job to renew your certificates automatically. You can check the renewal process with:
  ```
  sudo certbot renew --dry-run
  ```
