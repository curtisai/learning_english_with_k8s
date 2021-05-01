---
title: Commands I typed
---

## `docker top the_container_name` List the process id(s) in the named container.
### Seem like the process id listed by this command is also the process id you can find by `ps aux | grep entry_point`. Also, use `pgrep entry_point` can also get you the pid.
## Command `pstree` can be used to list process list. eg. `pstree -a`
## ```$ pstree -c -p -A $(pgrep dockerd)
dockerd(684)-+-docker-containe(726)-+-docker-containe(1073)-+-redis-server(1087)-+-{bio_aof_fsync}(1124)
             |                      |                       |                    |-{bio_close_file}(1123)
             |                      |                       |                    |-{bio_lazy_free}(1125)
             |                      |                       |                    `-{jemalloc_bg_thd}(1126)
             |                      |                       |-{docker-containe}(1074)
             |                      |                       |-{docker-containe}(1075)
             |                      |                       |-{docker-containe}(1076)
             |                      |                       |-{docker-containe}(1077)
             |                      |                       |-{docker-containe}(1078)
             |                      |                       `-{docker-containe}(1079)
             |                      |-{docker-containe}(729)
             |                      |-{docker-containe}(730)
             |                      |-{docker-containe}(731)
             |                      |-{docker-containe}(734)
             |                      |-{docker-containe}(735)
             |                      |-{docker-containe}(758)
             |                      |-{docker-containe}(759)
             |                      `-{docker-containe}(1072)
             |-{dockerd}(702)
             |-{dockerd}(704)
             |-{dockerd}(705)
             |-{dockerd}(723)
             |-{dockerd}(727)
             |-{dockerd}(728)
             |-{dockerd}(742)
             |-{dockerd}(743)
             |-{dockerd}(1029)
             `-{dockerd}(1030)```
##
