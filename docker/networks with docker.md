# Networks

For security reasons is not a good practice to always give a container access to
all the available features for the OS so we can define what it has access too.
A good example for that is having access to the network.

```terminal
--network none
```

This specification while running a container will disable it's access to the
internet for instance.

## Bridges

A bridge is a piece of software or hardware that forwards traffic between
segments in a network. In docker case we can create custom bridges to implement
things like load balancers to communication between containers.

## Create a custom network

To create a custom network we can use the following

```terminal
docker network create name
```

## Run a container on a network

To run a container on a specific network we must use the following link

```terminal
docker run --network network_name
```

To give a name of a container inside a network we can use the `--name name_you
want`.
