---
title: Containers
---

## Namespaces
### One of the fundamental parts of a container is namespaces. The concept of namespaces is to limit what processes can see and access certain parts of the system, such as other network interfaces or processes.
When a container is started, the container runtime, such as Docker, will create new namespaces to sandbox the process. By running a process in its own Pid namespace, it will look like it's the only process on the system.
### The available namespaces are:
`Mount (mnt)`
`Process ID (pid)`
`Network (net)`
`Interprocess Communication (ipc)`
`UTS (hostnames)`
`User ID (user)`
`Control group (cgroup)`
### Without using a runtime such as Docker, a process can still operate within its own namespace, using `unshare` is a way
#### eg. `sudo unshare --fork --pid --mount-proc bash`
#### Under the covers, Namespaces are [inode](https://en.wikipedia.org/wiki/Inode) locations on disk. This allows for processes to shared/reused the same namespace, allowing them to view and interact
#### `nsenter --target $PID --mount --uts --ipc --net --pid ps aux` allows you to run command towards a namespace with a provided pid
#### Docker allows you to share a container's namespace with another one.
##### eg. If you have a container called `server`, then share the `net` namespace would be like: `docker run -d --name=web --net=container:server nginx:alpine`. This command will connect the `nginx` with your `server`.
##### In the above case, the following two commands will result in one same `net` namespace for both PIDs.
###### `ls -lha /proc/$PID/ns/ | grep net`
## Chroot
### An important part of a container process is the ability to have different files that are independent of the host. This is how we can have different Docker Images based on different operating systems running on our system.

Chroot provides the ability for a process to start with a different root directory to the parent OS. This allows different files to appear in the root.
