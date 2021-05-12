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
## Docker took these container images(**tarballs**) and moved them to a web service from which they could be pulled, developed a protocol to pull them, and called the web service a **container registry**.
## **Container engines** are programs that can pull container images from container registries and reassemble them onto **container storage**. Container engines also launch **container runtimes**
## ![2021_05_12_image.png](https://cdn.logseq.com/%2Fa9681ad6-bdcb-48f5-9267-58877609cc6be58fa28d-1bbe-4805-9301-0135cbd08fc22021_05_12_image.png?Expires=4774427992&Signature=gky55KIEvr~AK-d0eXQUlYrctRpKvunndCq-ErmsrFoNv3xP6tRDecAhtG8E6MTIub5SLVCUnyi7eO7CSFuAmAJjfyLf3NPsT0qcD8YS4dhLpSup~StazKx8zUJb8Zj1k23pIEF1Gmk3O6hQzW-QkJIgKdBNwukjNVQQq9SiAdk8OYl9J4nJG~X7N8yZ0X688HqHei4dcJ6q7yc2hn1sAI4hbMEkwzjhffdoOxcgD6UCQJKkh3C2ycogl5aw62SfOCRQFOyGKojXeV-DCAIonedN1Oj5Tu45wCz92dBfUOXddvF1EMtvJn8zDyXu1vo8rP6RphuHv8W41XuF1KZjBw__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA){:height 760, :width 674}
## Container storage is usually a **copy-on-write(COW)** layered filesystem. When you pull down a container image from a container registry, you first need to untar the rootfs and place it on dis. If you have multiple layers that make up your image, each layer is downloaded and stored on a different layer on the COW filesystem. The COW filesystem allows each layer to be stored separately, which maximizes sharing for layered images.
## Container engines often support multiple types of container storage. Including **overlay**, **devicemapper**, **btrfs**,
