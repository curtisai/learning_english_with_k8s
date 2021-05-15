---
title: Control Group
---

## reference: [redhat manual](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/pdf/resource_management_guide/Red_Hat_Enterprise_Linux-6-Resource_Management_Guide-en-US.pdf)
## The `cgconfig` command mentioned in the manual was not found. `cgcreate` might be a sufficient substitute
## **How Control Groups are Organized**
### Cgroups are organized hierarchically, like processes, and child cgroups inherit some of the attributes of their parents. However, there are difference between the two.
### **The Linux Process Model**
#### All processes on a Linux system are child processes of a common parent: the **init** process, which is executed by the kernel at boot time and starts other processes (which may in turn start child processes of their own).
#### Because all processes descend from a single parent, the Linux process model is a single hierarchy, or tree.
#### Additionally, every Linux process except **init** inherits the environment (such as the *PATH* variable) and certain other attributes (such as open file descriptors) of its parent process.
### **The Cgroup Model**
#### Cgroups are similar to processes in that:
##### they are hierarchical, and
##### child cgroups inherit certain attributes from their parent cgroup
#### The fundamental difference is that many different hierarchies of cgroups can exist simultaneously on a system. If the Linux process
