# Networks

For security reasons is not a good practice to always give a container access to
all the available features for the OS so we can define what it has access too.
A good example for that is having access to the network.

```terminal
--network none
```

This specification while running a container will disable it's access to the
internet for instance.
