# Run a server in a container

To run a server inside a container we just must create and build a docker [[building images|image]]
and run the container attaching the ports from the local machine to the
application.

## Benefits

The benefits of dockerizing server are:

- Anyone can run your image
- We can deploy easily on applications that build on top of images, such as
  Kubernets.

## Example of Docker file

This docker file is running a Go application

```Dockerfile
FROM debian:stretch-slim
COPY Docker-steps /bin/goserver
CMD ["/bin/goserver"]
EXPOSE 8080 // Just for making a statemtn, won't expose a prot actually
```

This will copy to the container the go binary and will execute the server in the
command line

## OS

We can specify a OS too by running --platform inside our docker build

## Examples of other languages

Because go doesn't need a compiler to execute code, just the binary it's
Dockerfile kind of differs from other languages that need runtimes, such as
Python, Node.js etc.

To clarify, in this example apt is what we will sue to get dependencies

```Dockerfile
# Build from a slim Debian/Linux image
FROM debian:stable-slim

# Update apt
RUN apt update
RUN apt upgrade -y

# Install build tooling
RUN apt install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev

# Download Python interpreter code and unpack it
RUN wget https://www.python.org/ftp/python/3.10.8/Python-3.10.8.tgz
RUN tar -xf Python-3.10.*.tgz

# Build the Python interpreter
RUN cd Python-3.10.8 && ./configure --enable-optimizations && make && make altinstall

# Copy our code into the image
COPY main.py main.py

# Copy our data dependencies
COPY books/ books/

# Run our Python script
CMD ["python3.10", "main.py"]
```
