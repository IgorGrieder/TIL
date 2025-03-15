# Docker

## Whats Docker?

Docker makes it possible to ship application and they're environment in a
container, that can be thought like a lightweight VM.

## Docker Server/Deamon

Server that actually runs a process in the OS

![[Pasted image 20250315111347.png]]

## Getting Started image

```terminal
docker run -d -p 80:80 docker/getting-started:latest
```

- `run` -> runs the docker/getting-started image
- `-d` -> runs in detached mode (background)
- `-p` -> maps the port 80 of the container to port 80 o the machine

## Topics

- [[container]]
- [[commands]]
- [[images]]
- [[statefullness]]
- [[networks with docker|Networks]]
- [[load balancers docker|Load Balancers]]
- [[vertical and horizontal scaling]]
