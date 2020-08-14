# SocksBin Docker

Docker image for command line pastebin [SocksBin](https://github.com/MagnumDingusEdu/SocksBin) for sharing files and command outputs.

## Installation
+ Download image from [GitHub Packages](https://github.com/samedamci/socksbin-docker/packages).

or

+ Clone repo.
```
$ git clone https://git.samedamci.com/samedamci/socksbin-docker && cd socksbin-docker
```
+ Build image itself.
```
# docker build -t samedamci/socksbin ./socksbin
```
## Usage with NGINX container

+ Download modified NGINX image from [GitHub Packages](https://github.com/samedamci/socksbin-docker/packages).

or

+ Build image itself.
```
# docker build -t samedamci/nginx-socksbin ./nginx-socksbin
```
+ Create docker-compose.yml file.
```yaml
version: '3'

services:
  web:
    image: samedamci/nginx-socksbin
    volumes:
     - ./pastes:/usr/share/nginx/html:ro
    ports:
      - "80:80"
    environment:
      - NGINX_HOST=localhost
  socksbin:
    image: samedamci/socksbin
    volumes:
      - ./pastes:/pastes
    environment:
      - DOMAIN=localhost
    restart: always
    ports:
      - "8801:8888"
```
## Usage with working NGINX instance

+ Create configuration file.
```config
server {
  listen 80;
  server_name localhost; # Change to your domain.
  charset utf-8;

  location / {
    root /var/www/socksbin; # You can change it.
    index index.php index.html;

    location ~*_color {
      add_header Content-Type text/html;
      types { } default_type "text/html; charset=utf-8";
      add_header x-robots-tag "noindex, follow";
    }

    location ~* {
      add_header Content-Type text/plain;
      types { } default_type "text/plain; charset=utf-8";
      add_header x-robots-tag "noindex, follow";
    }
  }
}
```
+ Create docker-compose.yml file.
```yaml
version: '3'

services:
  socksbin:
    image: samedamci/socksbin
    volumes:
      - /var/www/socksbin:/pastes
    environment:
      - DOMAIN=localhost
    restart: always
    ports:
      - "8801:8888"
```

+ Run cointainer(s).
```
# docker-compose up
```
If all works well you can run it in detached mode.
```
# docker-compose up -d
```
