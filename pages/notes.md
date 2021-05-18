---
title: Notes
---

## This notes for https://youtu.be/sK5i-N34im8
## title: cgroups, namespaces, and beyond
### High level approach: lightweight vm
#### I can get a shell on it
##### through SSH or otherwise
#### It "feels" like a VM
##### own process space
##### own network interface
##### can run stuff as root
##### can install packages
##### can run services
##### can mess up routing, iptables ...
### Low level approach: chroot on steriods
#### It's not quite like a VM:
##### uses the host kernel
##### can't boot a different OS
##### can't have its own modules
##### doesn't need init as PID 1
##### doesn't need syslogd, cron ...
#### It's just a normal processes on the host machine
##### contrast with VMs witch are opaque
### How are they implemented?
#### Go to LXR
#### Looking for "LXC"  --> zero result
#### Looking for "containers" --> 1000+ results
#### Almost all of them are about data structures, or other unrelated concepts like "ACPI containers"
#### There are some references to "our" containers
##### but only in the documentation.
### cgroup generalities
#### Each subsystem has a hierarchy (tree)
##### separate hierarchies for CPU, memory, block I\/O
#### Hierarchies are independent
##### The trees for e.g. memory and CPU can be different
#### Each process is in a node in each hierarchy
##### Think of each hierarchy as a different dimension or axis
#### Each hierarchy starts with 1 node (the root)
#### Initially, all processes start at the root node
#### Each node = group of processes
##### Sharing the same resources
######
### **Namespaces**
#### Provide processes with their own system view
#### Cgroup = limits how much you can use
#### namespaces = limits what you can see
#### On top of that, you can't use/affect what you can't see
#### Multiple namespaces:
##### pid
###### Processes within a PID namespace only see processes in the same PID namespace
###### Each PID namespace has its own numbering. starting at 1
###### If PID 1 goes away, whole namespace is killed
###### Namespace can be nested
###### A process ends up having multiple PIDs. one per namespace in which its nested.
##### net
###### Processes within a given network namespace get their own private network stack, including:
####### network interfaces (including lo)
####### routing tables
####### iptables rules
####### sockets (ss, netstat)
###### You can move a network interface across netns
####### `ip link set dev eth0 netns <PID>`
######## typeical use-case:
######### use veth pairs (two virtual interfaces acting as a cross-over cable)
######### eth0 in container network namespace paired with vethxxx in host network namespace.
######### all the vethxxx are bridged together
######### (Docker calls the bridge docker0 )
######## But also: the magic --net container
######### shared localhost
##### mnt
###### processes can have their own rootfs, conceptually close to chroot
###### Processes can also have "private" mounts
####### `/tmp` (scoped per user, per service)
####### **masking** of `/proc`, `/sys`
####### NFS auto-mounts
###### Mounts can be totally private, or shared
###### No easy way to pass along a mount from a namespace to another
##### uts
###### To allow the container has its own hostname
##### ipc
###### Allows a process (or group of processes) to have own:
####### IPC semaphores
####### IPC message queues
####### IPC shared memory
###### ... without risk of conflict with other instances
##### user
###### Allows to map UID/GID. e.g.:
####### UID 0 --> 1999 in container C1 is mapped to UID 10000 --> 11999 on host.
###### UID in containers becomes irrelevant
###### Just use UID 0 in the container.
###### It gets squashed to a non-privileged user outside
##### Security improvement
#### Each process is in one namespace of each type
### **Namespace manipulation**
#### Namespaces are created with the `clone()` syscall with extra flags
#### Namespaces are materialized by pseudo-files
##### `/proc/<pid>/ns`
#### When the last process of a namespace exits, it is destroyed.
##### but can be preserved by **bind-mounting** the pseudo-file
#### It's possible to "enter" a namespace with `setns()`
##### exposed by the nsenter wrappter in util-linux
### **Copy-on-Write storage**
#### Create a new container instantly
##### instead of copying its whole file system
#### Storage keeps track of what has changed
#### Many options available
##### AUFS, overlay (file level)
##### Device mapper thinp (block level)
##### BTRFS, ZFS (FS level)
#### Considerably reduces footprint and `boot` times
### **Orthogonality**
#### All those things can be used independently
#### If you only need resource isolation, use a few selected cgroups
#### If you want to simulate a network of routers, use network namespaces
#### Put the debugger in a container's namespaces, but not it's cgroups
##### so that the debugger doesn't steal resources.
#### Setup a network interface in a container, then move it to another
#### etc.
### Some missing bits
#### Capabilites
##### Breakdown "root/non-root" into fine-grained rights allow to keep root, but without the dangerous bits however: **CAP_SYS_ADMIN** remains a big catchall
#### SELinux/AppArmor ...
##### Containers that actually contain
##### deserve a whole talk on their own.
##### check https://github.com/jfrazelle/bane
##### (AppArmor profile generator)
### **Container runtimes using cgroups + namespaces**
#### LXC
##### Set of userland tools
##### A container is a directory in `/var/lib/lxc`
##### Small config file + root filesystem
##### Early version had no support for CoW
##### Early version had no support for move images
##### Requires significant amount of elbow grease
###### easy for sysadmins/ops, hard for devs
#### systemd-nspawn
##### From its manpage:
###### For debugging, testing and building
###### Similar to chroot, but more powerful
###### Implements the Container Interface
##### Seems to position itself as plumbing
##### Support docker images, in a funny way
#### Docker Engine
##### Daemon controlled by REST(ish) API
##### First versions shelled out to LXC
###### now uses it's own runtime
##### Manages containers, images, buils, and more
##### Some people think it does too many things.
#### rkt, runC
##### Back to basics
##### Focus on continer execution
###### no API, no image management, no build, etc.
##### They implement different specifications:
###### rkt implements appc (App Container)
###### runC Implements OCP (Open Container Project)
###### runC leverages Docker's libcontainer
### **Container runtimes using other mechanisms**
#### OpenVZ
##### Also Linux
##### Older, but battle-tested
###### Travis CI gives you root in OpenVZ
##### Tons of neat features too
###### ploop (efficient block device for containers)
###### checkpoint/restore, live migration
###### venet (~more efficient veth)
###### still developed and maintained
#### Jails / Zones
##### FreeBSD / Solaris
##### Coarser granularity of features
##### Strong emphasis on security
##### Great for hosting providers
##### Not so much for developers
###### where's the equivalent of `docker run -it ubuntu`
##### Illumos branded zones can run Linux binaries
##### This is different from the Oracle Solaries port
###### the latter will run Solaris binaries, not Linux binaries.
### Building containers, demo purposes
#### Empty dir
#### `mount --make-rprivate /` to prevent my container file system bleed out
#### `mkdir -p images containers` to build directories for both images and containers
#### `btrfs subvol create images/alpine`
####
## Even if you are not using containers on your machines, you are still in containers. Your whole machine is in a container, but with no limits. If you want to get extra performance by not using containers, you cannot. Even if you don't use containers, you are in a container.
## **TAIL**
### didn't find **cgconfig** command on CentOS
### but found **cgcreate**
#### installed by:
##### `sudo yum install libcgroup-tools`
##### `sudo yum install libcgroup`
#####
