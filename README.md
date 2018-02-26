# traefik-docker-windows-server-2016
Example project showing how to run Traefik in a Windows Docker Container in Windows Server 2016 without the support of bind-mounting named pipes


# Problem

If you want to run Traefik on Windows Server 2016 (not 1709!), you don´t have support for named pipes mounting into your Docker Containers, which is needed for [Traefik](https://traefik.io/) to manage & proxy all your Docker Containers. So this configuration will only work on Windows Server 1709 ff:

```
version: '3.4'

services:
  proxy:
    image: stefanscherer/traefik-windows
    command: --web --docker --docker.domain=tarifrechner.dev.vorsorge.lu --logLevel=WARN --docker.endpoint=npipe:////./pipe/docker_engine
    #command: --web --docker --docker.domain=tarifrechner.dev.vorsorge.lu --logLevel=WARN --docker.endpoint=\\.\pipe\docker_engine
    networks:
      - default
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      #- //./pipe/docker_engine://./pipe/docker_engine
      - \\.\pipe\docker_engine:\\.\pipe\docker_engine
    restart:
      always

    logging:
      driver: "json-file"
      options:
        max-size: "500m"
        max-file: "3"

networks:
  default:
    external:
      name: nat

```
