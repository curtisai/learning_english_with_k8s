---
title: Commands I typed
---

## `docker top the_container_name` List the process id(s) in the named container.
### Seem like the process id listed by this command is also the process id you can find by `ps aux | grep entry_point`. Also, use `pgrep entry_point` can also get you the pid.
## Command `pstree` can be used to list process list. eg. `pstree -a`
## `pstree -c -p -A $(pgrep dockerd)`
##
