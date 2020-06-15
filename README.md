# SocksBin Docker

Docker image for command line pastebin [SocksBin](https://github.com/MagnumDingusEdu/SocksBin) for sharing files and command outputs.

## Usage

#### Clone repo
```
$ git clone https://git.samedamci.me/samedamci/socksbin-docker && cd socksbin-docker
```
#### Build SocksBin Docker image
```
# docker build -t samedamci/socksbin ./socksbin
```
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
