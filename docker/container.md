# Containers

Containers are a piece of software, a.k.a a process, that runs in the computer and looks like they
have they're own OS but in most part they share a lot of resources with every
container running on a machine.

Is an instance of an [[image]]. We can use multiple containers for the same
image to `scale` a application.

We can think of containers like a isolated environment with specific configurations to run applications.

For a Cloud company, like AWS is way easier to handle containers than handle VM's,
since multiple containers can run in a single machine using the same OS, instead
of VM's that uses the same hardware but have entirely OS's for each one. The
containers think they have full access to the OS but for most part they use a piece of the OS.

Each running container has a `namespace` in the OS.

![image](imgs/Pasted%20image%2020250315110811.png)
