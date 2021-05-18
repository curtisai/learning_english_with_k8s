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
#########
##### mnt
##### uts
##### ipc
##### user
#### Each process is in one namespace of each type
####
## Even if you are not using containers on your machines, you are still in containers. Your whole machine is in a container, but with no limits. If you want to get extra performance by not using containers, you cannot. Even if you don't use containers, you are in a container.
## **TAIL**
### didn't find **cgconfig** command on CentOS
### but found **cgcreate**
#### installed by:
##### `sudo yum install libcgroup-tools`
##### `sudo yum install libcgroup`
#####
