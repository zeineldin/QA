# This docker-compose file helps you to use SonarQube and secure it via SSL certificate 


# you have to create shared Folder to share your certificate and config files for reverse proxy 


# in vim reverse_proxy/nginx/
```

server {
  listen 80;
  listen [::]:80;
  server_name <add your domain here>;
  return 301 https://$server_name$request_uri;
}
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  client_max_body_size 100M;
  server_name <add your domain here>;
  ssl_certificate /etc/ssl/certs/sonarqube.pem;
  ssl_certificate_key /etc/ssl/private/sonarqube-key.pem;
  access_log /var/log/nginx/sonarqube.access.log;
  location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-SSL on;
    proxy_set_header X-Forwarded-Host $host;
    proxy_pass http://sonarqube:9000;
    proxy_redirect off;
  }
}

```
