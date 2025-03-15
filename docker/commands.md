# Docker Commands

## docker run

```terminal
docker run -d -p 80:80 docker/getting-started:latest
```

- Tip: To run an container without internet add `--network none`

## docker stop

```terminal
docker stop CONTAINER_ID
```

Stops the container in `SIGTERM` -> signal emitted to stop it in a clear way,
executing some cleaning tasks before shutting down everything in a grace full way, without forcing

### Use full tip

To stop all running containers we can do the following:

```terminal
docker stop $(docker ps -q)
```

- `$()` -> executes a shell command in a sub shell and returns the result to the
  original shell
- `docker ps -q` -> lists all running containers IDs

## docker kill

```terminal
docker kill CONTAINER_ID
```

Stops the container in `SIGKILL` -> signal emitted to stop shutting down in force mode everything

## docker remove

```terminal
docker remove CONTAINER_ID
```

### Tip to remove all containers

```terminal
docker rm $(docker ps --filter status=exited -q)
```

It filters all the status as exited and removes them.

## docker list

```terminal
docker ps
```

List all the current running containers. Add -a to see all, not just the
running one

## docker restart

```terminal
docker restart CONTAINER_ID
```

Restart the container

## docker exec

```terminal
docker exec CONTAINER_ID ls
```

Docker exec will execute a following command in the container environment. For
instance, ls in this case will run a list command inside the container and will
return it's result to our shell.

### Start a shell session

To have a shell section to interact with a container environment we can
proceed the following way

```terminal
docker exec -it CONTAINER_ID /bin/sh
```

- `/bin/sh` -> path to the shell section of the running container
- `-it` -> make it interactive by keyboard

#### command pwd prints the currently working directory in a shell section

## Docker stats

```terminal
docker stats
```

Shows all the current docker running containers information of execution
