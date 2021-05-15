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
####
##### Hierarchies are independent
######
#####
#####
