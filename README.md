# SocksBin Docker

Docker image for command line pastebin [SocksBin](https://github.com/MagnumDingusEdu/SocksBin) for sharing files and command outputs.

# Usage

#### Clone repo
```
$ git clone https://git.samedamci.me/samedamci/socksbin-docker && cd socksbin-docker
```
#### Build SocksBin Docker image
```
# docker build -t samedamci/socksbin ./socksbin
```
## With NGINX container

#### Build modified NGINX image
```
# docker build -t samedamci/nginx-socksbin ./nginx-socksbin
```
#### Create docker-compose.yml file
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
## With working NGINX instance

#### Create configuration file
```config
server {
	listen 80; # You can also use reverse proxy.
	server_name localhost; # Change to your domain.
	charset utf-8;

	location / {
		root /var/www/socksbin; # You can change it.
		index index.txt index.html;

		location ~* {
			# Required to display pastes as text
			# in browser instead download it.
			add_header Content-Type text/html;
		}
	}
}
```
#### Create docker-compose.yml file
```yaml
version: '3'

services:
  socksbin:
    image: samedamci/socksbin
    volumes:
			# The same location which is
			# set up in the NGINX config.
      - /var/www/socksbin:/pastes
    environment:
      - DOMAIN=localhost # Change to your domain.
    restart: always
    ports:
      - "8801:8888" # Port to push your pastes
			# via netcat - EXTERNAL_PORT:INTERNAL_PORT
```
