---
title: Commands I typed
---

## `docker top the_container_name` List the process id(s) in the named container.
### Seem like the process id listed by this command is also the process id you can find by `ps aux | grep entry_point`. Also, use `pgrep entry_point` can also get you the pid.
## Command `pstree` can be used to list process list.
### eg. `pstree -c -p -A $(pgrep dockerd)`
## Process Directory `/proc`
## `ldd` print shared object dependencies
## `chroot` is a feature that allows setting a new path as the root directory for a given program, forcing it to see everything as relative to that path.
## `unshare` can be used to create a new pid namespace
##
