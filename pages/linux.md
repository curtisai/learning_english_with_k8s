---
title: Linux
---

## What do the numbers in a manual page mean. For example, when I type `man ls` I see `LS(1)`
### The number corresponds to what section of the manual that page is from. The man page for man itself (`man man`) explains it and lists the standard ones: ```MANUAL SECTIONS
       The standard sections of the manual include:

       1      User Commands

       2      System Calls

       3      C Library Functions

       4      Devices and Special Files

       5      File Formats and Conventions

       6      Games et. Al.

       7      Miscellanea

       8      System Administration tools and Deamons
## Process Directory
### The configuration for each process is defined within the `/proc` directory. If you know the process ID, then you can identify the configuration directory.
#### eg. ```$ ls /proc/1087
attr       clear_refs       cpuset   fd       limits     mem         net        oom_score      projid_map  sessionid  stat     task
autogroup  cmdline          cwd      fdinfo   loginuid   mountinfo   ns         oom_score_adj  root        setgroups  statm    timers
auxv       comm             environ  gid_map  map_files  mounts      numa_maps  pagemap        sched       smaps      status   uid_map
cgroup     coredump_filter  exe      io       maps       mountstats  oom_adj    personality    schedstat   stack      syscall  wchan```
