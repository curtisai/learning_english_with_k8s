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
#### The fundamental difference is that many different hierarchies of cgroups can exist simultaneously on a system. If the Linux process model is a single tree of processes, then the cgroup model is one or more separate, unconnected trees of tasks (i.e. processes).
#### Multiple separate hierarchies of cgroups are necessary because each hierarchy is attached to *one or more subsystems* . A subsystem represents a single resource, such as CPU time or memory.
#### **Available Subsystems in RedHat**
##### **blkio** - this subsystem sets limits on input/output access to and from block devices such as physical drives (disk, solid state, or USB).
##### **cpu** - this subsystem uses the scheduler to provide cgroup tasks access to the CPU
##### **cpuacct** - this subsystem generates automatic reports on CPU resources used by tasks in a cgroup.
##### **cpuset** - this subsystem assigns individual CPUs (on a multicore system) and memory nodes to tasks in a cgroup
##### **devices** - this subsystem allows or denies access to devices by tasks in a cgroup.
##### **freezer** - this subsystem suspends or resumes tasks in a cgroup.
##### **memory** - this subsystem
