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
#####
