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
### Without using a runtime such as Docker, a process can still operate within it's own namespace
