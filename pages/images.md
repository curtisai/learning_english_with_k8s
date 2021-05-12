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
## The concept **container image** that introduced by *Docker* is a standard TAR file that combines:
### **rootfs(container root filesystem)**: A directory on the system that looks like the standard root(/) of the operating system.
### **JSON file(container configuration)**: Specifies how to run the rootfs; for example, what **command** or **entrypoint** to set for the container; the container's **working directory** and a few other things.
### *Docker* tar's up the **rootfs** and the JSON file to create the base image. This enables you to install additional content on the rootfs, create a new JSON file, and tar the different between the original image and the new image with the updated JSON file. This creates a **Layered Image**
## The definition of a container image was eventually standardized by the **Open Container Initiative (OCI)** standards body as the **OCI Image Specification**
