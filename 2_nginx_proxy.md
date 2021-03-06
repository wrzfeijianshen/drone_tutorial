﻿### nginx_proxy 反向代理

docker-compose.yml
```
version: '3.3'

networks:
  nginx_net:
    external: true

services:
  nginx-proxy:
    container_name: nginx-proxy
    image: jwilder/nginx-proxy:alpine
    ports:
      - 80:80
      - 443:443
    networks:
      - nginx_net
    volumes:
      - ./data/proxy_nginx/conf:/etc/nginx/conf.d
      - ./data/proxy_nginx/vhost:/etc/nginx/vhost.d
      - ./data/proxy_nginx/html:/usr/share/nginx/html
      - ./data/proxy_nginx/dhparam:/etc/nginx/dhparam
      - ./data/proxy_nginx/certs:/etc/nginx/certs:ro
      - /var/run/docker.sock:/tmp/docker.sock


  portainer:
    image: portainer/portainer
    container_name: portainer
    ports:
      - 10070:9000
    command: -H unix:///var/run/docker.sock
    restart: always
    networks:
      - nginx_net
    volumes:
      - //var/run/docker.sock:/var/run/docker.sock
      - ./data/portainer/portainer_data:/data
    environment:
      - VIRTUAL_HOST=portainer.dev.fjssoft.com
      - VIRTUAL_PORT=9000

```

![](img_files/13.png)

注意需要在本地添加hosts ,不如访问端口 192.168.0.70:10070

![](img_files/14.png)

