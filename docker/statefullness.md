# Statefullness

Docker has kind of an idea to be stateless between container runs. For you to
persist data between containers runs you are supposed to use volumes for it. In
the practical way if you have a container and stop it it will persist any files in the
files system when you run it again, but if you remove the container all data will be lost if
it isn't attached to a volume.

## Volumes

Because of the described behavior above to persist data we use a thing called
volumes, that are independent of `containers`. A place to `store` data when a
container is running, kind of a memory card to make an analogy.

### Create a volume

```terminal
docker volume create volume_name
```

#### Commands to check current volumes

```terminal
docker volume ls
```

```terminal
docker volume inspect volumne_name
```

#### Commands to delete a volume

For the deletion of a volume there must be not a single instance of a container
in our Daemon that uses the volume. By that means if a container is not running but registered in the
docker server we must remove it first to erase the volume.

```terminal
docker volume rm volumne_name
```

## Example of running an image with an volume

```terminal
docker run -d -e NODE_ENV=development -e url=http://localhost:3001 -p 3001:2368 -v ghost-vol:/var/lib/ghost/content ghost
```

- `e NODE_ENV=development` sets an environment variable within the container. This tells Ghost to run in "development" mode (rather than "production", for instance)
- `e url=<http://localhost:3001>` sets another environment variable, this one tells Ghost that we want to be able to access Ghost via a URL on our host machine. We've used -p before. -p 3001:2368 does some port-forwarding between the container and our host machine.
- `ghost-vol:/var/lib/ghost/content` mounts the `ghost-vol` volume that we created before to the /var/lib/ghost/content path in the container. Ghost will use the /var/lib/ghost/content directory to persist stateful data (files) between runs.
