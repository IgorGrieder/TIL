# Docker compose

Docker alternative to make it easier to deploy multiple containers at the same
time.

## Creating a docker-compose file

We will use docker-compose.yaml to set our configurations

```docker-compose
services:
  webserver:
    image: nginx:latest or Dockerfile
    ports:
      - "80:80"
    volumes:
      - "volume path in the disk"
    depends_on:
      - app
    restart: "always"
```

## Initialize docker compose

```terminal
docker-compose -d up
```

- link: <https://www.youtube.com/watch?v=D_ha0g9yS2E>
