# Project

This service spawns a wordpress server with a Let's Encrypt certificate.

## Structure

There's an Apache+Wordpress server, and a mysql server. Wordpress listens on port 8080. Letsencrypt listens on port 80 and 443, and proxies all incoming traffic to port 8080 where Apache sits. The MySQL server is not exposed to the outside world.

## Setup

- Edit file `app/docker-compose.yaml` to set appropriate volumes, url for Let's Encrypt and desired database credentials.
- Start the application with `up.sh`.
- Modify the nginx proxy configuration in `<config>/nginx/site-confs/default` in order to redirect all traffic to the Apache server:
  ```
  location / {
      proxy_pass http://domain.com:8080;
      proxy_redirect off;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Host $host;
      proxy_set_header X-Forwarded-Server $host;
      proxy_set_header X-Forwarded-Proto $scheme;
  }
  ```
