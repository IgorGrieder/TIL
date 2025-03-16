# Building images

Docker can build images automatically by reading the instructions from a Dockerfile.
A Dockerfile is a text document that contains all the commands a user could call
on the command line to assemble an image. It helps with `automation` in our daily
basis.

## Example of Dockerfile

```Dockerfile
FROM debian:stable-slim

CMD ["echo", "hello world"]
```

TO build the container from the image we use

```terminal
docker build . -t helloworld:latest
```

This command means we builded a container called helloworld from the Dockerfile
inside the current dir.

`-t helloworld:latest` tags the image with the name "helloworld" and the "latest"
tag. Names are used to organize your images, and tags are used to keep track of
different versions.
