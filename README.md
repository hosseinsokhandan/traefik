# Traefik
### The simplest ProxyManager that can handle your most complex needs.

#### ⚠️ Only for Development purposes do not use it in PRODUCTION!

#### Configuration Information
- YAML base Static Configuration
- The fully dynamic routing orchestrator configuration
- Use Docker as a Provider
- Docker Provider in an OFF exposition manner


By using this configuration you can blindly deploy mutliple instances of a container in an easy way and the benefit the **AutoDiscovery**, **LoadBalancing** and **ProxyManagement** of Traefik!

## Docker
a single docker-compose.yml

By default, it will expose port **80** and **8080**.<br/>
**8080** is a great dashboard telling you your services and their status plus many useful information.

```sh
docker-compose up -d
```

## Register your containers in Traefik
So far, you have Traefik runnig on port 80 and its Dashboard on port 8080. Now it's time to label your containers in order to be registered in Traefik.

```sh
app:
    build: ...
    restart: ...
    command: ...
    volumes:
      - ...
    expose:
      - "80"
    depends_on:
      - ...
    networks:
      traefik: {}
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.MY_SERVICE.rule=Host(`MY_SERVICE.yourdomain.com`)"
      
networks:
  traefik:
    external: true
    name: traefik_default
```

##### Here we do 3 things:
* Use ```traefik: {}``` network in order to let Traefik get access to your container network.
* ```"traefik.enable=true"``` enables Traefik to discover the container.
* ```"traefik.http.routers.MY_SERVICE.rule=Host(`MY_SERVICE.yourdomain.com`)"``` it tells the Traefik to redirect any request from **MY_SERVICE.yourdomain.com** to this container.
<br/>

> **MY_SERVICE** is an arbitrary name that you want to use as your service name.
