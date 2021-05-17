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
##### **memory** - this subsystem sets limits on memory use by tasks in a cgroup and generates automatic reports on memory resources used by those tasks.
##### **net_cls** - this subsystem tags network packgets with a class identifier (classid) that allows the Linux traffic controller (**tc**) to identify packets originating from a particular cgroup task.
##### **net_prio** - this subsystem provides a way to dynamically set the priority of network traffic per network interface.
##### **ns** - the *namespace* subsystem
##### **perf_event** - this subsystem identifies cgroup membership of tasks and can be used for performance analysis.
### **NOTE** You may come across the term **resource controller** or simply **controller** in cgroup literature such as the man pages or kernel documentation. Bothe of these terms are synonymous with *subsystem* and arise from the fact that a subsystem typically schedules a resource or applies a limit to the cgroups in the hierarchy it is attached to.
####
### **Relationships Between Subsystems, Hierarchies, Control Groups and Tasks**
#### System processes are called tasks in cgroup terminology.
#### There are rules governing the relationships between subsystems, hierarchies and tasks:
##### **Rule 1** A single hierarchy can have one or more subsystem attached to it. ![2021_05_15_image.png](https://cdn.logseq.com/%2Fa9681ad6-bdcb-48f5-9267-58877609cc6b79fa6ddf-67fc-4952-a312-1be14a6086e92021_05_15_image.png?Expires=4774682541&Signature=TWDRH~P8z7U~ovN1ojTKJ-RRnXwNY2W0J~qmYp2FKM1CR-3RMGm8QOuzYzyGRnfGLw1qUdZgEYVeLXkF98ydVbEY6yFwzLLvUgDQz6tw6GdEuilWOZjC2~F4WSlEW5Y3sbM7IUZ5ipHaHGBzUBeFrLeUHN3J6uCTrU9I4UvQ7QJAlyNnif7u9hmd0HUORBMWBvbZ5iVhqP5gJKN71YvANMD3YB28WjAK4y-8j7YWhb~NeZnqTG0Zp40Z5jtQrlH5uqlmCl5s~AK3MDwX6cewmJGRu36WptHJq7p9G2TFjtw~LmWiWoftC-NgnJkGX0V7EGEonMXs7UZdgrBiIs08FQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
##### **Rule 2** Any single subsystem (such as **cpu**) cannot be attached to more than one hierarchy if one of those hierarchies has a different subsystem attached to it already. ![2021_05_15_image.png](https://cdn.logseq.com/%2Fa9681ad6-bdcb-48f5-9267-58877609cc6bf567c862-32fc-4506-b277-129573147eab2021_05_15_image.png?Expires=4774682697&Signature=heETTrwRcv4pXUMJLl5i0T9wuIADWYaz2Tqz8vuKRkernR1GkJx2C5yeE23jT3pqYG9KQittl9umXYMeB7aaoQdhbjBACqTIdcHFbVgMZZDSEfzDeDWCWHQ9Q2P1jnlsLUIQjwHmJdlQa1oWbY44MjuCbDKoyHWkWDHNMdkhFuHy7fCv88bZoi1~DnlKyc-aY-WXMfvWakYsujE4S7F1Y-Sr6cbafLORiQi5y~QjEXnOQda2ABraanZyerMjLy6sfMLVHyF9LbeyUqTwvuA~SXawvuMBiaziynq4nl1kwS226uTWW0Xh6o91FCm~SFyQcVTW7ngQ~2MLJxoBE1MbgQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
##### **Rule 3** Each time a new hierarchy is created on the systems, all tasks on the system are initially members of the default cgroup of that hierarchy, which is known as the *root cgroup*. For any single hierarchy you create, each task on the system can be a member of *exactly* one cgroup in that hierarchy. A single task may be in multiple cgroups, as long as each of those cgroups is in a different hierarchy. As soon as a task becomes a member of a second cgroup in the same hierarchy, it is removed from the first cgroup in that hierarchy. At no time is a task ever in two different cgroups in the same hierarchy. ![2021_05_15_image.png](https://cdn.logseq.com/%2Fa9681ad6-bdcb-48f5-9267-58877609cc6b003a4bda-d717-4056-8dc3-e5c0e50d59622021_05_15_image.png?Expires=4774683232&Signature=C5Ea0B0Ere9WvbYnEzGmNKVyEq24z7Ae5XtEnQWqcEXKxxWb3VDGY7e6UwOFxNPeOsYeApfx2d3K28P0GBzW0Db6LawA9ViWOickQuXL3Y7blj4mdCg02k~K3GgCoEfcQ1tLUHIPzADj8ksEWHh6KKYbGGJJ4Az6nnG9A2R3MPCMPTVFVHa102ETbYq-kAjrtp12Pl63tIBYM7s3HZRt4hDsS7RR~ze2QnWocZfN1284mqpv7xEO9sbK0wGow6qIokAup6scuivQFXQrdyk8-c3Oia5~rCWrUJNTzNi-DKXtB5lolcZ0B7xQWk9k8TakSXVBDlMVViqec8xn7cDwJA__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
##### **Rule 4** Any process (task) on the system which forks itself creates a child task. A child task automatically inherits the cgroup membership of it's parent but can be moved to a different cgroups as needed. Once forked, the parent and child processes are completely independent. ![2021_05_15_image.png](https://cdn.logseq.com/%2Fa9681ad6-bdcb-48f5-9267-58877609cc6b30ff9469-8549-4e32-a118-ba6fb5574a7e2021_05_15_image.png?Expires=4774683405&Signature=kxIb7e2ZF1BE3B4GX5r6KcN6W14iXNPn6W5MAb7DhPLxiIjsBF0Ez8U-STAnCa0jqZlB117ib4S7vRC-vMWMdkazGVs0tJDPTSJf9V9e87p63JEmaeGUnYUiFycaWZBTw6GO3VBO8trNQThlEWeG-BIlgPglAUi8LjlknTth2w~hCLdmmk~Ok7tKWMFeJtPcJh~40ZjNlFo21v~rBsBqYVEMmJfyUbXcNZFdZFGEZVv4FA6wX38ZKmQLAKe9N7QfAks2erhIWXLe9WC8ZgmMR6vPV-dmenYqDHCnKY4LbXWcrNH3wAoertCkLo92O-ztqaG7hC3AQ-xwO-BJuEx~Bg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
### **Implications for Resource Management**
#### Because a task can belong to only a single cgroup in any one hierarchy, there is only one way that a task can be limited or affected by any single subsystem.
#### You can group several subsystems together so that they affect all tasks in a single hierarchy. Because cgroups in that hierarchy have different parameters set, those tasks will be affected differently.
#### It may sometimes be necessary to *refactor* a hierarchy. Any example would be removing a subsystem from a hierarchy that has several subsystems attached, and attaching it to a new, separate hierarchy.
#### Conversely, if the need for splitting subsystems among separate hierarchies is reduced, you can remove a hierarchy and attach its subsystems to an existing one.
#### The design allows for simple cgroup usage, such as setting a few parameters for specific tasks in a single hierarchy, such as one with just the **cpu** and **memory** subsystems attached.
#### The design also allows for highly specific configuration: each task (process) on a system could be a member of each hierarchy, each of which has a single attached subsystem. Such a configuration would give the system administrator absolute control over all parameters for every single task.
## **Using Control Groups**
### `yum install libcgroup`
### The **cgconfig** service installed with the libcgroup package provides a convenient way to create hierarchies. Attach subsystems to hierarchies, and manage cgroups within those hierarchies.
### The default **/etc/cgconfig.conf** files. You need to restart **cgconfig** if want to applied the changes in the config.
### The default **/etc/config.d/** directory
### `lssubsys -am` List information about given controller(s)
### To remove a hierarchy, ensure that all child cgroups are removed before you **unmount** the hierarchy,
### Or use the **cgclear** to deactivate a hierarchy even when it's not empty.
### The **only way** to remove a task from a cgroup is to move it to a different cgroup.
### Use the **cgcreate** command to create cgroups.
### Use **cgdelete** to remove cgroups
### Use **cgset** to set parameters.
### Use **cgclassify** to move a process to a cgroup.
### Use **cgexec** to start a process in a cgroup.
### To find the cgroup to witch a process belongs, ` ps -O cgroup`
### Or if you know the *PID*, `cat /proc/PID/cgroup`
### **cgget** to display the parameters of specific cgroups
###
