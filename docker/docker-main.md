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

- [container](https://github.com/IgorGrieder/TIL/blob/main/docker/container.md)
- [commands](https://github.com/IgorGrieder/TIL/blob/main/docker/commands.md)
- [images](https://github.com/IgorGrieder/TIL/blob/main/docker/images.md)
- [statefullness](https://github.com/IgorGrieder/TIL/blob/main/docker/statefullness.md)
- [networks with docker](https://github.com/IgorGrieder/TIL/blob/main/docker/networks%20with%20docker.md)
- [load balancers docker](https://github.com/IgorGrieder/TIL/blob/main/docker/load%20balancers%20docker.md)
- [vertical and horizontal scaling](https://github.com/IgorGrieder/TIL/blob/main/docker/vertical%20and%20horizontal%20scaling.md)
- [building images](https://github.com/IgorGrieder/TIL/blob/main/docker/building%20images.md)
- [docker-compose](https://github.com/IgorGrieder/TIL/blob/main/docker/docker-compose.md)
- [running a server](https://github.com/IgorGrieder/TIL/blob/main/docker/running%20a%20server.md)
