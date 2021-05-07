---
title: Images
---

## A container image is a tar file containing tar files.
## Each of the tar file is a layer.
## Once all tar files have been extract into the same location then you have the container's filesystem.
## `docker save <image_name>  > output.tar` can be used to export an docker image to a tar file
## Since one image is a tar file within which are tar files, we can create an empty image by importing an empty tar file. `tar cv --files-from /dev/null | docker import - <image_name>`
## Also, as long as we can organize tar files by ourselves, a `Dockerfile` is not required when creating container images.
### Creating an image without a Dockerfile
### `curl -LO https://raw.githubusercontent.com/moby/moby/a575b0b1384b2ba89b79cbd7e770fbeb616758b3/contrib/mkimage/busybox-static && chmod +x busybox-static`
### `./busybox-static <dir_name>`
### `tar -C <dir_name> -c . | docker import - <img_name>`
##
