---
title: Containers
---

## The term "**containers**" is heavily overused. Also, depending on the context, it can mean different things to different people.
## Traditional Linux containers are really just ordinary processes on a Linux system. These groups of processes are isolated from other groups of processes using resource
## **Namespaces**
### One of the fundamental parts of a container is namespaces. The concept of namespaces is to limit what processes can see and access certain parts of the system, such as other network interfaces or processes.
When a container is started, the container runtime, such as Docker, will create new namespaces to sandbox the process. By running a process in its own Pid namespace, it will look like it's the only process on the system.
### As said above, namespaces are the keys. Docker is not necessary if you can find a different way to manage the namespaces. Here is an example that uses shell scripts: https://github.com/p8952/bocker
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
#### Under the covers, Namespaces are [inode](https://en.wikipedia.org/wiki/Inode) locations on disk. This allows for processes to shared/reused the same namespace, allowing them to view and interact.
#### `nsenter --target $PID --mount --uts --ipc --net --pid ps aux` allows you to run command towards a namespace with a provided pid
#### Docker allows you to share a container's namespace with another one.
##### eg. If you have a container called `server`, then share the `net` namespace would be like: `docker run -d --name=web --net=container:server nginx:alpine`. This command will connect the `nginx` with your `server`.
##### In the above case, the following two commands will result in one same `net` namespace for both PIDs.
###### `ls -lha /proc/$PID/ns/ | grep net`
## **Chroot**
### An important part of a container process is the ability to have different files that are independent of the host. This is how we can have different Docker Images based on different operating systems running on our system.

Chroot provides the ability for a process to start with a different root directory to the parent OS. This allows different files to appear in the root.
## **cgroup** (Control Groups)
### Control groups limit the amount of resources a process can consume. These cgroups are value defined in a particular files within `/proc` directory.
### To see the mapping, using following command: `cat /proc/<pid>/cgroup`. The content are mapped to other cgroup directories at `ls /sys/fs/cgroup`
### For example, to inspect the CPU stats and CPU shares limit
#### `cat /sys/fs/cgroup/cpu,cpuacct/docker/<container_id>/cupacct.stat`
#### `cat /sys/fs/cgroup/cpu,cpuacct/docker/<container_id>/cpu.shares`
### By default, containers have no limit on the memory. To inspect, `docker stats <container_name/_id>`
### By writing content to the cgroup files, you can modify the resource config for the container.
#### eg. `echo 1024 > /sys/fs/cgroup/memory/docker/$DBID/memory.limit_in_bytes`
## **Seccomp, AppArmor and Capabilities**
### Each of these security features have different purposes. They all function to reduce the damage that a process can cause once it has been compromised.
### [**Seccomp**](https://www.kernel.org/doc/Documentation/prctl/seccomp_filter.txt) is a Linux feature that allows a userspace program to set up syscall filters.
### [**AppArmor**](https://www.apparmor.net/) is a [Mandatory Access Control](https://en.wikipedia.org/wiki/Mandatory_access_control) framework that functions as an LSM(Linux Security Module). It is used to whitelist or blacklist a subject's (program's) access to an object.
### [**Capabilites**](https://man7.org/linux/man-pages/man7/capabilities.7.html) and capability dropping is a general technique whereby a privileged process revokes a subset of the privileges it is endowed with.
### In summary, **Seccomp** reduces the chance that a kernel vulnerability will be successfully exploited. **AppArmor** prevents an application from accessing files it should not access. **Capability dropping** reduces the damage a compromised privileged process can do.
### To view the current AppArmor profiles assigned to a process:
#### `cat /proc/<pid>/attr/current`
#### The default AppArmor profile for Docker is `docker-default (enforce)`
### Default seccomp config for docker can be found at [here](https://github.com/moby/moby/blob/a575b0b1384b2ba89b79cbd7e770fbeb616758b3/profiles/seccomp/default.json)
###
